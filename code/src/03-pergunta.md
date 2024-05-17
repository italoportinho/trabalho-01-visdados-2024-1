---
sql:
  spotify: ./data/spotify-2023.csv
theme: [glacier]
title: Pergunta 3
---
<style> body, div, p, li, ol, h1, h2, h3 { max-width: none; } </style>

<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>

<h1> 3) Discuta as diferenças entre as plataformas Spotify, Deezer, Apple Music e Shazam</h1>
<hr>


<div style="background-color: #f2f2f2; border-left: 6px solid #4CAF50; padding: 10px;">
    <p style="text-align: justify;">
        Para analisar as diferenças entre o Spotify, Deezer, Apple Music e Shazam, é essencial considerar suas funcionalidades específicas e propósitos distintos. É importante observar que o Shazam não  é uma plataforma de streaming de música, mas sim um aplicativo de reconhecimento de músicas. Seu principal objetivo é identificar músicas desconhecidas em reprodução, fornecendo informações sobre título, artista e álbum. Diferenciando-se disso, o Spotify, Deezer e Apple Music são serviços de streaming de música que compartilham a funcionalidade principal de permitir aos usuários ouvir músicas sob demanda. Por consequência dessa informação, a nossa base de dados conta com as colunas "in_playlists" para as plataformas do Spotify, Deezer e Apple Music que indica a quantidade de playlists contendo determinada música dentro dessas plataformas. Em contrapartida, o Shazam não possui essa coluna devido o fato de não fornecer a possibilidade de reproduzir músicas em playlists. Porém uma boa maneira de comparar as 4 plataformas seria analisar o desempenho e popularidade das músicas através da coluna "in_charts", que indica quantas vezes uma determinada música apareceu nos rankings das plataformas, divulgados em intervalos específicos de tempo.
    </p>
</div>
<br>
<div style="background-color: #f2f2f2; border-left: 6px solid #4CAF50; padding: 10px;">
    <p style="text-align: justify;">
        Para iniciar nossa análise, considerando a questão 3 do presente trabalho, vamos levantar duas perguntas auxiliares que nos ajudarão a comparar as plataformas. A primeira pergunta é: "A quantidade de playlists em que uma música está presente influencia sua presença em mais rankings?" Enquanto a segunda é: "Considerando as top x músicas que mais aparecem nos rankings de uma plataforma, elas possuem características acústicas diferentes entre as plataformas?".
        A primeira pergunta pode ser respondida apenas para as plataformas Spotify, Deezer e Apple Music, já que elas possuem a coluna "in_play_lists". Já a segunda pergunta pode incluir a plataforma Shazam.
    </p>
</div>
<br>

<div style="background-color: #f2f2f2; border-left: 6px solid #4CAF50; padding: 10px;">
    <p style="text-align: justify;">      
        Com os filtros abaixo, podemos ajustar o intervalo de datas de lançamento das músicas em nossa base de dados, além de filtrar por plataforma e selecionar as top x músicas mais frequentes nos rankings.
    </p>
</div>



```js
import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";
const db = await DuckDBClient.of({spotify: FileAttachment("data/spotify-2023.csv").csv({})});
```
<hr>
<div id = "yearSelect" style = "display: flex; justify-content: space-between; width: 100%;">

```js
let infYear = view(Inputs.range([1930, 2023], {value: 1930, step: 1, label: "Ano Inferior"}));
```
```js
let supYear = view(Inputs.range([1930, 2023], {value: 2023, step:1, label: "Ano Superior"}));
```

</div>

<div id = "yearSelect" style = "display: flex; justify-content: space-between; width: 100%;">

```js
const platformOptions = ["Spotify", "Apple", "Deezer", "Shazam"];
const platforms = view(Inputs.radio(platformOptions, {value: "Spotify", label: "Plataformas: "}));
const divWidth = Generators.width(document.querySelector("#ex01"));
```

```js
let topx = view(Inputs.range([0, 30], {value: 10, step: 1, label: "Top x charts"}));
```

