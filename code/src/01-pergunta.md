---
title: Pergunta 1
theme: [glacier,dashboard]
toc: false
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>
<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>

# 1) Existe alguma característica que faz uma música ter mais chance de se tornar popular?
<hr>

## Total de músicas lançadas por mês:

<div class="grid grid-cols-2">
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart(months_array_completo, "Dataset Completo")) }
  </div>  
</div>
<div class="grid grid-cols-2">  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart(months_array_25, "Dataset 25%")) }
  </div>  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart(months_array_50, "Dataset 25% - 50%")) }
  </div>  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart(months_array_75, "Dataset 50% - 75%")) }
  </div>  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart(months_array_100, "Dataset 75% - 100%")) }
  </div>  
</div>

<hr>

## Heatmap

<div class="grid grid-cols-1">
  <div class="card" id="chart_heatmap">   

      <!-- ${ vl.render(heatmap(heatmap_data)) } -->
  </div>  
</div>

<hr>

## BPM das músicas:

<div class="grid grid-cols-1">
  <div class="card" id="chart_dataset_bpm">        
      ${ vl.render(line_chart(dataset, "BPM Top 50 músicas", "streams", "streams", "bpm", "BPM da música")) }      
  </div>
</div>

## Propriedades percentuais
<div class="grid grid-cols-1">
  <div class="card" id="multiline_chart">         
      ${ vl.render(multiline_chart(dataset)) }
  </div>  
</div>

## Propriedades musicais
<div class="grid grid-cols-1">
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart2(musical_data, "Streams por tom musical", "streams", "Streams", "tom_da_musica", "Tom musical")) }
  </div>    
</div>

```js
import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";
//const divWidth = Generators.width(document.querySelector("#vis_completo"));

/*
*
*/

// Carregamos o dataset.
let dataset = await FileAttachment("data/spotify-2023.csv").csv({typed: true});
// Ordenamos o dataset por streams, de forma CRESCENTE.
dataset = dataset.sort((a, b) => (a.streams > b.streams ? 1 : -1));

// É criado um array de objetos com a estrutura para totalizar os lançamentos por mês
let months_array_25 = [
  { mes: "Janeiro", lancamentos: 0} ,
  { mes: "Fevereiro", lancamentos: 0} ,
  { mes: "Março", lancamentos: 0} ,
  { mes: "Abril", lancamentos: 0} ,
  { mes: "Maio", lancamentos: 0} ,
  { mes: "Junho", lancamentos: 0} ,
  { mes: "Julho", lancamentos: 0} ,
  { mes: "Agosto", lancamentos: 0} ,
  { mes: "Setembro", lancamentos: 0} ,
  { mes: "Outubro", lancamentos: 0} ,
  { mes: "Novembro", lancamentos: 0} ,
  { mes: "Dezembro", lancamentos: 0} 
];
let months_array_50 = [
  { mes: "Janeiro", lancamentos: 0} ,
  { mes: "Fevereiro", lancamentos: 0} ,
  { mes: "Março", lancamentos: 0} ,
  { mes: "Abril", lancamentos: 0} ,
  { mes: "Maio", lancamentos: 0} ,
  { mes: "Junho", lancamentos: 0} ,
  { mes: "Julho", lancamentos: 0} ,
  { mes: "Agosto", lancamentos: 0} ,
  { mes: "Setembro", lancamentos: 0} ,
  { mes: "Outubro", lancamentos: 0} ,
  { mes: "Novembro", lancamentos: 0} ,
  { mes: "Dezembro", lancamentos: 0} 
];

let months_array_75 = [
  { mes: "Janeiro", lancamentos: 0} ,
  { mes: "Fevereiro", lancamentos: 0} ,
  { mes: "Março", lancamentos: 0} ,
  { mes: "Abril", lancamentos: 0} ,
  { mes: "Maio", lancamentos: 0} ,
  { mes: "Junho", lancamentos: 0} ,
  { mes: "Julho", lancamentos: 0} ,
  { mes: "Agosto", lancamentos: 0} ,
  { mes: "Setembro", lancamentos: 0} ,
  { mes: "Outubro", lancamentos: 0} ,
  { mes: "Novembro", lancamentos: 0} ,
  { mes: "Dezembro", lancamentos: 0} 
];

let months_array_100 = [
  { mes: "Janeiro", lancamentos: 0} ,
  { mes: "Fevereiro", lancamentos: 0} ,
  { mes: "Março", lancamentos: 0} ,
  { mes: "Abril", lancamentos: 0} ,
  { mes: "Maio", lancamentos: 0} ,
  { mes: "Junho", lancamentos: 0} ,
  { mes: "Julho", lancamentos: 0} ,
  { mes: "Agosto", lancamentos: 0} ,
  { mes: "Setembro", lancamentos: 0} ,
  { mes: "Outubro", lancamentos: 0} ,
  { mes: "Novembro", lancamentos: 0} ,
  { mes: "Dezembro", lancamentos: 0} 
];

let months_array_completo = [
  { mes: "Janeiro", lancamentos: 0} ,
  { mes: "Fevereiro", lancamentos: 0} ,
  { mes: "Março", lancamentos: 0} ,
  { mes: "Abril", lancamentos: 0} ,
  { mes: "Maio", lancamentos: 0} ,
  { mes: "Junho", lancamentos: 0} ,
  { mes: "Julho", lancamentos: 0} ,
  { mes: "Agosto", lancamentos: 0} ,
  { mes: "Setembro", lancamentos: 0} ,
  { mes: "Outubro", lancamentos: 0} ,
  { mes: "Novembro", lancamentos: 0} ,
  { mes: "Dezembro", lancamentos: 0} 
];

months_array_25 = popula_months_array(months_array_25, dataset.slice(0, 237));
months_array_50 = popula_months_array(months_array_50, dataset.slice(238, 475));
months_array_75 = popula_months_array(months_array_75, dataset.slice(476, 715));
months_array_100 = popula_months_array(months_array_100, dataset.slice(716, 952));
months_array_completo = popula_months_array(months_array_completo, dataset);

const db = await DuckDBClient.of({spotify: FileAttachment("data/spotify-2023.csv").csv({typed: true})});

const heatmap_data = await db.sql`
CREATE TEMP TABLE all_dates (
    minha_date VARCHAR,
    streams_total INT DEFAULT 0
);

