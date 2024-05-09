---
theme: dashboard
title: Testes
toc: false
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>

# Testes de carregamento de arquivo e gráficos

<!-- Load and transform the data -->

```js
const dataset = await FileAttachment("data/spotify-2023.csv").csv({typed: true});
display(dataset.slice(0,10));
display(dataset.length);
```

- Analisar o top 20 e ver características em comum:
```js
dataset.sort((a, b) => (a.streams < b.streams ? 1 : -1));
display(dataset.slice(0,10));
```
Ordenamos o array como dataset pela propriedade "streams", e mostramos os 10 primeiros elementos:
```
dataset.sort((a, b) => (a.streams < b.streams ? 1 : -1));
display(dataset.slice(0,10));
```
# Top 10 das músicas (dataset ordenado por streams)

<div id="vis" style="width: 100%; margin-top: 15px;">    
    ${ vl.render(teste_streams()) }
</div>

```js
const divWidth = Generators.width(document.querySelector("#vis"));

```
```js
import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);

function teste_streams(){
    return {
        spec: {
            data: {
                values: dataset.sort((a, b) => (a.streams < b.streams ? 1 : -1)).slice(0 ,50)
            },
            mark: "bar",
            encoding: {
                y: {
                    field: "streams",
                    type: "quantitative",
                    title: "Número de streams até 2023"
                },
                x: {
                    field: "track_name",
                    title: "Música",
                    sort: null
                }
            }
        }
    }
}
```