</div>

```js #Calculo de correlação
const query_corr=`
SELECT source, CORR(playlists, charts) AS correlation
FROM (
    SELECT 'Spotify' AS source, CAST(REPLACE(in_spotify_playlists, ',', '') AS INTEGER) AS playlists, CAST(REPLACE(in_spotify_charts, ',', '') AS INTEGER) AS charts FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    UNION ALL
    SELECT 'Apple' AS source, CAST(REPLACE(in_apple_playlists, ',', '') AS INTEGER) AS playlists, CAST(REPLACE(in_apple_charts, ',', '') AS INTEGER) AS charts FROM spotify 
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    UNION ALL
    SELECT 'Deezer' AS source, CAST(REPLACE(in_deezer_playlists, ',', '') AS INTEGER) AS playlists, CAST(REPLACE(in_deezer_charts, ',', '') AS INTEGER) AS charts FROM 
    spotify 
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
) AS results
WHERE source IN ('${platforms}')
GROUP BY source;
`

let data_corr = await db.query(query_corr);
let coor = data_corr.batches[0].data.children[1].values["0"];
if(coor == undefined){
    coor = 0
}
```


```js #Gráfico de dispersão
const query_scatter= `
SELECT * FROM(
    SELECT 'Spotify' AS source, in_spotify_playlists AS playlists, in_spotify_charts AS charts, track_name, artists_name, released_year, streams
    FROM spotify

    UNION ALL

    SELECT 'Apple' AS source, in_apple_playlists AS playlists, in_apple_charts AS charts, track_name, artists_name, released_year, streams
    FROM spotify

    UNION ALL

    SELECT 'Deezer' AS source, in_deezer_playlists AS playlists, in_deezer_charts AS charts, track_name, artists_name, released_year, streams
    FROM spotify
    ) as results
    WHERE source IN ('${platforms}') 
    AND released_year BETWEEN ${infYear} AND ${supYear};

`

const data_scatter = await db.query(query_scatter);

const graph_scatter = {
    "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
    "description": "",
    "width": "container",
    "height": 500,
    "data": {"values": data_scatter}, 
    "layer": [
        {
            "mark": "point",
            "encoding": {
                "x": {
                    "field": "charts", 
                    "type": "quantitative", 
                    "title": "Inclusão em rankings"
                },
                "y": {
                    "field": "playlists", 
                    "type": "quantitative", 
                    "title": "Inclusão em playlists"
                },
                "color": {
                    "field": "source", 
                    "type": "nominal",
                    "legend": {
                        "title": "Plataforma",
                        "orient": "right"
                    },
                    "scale": {
                        "domain": ["Spotify", "Deezer", "Apple"],
                        "range": ["#1f77b4", "#ff7f0e", "#2ca02c"]
                    }
                },
                "tooltip": [
                    {"field": "source", "type": "nominal", "title": "Plataforma"},
                    {"field": "charts", "type": "quantitative", "title": "Inclusão em rankings"},
                    {"field": "playlists", "type": "quantitative", "title": "Inclusão em playlists"},
                    {"field": "artists_name", "type": "nominal", "title": "Nome do Artista"},
                    {"field": "track_name", "type": "nominal", "title": "Nome da Música"},
                    {"field": "released_year", "type": "quantitative", "title": "Ano de lançamento"},
                    {"field": "streams", "type":"quantitative", "title":"Streams no spotify"}
                ]
            }
        },
        {
            "mark": {
                "type": "text",
                "align": "left",
                "baseline": "top",
                "dx": 5,
                "fontSize": 16
            },
            "encoding": {
                "x": {"value": divWidth-300},  
                "y": {"value": 0},  
                "text": {"value": `Correlação: ${coor.toFixed(3)}`},  
                
            },
        }
    ]
};

vegaEmbed('#ex01', graph_scatter)
```

<h3>A quantidade de playlists em que uma música está presente influencia sua presença em mais rankings?</h3>

<div class="grid grid-cols-1"> 
    <div id="ex01" class="card grid-colspan-1"></div>