INSERT INTO all_dates (minha_date)
SELECT
    CONCAT(m, '-', d) AS minha_date
FROM
    (SELECT * FROM (VALUES (1),(2),(3),(4),(5),(6),(7),(8),(9),(10),(11),(12)) AS m(m)),
    (SELECT * FROM (VALUES (1),(2),(3),(4),(5),(6),(7),(8),(9),(10),(11),(12),(13),(14),(15),(16),(17),(18),(19),(20),(21),(22),(23),(24),(25),(26),(27),(28),(29),(30),(31)) AS d(d));

SELECT 
    all_dates.minha_date, 
    COALESCE(LOG10(SUM(CAST(s.streams_total AS INT64))), 0) AS log_streams_total,
    COALESCE(SUM(CAST(s.streams_total AS INT64)), 0) AS streams_total
FROM 
    all_dates
LEFT JOIN (
    SELECT 
        CONCAT(released_month::INTEGER, '-', released_day::INTEGER) AS minha_date,
        streams::BIGINT AS streams_total
    FROM 
        spotify 
    WHERE 
        streams IS NOT NULL 
) AS s ON all_dates.minha_date = s.minha_date
GROUP BY 
    all_dates.minha_date
ORDER BY 
    all_dates.minha_date;

DROP TABLE all_dates;


