---
title: Pergunta 2
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>


# 2) O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas musicas lançadas no mesmo ano?
<hr>

```js
const db = await DuckDBClient.of({spotify: FileAttachment("data/spotify-2023.csv").csv({typed: true})});

const top_10_musicas = await db.sql`SELECT released_year, COUNT(1) as total_musicas FROM spotify GROUP BY released_year ORDER BY released_year DESC LIMIT 100`;
view(Inputs.table(top_10_musicas));

const top_10_artistas = await db.sql`SELECT artists_name, sum(streams) as total FROM spotify WHERE streams is NOT NULL GROUP BY artists_name ORDER BY total DESC LIMIT 10`;
view(Inputs.table(top_10_artistas));
```