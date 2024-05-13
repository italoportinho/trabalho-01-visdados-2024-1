---
sql:
  spotify: ./data/spotify-2023.csv
title: Pergunta 3
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>

# 3) Discuta as diferenças entre as plataformas Spotify, Deezer, Apple Music e Shazam.
<hr>

```js
import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";
const db = await DuckDBClient.of({spotify: FileAttachment("data/spotify-2023.csv").csv({})});
```

<h>Selecione o intervalo das datas</h>
<div id = "yearSelect" style = "display: flex; justify-content: space-between; width: 100%;">

```js
let infYear = view(Inputs.range([1930, 2023], {value: 1930, step: 1, label: "Ano Inferior"}));
```
```js
let supYear = view(Inputs.range([1930, 2023], {value: 2023, step:1, label: "Ano Superior"}));
```

</div>


```js
const platformOptions = ["Spotify", "Apple", "Deezer"];
const platforms = view(Inputs.radio(platformOptions, {value: "Spotify", label: "Plataformas: "}));
```


```js
console.log(infYear)
const query1= `
SELECT * FROM(
    SELECT 'Spotify' AS source, in_spotify_playlists AS playlists, in_spotify_charts AS charts, track_name, artists_name, released_year
    FROM spotify

    UNION ALL

    SELECT 'Apple' AS source, in_apple_playlists AS playlists, in_apple_charts AS charts, track_name, artists_name, released_year
    FROM spotify

    UNION ALL

    SELECT 'Deezer' AS source, in_deezer_playlists AS playlists, in_deezer_charts AS charts, track_name, artists_name, released_year
    FROM spotify
    ) as results
    WHERE source IN ('${platforms}') 
    AND released_year BETWEEN ${infYear} AND ${supYear};

`

const dataGraph1 = await db.query(query1);
view(Inputs.table(dataGraph1));

const graph1 = {
    "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
    "description": "A scatterplot showing chart positions and playlist inclusions for various platforms.",
    "width": "container",
    "height": 500,
    "data": {"values": dataGraph1},  // Ensure dataGraph1 has columns 'source', 'playlists', 'charts'
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
                "title": "Plataforma"
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
            
        ]
    }
}

vegaEmbed('#ex01', graph1)
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

<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>

<div class="grid grid-cols-1"> 
    <div id="ex01" class="card grid-colspan-1"></div>
</div>

<div class="grid grid-cols-1"> 
    <div id="ex02" class="card grid-colspan-1"></div>
</div>
