---
sql:
  spotify: ./data/spotify-2023.csv
---

---
title: Pergunta 3
---

```js
const year = view(Inputs.range([1990, 2023], {value: 1990, step: 5, label: "Years"}))
const divWidth = Generators.width(document.querySelector("#ex01"));
```

```sql
SELECT * FROM spotify LIMIT 10
```

```sql id=data display
SELECT released_year, SUM(in_spotify_playlists) AS total_in_playlist
FROM spotify
GROUP BY released_year
ORDER BY released_year
```


```js
import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

// const vl = vegaLiteApi.register(vega, vegaLite);


const spec = {
    "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
    "description": "A simple bar chart with tooltips.",
    "width": divWidth-50,
    "height": 500,
    "data": {
        "values": data
    },
    "mark": "bar",
    "encoding": {
        "x": {"field": "released_year", "type": "ordinal", "title": "Ano de Lançamento"},
        "y": {"field": "total_in_playlist", "type": "quantitative", "title": "Total em Playlists", "format": ","},
        "tooltip": [
            {"field": "total_in_playlist", "type": "quantitative", "title": "Total em Playlists", "format": ","}
        ]
    }
};

vegaEmbed('#ex01', spec);



```
<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>

<div class="grid grid-cols-2"> 
    <div id="ex01" class="card grid-colspan-2">
    </div>
</div>
