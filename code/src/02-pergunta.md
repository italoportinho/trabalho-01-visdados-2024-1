---
title: Pergunta 2
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>


# 2) O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas musicas lançadas no mesmo ano?
<hr>

## Total de lançamentos por ano

<div class="grid grid-cols-1">
  <div class="card" id="total_musicas_ano">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart2(lancamentos_por_ano, "Total de músicas lançadas em cada ano", "Ano", "Ano", "Musicas", "Total de lançamentos")) }
  </div>  
</div>

## Ano relevante: 2022
* ### Top 10 de artistas por streams
<div class="grid grid-cols-1">
  <div class="card" id="total_musicas_ano">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart2(top_10_artistas_2022, "Total de streams no ano", "Artista", "Artista", "total_streams", "Total de streams")) }
  </div>  
</div>

* ### Do top 10 de artistas, o total de músicas lançadas no ano. Quantas músicas para chegar ao top 10?

<div class="grid grid-cols-1">
  <div class="card" id="count_musicas_artistas">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart2(artists_array.slice(0, 10), "Músicas por Artista", "artists_name", "Artista", "musicas", "Total de músicas")) }
  </div>  
</div>

* ### Top 10 músicas no ano. Alguma música dos artistas anteriores?

<div class="grid grid-cols-1">
  <div class="card" id="chart_top_10_musicas_2022">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart2(top_10_musicas_2022, "Top 10 músicas - 2022", "track_name", "Música", "streams", "Streams")) }
  </div>  
</div>

```js
// Carregamos o dataset.
let dataset = await FileAttachment("data/spotify-2023.csv").csv({typed: true});
const db = await DuckDBClient.of({spotify: FileAttachment("data/spotify-2023.csv").csv({typed: true})});

const top_10_musicas_geral = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2023 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2023' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2022 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2022' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2021 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2021' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2020 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2020' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2019 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2019' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2018 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2018' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2017 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2017' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2016 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2016' ORDER BY streams DESC LIMIT 10`;
const top_10_musicas_2015 = await db.sql`SELECT track_name, artists_name, streams, released_year FROM spotify WHERE released_year = '2015' ORDER BY streams DESC LIMIT 10`;

//view(Inputs.table(top_10_musicas_geral));
//view(Inputs.table(top_10_musicas_2022));

const top_10_artistas_geral = await db.sql`SELECT artists_name, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL GROUP BY artists_name ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2023 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2023' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2022 = await db.sql`SELECT artists_name as "Artista", released_year as "Ano", sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2022' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2021 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2021' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2020 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2020' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2019 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2019' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2018 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2018' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;

//view(Inputs.table(top_10_artistas_2022));

let artists_array = [];
for (let i=0; i<dataset.length; i++) {
    if(dataset[i].released_year == 2022){
        let artist_name = dataset[i].artists_name;
        let achou = false;
        for(let j=0;j<artists_array.length;j++){
            if(artists_array[j].artists_name == artist_name){
                artists_array[j].musicas++;
                artists_array[j].streams = artists_array[j].streams + dataset[i].streams;
                achou = true;
            }
        }
        if(!achou){
            let new_artist = { artists_name: dataset[i].artists_name, musicas: 1, streams: dataset[i].streams };
            artists_array.push(new_artist);
        }
    }    
}
//console.log(artists_array.sort((a, b) => (a.musicas < b.musicas ? 1 : -1)));
artists_array = artists_array.sort((a, b) => (a.streams < b.streams ? 1 : -1));
console.log(artists_array.slice(0,10));

//view(Inputs.table(top_10_musicas_2022));

/* ------------------------------------------------------------- */

//view(Inputs.table(top_10_artistas_2023));
const artists_music_count_2023 = await db.sql`
    SELECT 
        artists_name, released_year, count(1) as musicas 
    FROM spotify 
    WHERE 
        streams is NOT NULL AND released_year = 2023
    GROUP BY  artists_name,  released_year
    ORDER BY musicas DESC LIMIT 10`;
//view(Inputs.table(artists_music_count_2023));

/* ------------------------------------------------------------- */

//view(Inputs.table(top_10_artistas_2021));
const artists_music_count_2021 = await db.sql`
    SELECT 
        artists_name, released_year, count(1) as musicas 
    FROM spotify 
    WHERE 
        streams is NOT NULL AND released_year = 2021
    GROUP BY  artists_name,  released_year
    ORDER BY musicas DESC LIMIT 10`;
//view(Inputs.table(artists_music_count_2021));

/* ------------------------------------------------------------- */

const lancamentos_por_ano = await db.sql`
    SELECT 
         released_year as "Ano", count(1) as "Musicas" 
    FROM spotify 
    WHERE 
        streams is NOT NULL 
    GROUP BY  released_year
    ORDER BY musicas DESC`;

function bar_chart2(data_array, titulo, campo_x, titulo_x, campo_y, titulo_y){
  return {
    spec: {
      data: {
          values: data_array
      },
      width: "800",
      mark: "bar",
      title: titulo,
      encoding: {
        x: {
            field: campo_x,
            title: titulo_x,
            sort: null
        },
        y: {
            field: campo_y,
            type: "quantitative",
            title: titulo_y
        }                
      }
    }
  }
}

```