</div>

<div style="background-color: #f2f2f2; border-left: 6px solid royalblue; padding: 10px;">
    <p style="text-align: justify;">   
    Para essa análise, optamos pelo uso do gráfico de dispersão devido à sua capacidade de visualizar a relação entre duas variáveis quantitativas, "in_playlists" e "in_charts", de maneira clara e intuitiva. Esse tipo de gráfico nos permite representar as observações da nossa base de dados por marcadores de pontos, facilitando a identificação de padrões ou tendências.
    Para melhorar a distinção entre as diferentes plataformas, decidimos mapeá-las utilizando cores distintas, o que permite uma fácil associação entre os pontos e suas respectivas fontes de dados. Além disso, como complemento à visualização do gráfico de dispersão, calculamos o coeficiente de correlação entre as variáveis e o apresentamos em conjunto com o gráfico, oferecendo uma medida quantitativa da relação entre "in_playlists" e "in_charts".
    </p>
</div>

<br>

<div style="background-color: #f2f2f2; border-left: 6px solid #4CAF50; padding: 10px;">
    <p style="text-align: justify;">      
    Podemos observar que ao considerar o período completo dos dados, abrangendo os anos de 1930 a 2023, as colunas "in_playlists" e "in_charts" apresentaram uma correlação geralmente baixa para as três plataformas: Spotify, Apple Music e Deezer. Notavelmente, a Apple Music se destaca com uma correlação de 0.415. Um aspecto a se notar neste intervalo de tempo é que tanto o Spotify quanto o Deezer mostram uma quantidade significativa de músicas com alta presença em playlists porém uma presença relativamente baixa nos rankings. Isso sugere que essas plataformas podem não considerar a popularidade das músicas em playlists de usuários ao criar seus rankings.
    Ao aproximar o limite inferior dos anos de lançamento das músicas para anos mais rescentes, observamos uma mudança nas correlações. Para o Spotify e o Deezer, a correlação tende a aumentar, enquanto para a Apple Music ela diminui. Isso sugere que essas duas plataformas podem preferir destacar músicas mais recentes em seus rankings. Por exemplo, ao filtrar para o ano de 2023, a correlação do Deezer atinge 0.802, a do Spotify é de 0.497 e a da Apple Music é de 0.408. Esses insights podem indicar que a relação entre a inclusão das músicas em playlists dos usuários e a presença nos rankings varia ao longo do tempo e entre as diferentes plataformas de música
    </p>
</div>




```js

// const query2 = `
// SELECT * FROM (
//     (SELECT 'Spotify' AS source, COALESCE(CAST(NULLIF(REPLACE(in_spotify_charts, ',', ''), '') AS INTEGER), 0) AS charts, track_name, artists_name, released_year,
//            bpm, key, mode, "danceability_%" AS danceability, "valence_%" AS valence, 
//            "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
//            "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
//     FROM spotify
//     ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_spotify_charts, ',', ''), '') AS INTEGER), 0) DESC
//     LIMIT ${topx})

//     UNION ALL

//     (SELECT 'Apple' AS source, COALESCE(CAST(NULLIF(REPLACE(in_apple_charts, ',', ''), '') AS INTEGER), 0) AS charts, track_name, artists_name, released_year,
//            bpm, key, mode, "danceability_%" AS danceability, "valence_%" AS valence, 
//            "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
//            "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
//     FROM spotify
//     ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_apple_charts, ',', ''), '') AS INTEGER), 0) DESC
//     LIMIT ${topx})

//     UNION ALL

//     (SELECT 'Deezer' AS source, COALESCE(CAST(NULLIF(REPLACE(in_deezer_charts, ',', ''), '') AS INTEGER), 0) AS charts, track_name, artists_name, released_year,
//            bpm, key, mode, "danceability_%" AS danceability, "valence_%" AS valence, 
//            "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
//            "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
//     FROM spotify
//     ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_deezer_charts, ',', ''), '') AS INTEGER), 0) DESC
//     LIMIT ${topx})

