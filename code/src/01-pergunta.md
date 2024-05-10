---
theme: dashboard
title: Pergunta 1
toc: false
---
<style> body, div, p, li, ol, h1 { max-width: none; } </style>

# 1) Existe alguma característica que faz uma música ter mais chance de se tornar popular?
<hr>

## Total de músicas lançadas por mês:

<div class="grid grid-cols-3">
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;">Dataset Completo</span>  
      ${ vl.render(teste_streams(months_array_completo)) }
  </div>  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;">Dataset 25%</span>  
      ${ vl.render(teste_streams(months_array_25)) }
  </div>  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;">Dataset 25% - 50%</span>  
      ${ vl.render(teste_streams(months_array_50)) }
  </div>  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;">Dataset 50% - 75%</span>  
      ${ vl.render(teste_streams(months_array_75)) }
  </div>  
  <div class="card" id="vis_completo">  
      <span style="font-size: 80%;">Dataset 75% - 100%</span>  
      ${ vl.render(teste_streams(months_array_100)) }
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

/*
*
*/
const vl = vegaLiteApi.register(vega, vegaLite);

function teste_streams(data_array){
    return {
        spec: {
            data: {
                values: data_array
            },
            mark: "bar",
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
                }
            }
        }
    }
}

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

