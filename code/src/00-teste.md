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