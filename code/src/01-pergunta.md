---
title: Pergunta 1
theme: [glacier,dashboard]
toc: false
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>

<h1> 1) Existe alguma característica que faz uma música ter mais chance de se tornar popular?</h1>
<hr>

```js
let divWidth = 670;
```

<div style="background-color: #f2f2f2; border-left: 6px solid #4CAF50; padding: 10px;">
    <p style="text-align: justify;">
        Nesta primeira seção do trabalho vamos explorar os atributos do dataset com diversas visualizações para verificar se algum valor específico, ou faixa de valor, desses atributos tem alguma influência relevante na posição da música no conjunto. Serão apresentadas visualizações para o mês de lançamento, o dia de lançamento, BPM, para todas as propriedades percentuais, e também para as propriedades musicais(tom da música). Exceto pela visualização de  dia de lançamento, todas usam o dataset já ordenado pelo número de streams.
    </p>
</div>
<br>

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

<div style="background-color: #f2f2f2; border-left: 6px solid royalblue; padding: 10px;">
    <p style="text-align: justify;">   
        Essa conjunto de visualizações apresenta o número de lançamentos por mês, como dataset ordenado por streams em ordem crescente e divido em intervalo quantil. Foi utilizado o gráfico de barra, pois cada mês seria um "pote" aonde os lançamentos seriam colocados, possibilitando uma comparação quantitativa visual direta entre os diferentes intervalos. Primeiramente é apresentado o gráfico com o dataset completo, e é verificado que os meses de janeiro e maio tem os maiores números de lançamentos, sendo os únicos acima de 100. Em seguida temos 4 visualizaçãoes, uma para cada quartil do dataset começando pelo primeiro e o mês de maio se destaca no primeiro, segundo e terceiro, e o mês de janeiro se destaca no quarto, que é o intervalo mais relevante para nosso estudo pois é o que contém as  músicas com os maiores números de visualizações. 
    </p>
</div>
<br>

<hr>

## Heatmap

<div class="grid grid-cols-1">
  <div class="card" id="chart_heatmap">   

      <!-- ${ vl.render(heatmap(heatmap_data)) } -->
  </div>  
</div>

<div style="background-color: #f2f2f2; border-left: 6px solid royalblue; padding: 10px;">
    <p style="text-align: justify;">   
        Nessa visualiação apresentamos uma visualização em forma de matriz, o mapa de calor, da forma mês(eixo y) por dia do mês(eixo x). Quanto mais lançamentos um dia do mês tiver, mais escura será a cor do quadrante e se destacam os dias 6 de maio e 1º de janeiro, o que corrobora o achado das visualizações de gráfico de barra que já apontavam esses meses como os com maior número de lançamentos de músicas que alcançaram as posições mais altas no dataset ordenado. 
    </p>
</div>
<br>

<hr>

## BPM das músicas:

<div class="grid grid-cols-1">
  <div class="card" id="chart_dataset_bpm">        
      ${ vl.render(line_chart(dataset, "BPM Top 50 músicas", "streams", "streams", "bpm", "BPM da música")) }      
  </div>
</div>
<div style="background-color: #f2f2f2; border-left: 6px solid royalblue; padding: 10px;">
    <p style="text-align: justify;">   
        Para analizar o bpm(batidas por minuto) será utilizado um gráfico de linha com pontos. Nele podemos verificar que na metade inferior da escala de streams, o bpm das músicas oscila bastante entre valores altos e baixos. Porém da metade pra cima, e mais especificamente na parte mais alta da escala de streams (aonde estão as  músicas mais populares), podemos constatar  que média do valor de bpm mantém um valor bem estável, tendendo à valores entre 150 e 200. Podemos verificar também que músicas com bpm maior que 200 estão associdas à metade menor da escala de streams, bem como músicas com bpm inferior à 100.
    </p>
</div>
<br>


## Propriedades percentuais
<div class="grid grid-cols-1">
  <div class="card" id="multiline_chart">         
      ${ vl.render(multiline_chart(dataset)) }
  </div>  