//     UNION ALL

//     (SELECT 'Shazam' AS source, COALESCE(CAST(NULLIF(REPLACE(in_shazam_charts, ',', ''), '') AS INTEGER), 0) AS charts, track_name, artists_name, released_year,
//            bpm, key, mode, "danceability_%" AS danceability, "valence_%" AS valence, 
//            "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
//            "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
//     FROM spotify
//     ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_shazam_charts, ',', ''), '') AS INTEGER), 0) DESC
//     LIMIT ${topx})
// ) AS results
// WHERE source IN ('${platforms}') 
// AND released_year BETWEEN ${infYear} AND ${supYear}; 


// `

// const dataGraph2 = await db.query(query2);
// view(Inputs.table(dataGraph2));


const query_boxplot = `
SELECT source, 
       CAST(danceability AS INTEGER) AS Dançabilidade,
       CAST(valence AS INTEGER) AS Valência,
       CAST(energy AS INTEGER) AS Energia,
       CAST(acousticness AS INTEGER) AS Acústica,
       CAST(instrumentalness AS INTEGER) AS Instrumentalidade,
       CAST(liveness AS INTEGER) AS Vivacidade,
       CAST(speechiness AS INTEGER) AS Vocalidade,
       released_year
FROM (
    (SELECT 'Spotify' AS source, "danceability_%" AS danceability, "valence_%" AS valence, 
           "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
           "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_spotify_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})

    UNION ALL

    (SELECT 'Apple' AS source, "danceability_%" AS danceability, "valence_%" AS valence, 
           "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
           "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_apple_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})

    UNION ALL

    (SELECT 'Deezer' AS source, "danceability_%" AS danceability, "valence_%" AS valence, 
           "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
           "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_deezer_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})

    UNION ALL

    (SELECT 'Shazam' AS source, "danceability_%" AS danceability, "valence_%" AS valence, 
           "energy_%" AS energy, "acousticness_%" AS acousticness, "instrumentalness_%" AS instrumentalness, 
           "liveness_%" AS liveness, "speechiness_%" AS speechiness, released_year
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_shazam_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})
) AS aggregated_data
WHERE source IN ('${platforms}') 
`

const data_boxplot = await db.query(query_boxplot);

const graph_boxplot = {
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "description": "Average audio properties by source",
  "width": "container",
  "height": 500,
  "data": {
    "values": data_boxplot 
  },
  "transform": [
    {
      "fold": ["Dançabilidade", "Valência", "Energia", "Acústica", "Instrumentalidade", "Vivacidade", "Vocalidade"],
      "as": ["Audio Property", "Average"]
    }
  ],
  "mark": "boxplot",
  "encoding": {
    "x": {
      "field": "Audio Property", 
      "type": "nominal",
      "title": "Propriedades da música"
    },
    "y": {
      "field": "Average",
      "type": "quantitative",
      "title": "Valor(%)"
    },
    "color": {
    "field": "source", 
    "type": "nominal",
    "legend": {
        "title": "Plataforma",
        "orient": "right"
    },
    "scale": {
        "domain": ["Spotify", "Deezer", "Apple", "Shazam"],
        "range": ["#1f77b4", "#ff7f0e", "#2ca02c", "#ffff05"]
    }
    },
    // "tooltip": [
    //   {"field": "source", "type": "nominal", "title": "Plataforma"},
    //   {"field": "Audio Property", "type": "nominal", "title":"Propriedade da música"},
    //   {"field": "Average", "type": "quantitative", "title": "Valor", "format": ".2f"}
    // ]
  }
}

vegaEmbed('#ex02', graph_boxplot)


