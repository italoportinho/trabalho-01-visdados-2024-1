---
title: Pergunta 2
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>


# 2) O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas musicas lançadas no mesmo ano?
<hr>

```js
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
const top_10_artistas_2022 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2022' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2021 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2021' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2020 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2020' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2019 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2019' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
const top_10_artistas_2018 = await db.sql`SELECT artists_name, released_year, sum(streams) as total_streams FROM spotify WHERE streams is NOT NULL AND released_year = '2018' GROUP BY artists_name, released_year ORDER BY total_streams DESC LIMIT 10`;
view(Inputs.table(top_10_artistas_2022));
view(Inputs.table(top_10_artistas_2021));
```