</div>
<div style="background-color: #f2f2f2; border-left: 6px solid royalblue; padding: 10px;">
    <p style="text-align: justify;">   
        Aqui é utilizado um gráfico multilinha para explorar os atributos que são apresentados com um valor percentual. São eles: danceability(o quão dançante é  a música), valence (positividade da música), energy (nível de energia associado à música), acousticness (sons acústicos na música), instrumentalness (proporção de instrumental na música), liveness (presença de elementos de gravação ao vivo na música) e speechiness (presença de vocais na música). Aqui vamos analizar as duas pontas do gráfico para verificar se um valor alto ou baixo de uma propriedade influencia no sucesso da música, e também vamos ver se alguma propriedade é mais relevante que outra.         
    </p>
    <ul style="max-width: none;">
      <li>
        <p style="text-align: justify;">   
          <span style="font-weight: 700;">danceability:</span> as músicas com mais streams estão associadas com valores acima de 65, próximos a 70, no entanto as músicas com menos streams também apresentam esse comportamento. 
        </p>
      </li>
      <li>
        <p style="text-align: justify;">   
          <span style="font-weight: 700;">valence:</span> essa  é uma propriedade que mostra uma relevância de maiores valores para alcançar o sucesso. As músicas com mais streams apresentam valores entre 60 e 65, enquanto as com menos streams tem valores um pouco acima de 50.
        </p>
      </li>
      <li>
        <p style="text-align: justify;">   
          <span style="font-weight: 700;">energy:</span> as músicas com mais streams apresentam valores acima de 70, enquanto as com menos streams estão estávei em torno de 65.
        </p>
      </li>
      <li>
        <p style="text-align: justify;">   
          <span style="font-weight: 700;">acousticness:</span> tanto as músicas com menos streams quanto as com mais strams apresentam valores próximo de 30, no entanto na parte média do gráfico tem um grande aumento de valor, mais próximo à 40.
        </p>
      </li>
      <li>
        <p style="text-align: justify;">   
          <span style="font-weight: 700;">instrumentalness:</span> essa parece ser uma propriedade irrelevante, todo o conjunto apresenta valores próximo à 0.
        </p>
      </li>
      <li>
        <p style="text-align: justify;">   
          <span style="font-weight: 700;">liveness:</span> dois terços do conjunto apresentam valores próximos à 20, no entanto as músicas mais populares apresentam valores abaixo de 10. 
        </p>
      </li>
      <li>
        <p style="text-align: justify;">   
          <span style="font-weight: 700;">speechiness:</span> a maior parte do conjunto apresenta valor estável próximo de 8, no entanto as músicas com menos streams apresentam valores acima de 10.
        </p>
      </li>
    </ul>
    
</div>
<br>


## Propriedades musicais
<div class="grid grid-cols-1">
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;"></span>  
      ${ vl.render(bar_chart2(musical_data, "Streams por tom musical", "streams", "Streams", "tom_da_musica", "Tom musical")) }
  </div>    
</div>
<div style="background-color: #f2f2f2; border-left: 6px solid royalblue; padding: 10px;">
    <p style="text-align: justify;">   
        Para essa visualização, os atributos key e mode foram concatenados para formar o tom da música e totalizamos o número de streams para cada tom e apresentamos em ordem crescente em um gráfico de barra, para comparação direta. O tom C# Maior (Dó SUstenido Maior) é de longe o mais comum, seguido por Ré Maior, Sol sustenido Maior e Sol Maior. A primeira tonalidade menor surge apenas na quinta posição. Podemos notar também que da primeira para a segunda posição a tonalidade varia apenas um semitom, bem como da terceira para a quarta posição, ou seja bem pouco.
    </p>
</div>


```js
import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";
import embed from "npm:vega-embed";


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
//display(heatmap_data);
//view(Inputs.table(heatmap_data));

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
          width: divWidth,
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
      width: divWidth*2,
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
      width: divWidth*2,
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
      width: divWidth*2,
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
    width: divWidth*2 - 50,
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


 embed("#chart_heatmap", graph_heatmap)

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