// FAZER O GRAFICO DE INTERSECÇÃO AQUI
const query4 = `
SELECT track_name, 
       artists_name,
       CAST(streams AS INT64) streams,
       COUNT(DISTINCT source) AS platform_count,
       STRING_AGG(DISTINCT source, ', ') AS platforms
FROM (
    (SELECT 'Spotify' AS source, track_name, released_year, artists_name, streams
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_spotify_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})

    UNION ALL

    (SELECT 'Apple' AS source, track_name, released_year, artists_name, streams
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_apple_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})

    UNION ALL

    (SELECT 'Deezer' AS source, track_name, released_year, artists_name, streams
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_deezer_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})

    UNION ALL

    (SELECT 'Shazam' AS source, track_name, released_year, artists_name, streams
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    ORDER BY COALESCE(CAST(NULLIF(REPLACE(in_shazam_charts, ',', ''), '') AS INTEGER), 0) DESC
    LIMIT ${topx})
) AS results
GROUP BY track_name, artists_name, streams
HAVING COUNT(DISTINCT source) > 1
ORDER BY platform_count DESC, track_name;
`

const dataGraph4 = await db.query(query4);
view(Inputs.table(dataGraph4));
```


<!-- ```js
const query1 = `SELECT * FROM(
    SELECT released_year, 'Apple' AS source, SUM(CAST(in_apple_playlists AS INT)) AS total
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    GROUP BY released_year

    UNION ALL

    SELECT released_year, 'Spotify' AS source, SUM(CAST(in_spotify_playlists AS INT)) AS total
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    GROUP BY released_year

    UNION ALL

    SELECT released_year, 'Deezer' AS source, SUM(CAST(REPLACE(in_deezer_playlists, ',', '') AS INT)) AS total
    FROM spotify
    WHERE released_year BETWEEN ${infYear} AND ${supYear}
    GROUP BY released_year


)as results
WHERE source IN ('Apple')
ORDER BY released_year, source;
`

const data = await db.query(query1);


const spec = {
    "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
    "description": "Um gráfico de barras estacadas com percentuais para Spotify, Deezer e Apple.",
    "width": "container",
    "height": 500,
    "data": {
        "values": data
    },
    "transform": [
        {
            "aggregate": [
                {
                    "op": "sum",
                    "field": "total",
                    "as": "total_per_source_year"
                }
            ],
            "groupby": ["released_year", "source"]
        },
        {
            "window": [
                {
                    "op": "sum",
                    "field": "total_per_source_year",
                    "as": "total_per_year"
                }
            ],
            "frame": [null, null],
            "groupby": ["released_year"]
        },
        {
            // Calcula a percentagem que cada 'source' contribui no ano respectivo
            "calculate": "datum.total_per_source_year / datum.total_per_year",
            "as": "percentage"
        }
    ],
    "mark": "bar",
    "encoding": {
        "x": {"field": "released_year", "type": "ordinal", "title": "Ano de Lançamento"},
        "y": {
            "field": "percentage",
            "type": "quantitative",
            "title": "Percentual em Playlists",
            "stack": "normalize",
            "axis": {"format": "%"}
        },
        "color": {
            "field": "source",
            "type": "nominal",
            "title": "Plataforma",
            "scale": {
                "domain": ["Spotify", "Deezer", "Apple"],
                "range": ["#1f77b4", "#ff7f0e", "#2ca02c"]
            }
        },
        "tooltip": [
            {"field": "released_year", "type": "ordinal", "title": "Ano de Lançamento"},
            {"field": "source", "type": "nominal", "title": "Plataforma"},
            {"field": "total_per_source_year", "type": "quantitative", "title": "Total em Playlists", "format": ","},
            {"field": "percentage", "type": "quantitative", "title": "Percentual", "format": ".1%"}
        ]
    },
    "selection": {
        "select": {
            "type": "single",
            "on": "click",
            "fields": ["released_year"],
            "empty": "none"
        }
    }
};

vegaEmbed('#ex02', spec).then((result) => {
    const view = result.view;

    // Cria um observador no sinal da seleção
    view.addSignalListener('select', function(name, value) {
        if (value) {
            console.log('Ano selecionado:', value.released_year);
        }
    });
});
``` -->




<div class="grid grid-cols-1"> 
    <div id="ex02" class="card grid-colspan-1"></div>
</div>