`;
display(heatmap_data);
view(Inputs.table(heatmap_data));

const musical_data = await db.sql`
  SELECT 
    concat(key, ' ', mode ) as tom_da_musica, 
    sum(streams)::LONG as streams
  FROM spotify WHERE streams is NOT NULL AND key is not null GROUP BY tom_da_musica ORDER BY streams DESC`;
//view(Inputs.table(musical_data));

/*
*
*/
const vl = vegaLiteApi.register(vega, vegaLite);

function bar_chart(data_array, titulo, campo_x, titulo_x, campo_y, titulo_y){
    return {
        spec: {
          width: "360",
            data: {
                values: data_array
            },
            mark: "bar",
            title: titulo,
            encoding: {
                y: {
                    field: "lancamentos",
                    type: "quantitative",
                    title: "Músicas lançadas no mês"
                },
                x: {
                    field: "mes",
                    title: "Mês de Lançamento",
                    sort: null
                },
                tooltip: [
                  {field: "lancamentos", type: "quantitative", title: "Lançamentos"}
                ],                
            }
        }
    }
}

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
            field: "tom_da_musica",
            title: "Tom da música",
            sort: null
        },
        y: {
            field: "streams",
            type: "quantitative",
            title: "Total de streams"
        }                
      }
    }
  }
}

function line_chart(data_array, titulo, campo_x, title_x, campo_y, title_y){
  return {
    spec: {
      width: "800",
      data: {
          values: data_array
      },
        mark: {
          type: "line",
          point: true
      },
      title: titulo,
      encoding: {
          y: {
              field: campo_y,
              type: "quantitative",
              title: title_y
          },
          x: {
              field: campo_x,
              title: title_x,
              aggregate: "mean",
              sort: "asc"
          },
          
      }
    }
  }
}

function multiline_chart(data_array){
  return {
    spec: {
      "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
      width: "800",
      height: "400",
      data: {
        values: data_array
      },
      layer: [
        {
          mark: "line",
          encoding: {
            x: { bin: true, field: "streams", type: "quantitative" },
            y: {
              aggregate: "mean", field: "danceability_%", type: "quantitative", title: "Média do valor da propriedade(%)"
            },
            color: { datum: "danceability_%", "type": "nominal"},
          },
          name: "child_layer_danceability_%"
        },
        {
          mark: "line",
          encoding: {
            x: { bin: true, field: "streams", type: "quantitative" },
            y: {
              aggregate: "mean", field: "valence_%", type: "quantitative", title: "Média do valor da propriedade(%)"
            },
            color: { datum: "valence_%", "type": "nominal"},
          },
          name: "child_layer_valence_%"
        },
        {
          mark: "line",
          encoding: {
            x: { bin: true, field: "streams", type: "quantitative" },
            y: {
              aggregate: "mean", field: "energy_%", type: "quantitative", title: "Média do valor da propriedade(%)"
            },
            color: { datum: "energy_%", "type": "nominal"},
          },
          name: "child_layer_energy_%"
        },
        {
          mark: "line",
          encoding: {
            x: { bin: true, field: "streams", type: "quantitative" },
            y: {
              aggregate: "mean", field: "acousticness_%", type: "quantitative", title: "Média do valor da propriedade(%)"
            },
            color: { datum: "acousticness_%", "type": "nominal"},
          },
          name: "child_layer_acousticness_%"
        },
        {
          mark: "line",
          encoding: {
            x: { bin: true, field: "streams", type: "quantitative" },
            y: {
              aggregate: "mean", field: "instrumentalness_%", type: "quantitative", title: "Média do valor da propriedade(%)"
            },
            color: { datum: "instrumentalness_%", "type": "nominal"},
          },
          name: "child_layer_instrumentalness_%"
        },
        {
          mark: "line",
          encoding: {
            x: { bin: true, field: "streams", type: "quantitative" },
            y: {
              aggregate: "mean", field: "liveness_%", type: "quantitative", title: "Média do valor da propriedade(%)"
            },
            color: { datum: "liveness_%", "type": "nominal"},
          },
          name: "child_layer_liveness_%"
        },
        {
          mark: "line",
          encoding: {
            x: { bin: true, field: "streams", type: "quantitative" },
            y: {
              aggregate: "mean", field: "speechiness_%", type: "quantitative", title: "Média do valor da propriedade(%)"
            },
            color: { datum: "speechiness_%", "type": "nominal"},
          },
          name: "child_layer_speechiness_%"
        },
      ]    
    }
  }
}


console.log(heatmap_data.batches[0].data.children[0]);


const graph_heatmap = {
    width: "800",
    height: "268",
    "data": { values: heatmap_data},
    "title": "Soma de streams por dia de lançamento",
    "config": {
        "view": {
            "strokeWidth": 0,
            "step": 13
        },
        "axis": {
            "domain": false
        }
    },
    "mark": "rect",
    "encoding": {
        "x": {
            "field": "minha_date",
            "timeUnit": "date",
            "type": "ordinal",
            "title": "Dia",
            "axis": {
                "labelAngle": 0,
                "format": "%e"
            }
        },
        "y": {
            "field": "minha_date",
            "timeUnit": "month",
            "type": "ordinal",
            "title": "Mês"
        },
        "color": {
            "field": "log_streams_total",
            "type": "quantitative",
            "legend": {
                "title": "Número de streams em log",
                "format": ",.0s"
            }
        },
        "tooltip": [
            {
                "field": "streams_total", 
                "type": "quantitative", 
                "title": "Total do número de streams",
                "format":","
            },
            {
                "field": "log_streams_total", 
                "type": "quantitative", 
                "title": "Log total do número de streams",
                "format":",.3s"
            },
        ]
    }
}


 vegaEmbed("#chart_heatmap", graph_heatmap)

function popula_months_array(months_array, dataset){
  // Iteramos no dataset para extrair os lançamentos por mês e popular o array.
  for(var i=0;i<dataset.length;i++){
    switch(dataset[i].released_month){
      case 1: 
        months_array[0].lancamentos++; 
        break;
      case 2:
        months_array[1].lancamentos++;
        break;
      case 3:
        months_array[2].lancamentos++;
        break;
      case 4:
        months_array[3].lancamentos++;
        break;
      case 5:
        months_array[4].lancamentos++;
        break;
      case 6:
        months_array[5].lancamentos++;
        break;
      case 7:
        months_array[6].lancamentos++;
        break;
      case 8:
        months_array[7].lancamentos++;
        break;
      case 9:
        months_array[8].lancamentos++;
        break;
      case 10:
        months_array[9].lancamentos++;
        break;
      case 11:
        months_array[10].lancamentos++;
        break;
      case 12:
        months_array[11].lancamentos++;
        break;
      default:
        //do nothing
        break;
    }
  }
  return months_array;
}
```

