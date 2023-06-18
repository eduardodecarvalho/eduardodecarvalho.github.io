---
title: Fundamentos sobre Observabilidade
date: 2023-01-07 12:00:00
categories: [engineering]
tags: [observability, security, Kibana, Elasticsearch, Logstash]
---
# O que é Observabilidade?

> *“Na teoria de controle, a observabilidade é definida como uma medida de quão bem os estados internos de um sistema podem ser inferidos a partir do conhecimento das saídas externas desse sistema. Simplificando, observabilidade é quão bem você pode entender seu sistema complexo.”*
> 

https://newrelic.com/blog/best-practices/what-is-observability

Podemos deduzir que, no momento que conseguimos medir os *outputs* do nosso sistema, podemos saber o que está ocorrendo dentro do meu sistema, podemos criar métricas para entender o estado do nosso sistema e saber se temos problemas ou não. Observabilidade é a forma com a qual conseguimos analisar as saídas do nosso sistema.

## Observabilidade x Monitoramento

- Observabilidade se baseia no “porquê” algo está certo ou errado;
- Monitoramento tem como função mostrar que algo está errado;
- Monitoramento se baseia em saber com antecedência quais sinais você deseja monitorar;

## Os três pilares da Observabilidade

- **Métricas:** são os números que irão nos guiar, podendo ser:
    - Métricas técnicas: Quantidade de memória usada, velocidade das requests;
    - Métricas de negócio: Quantos acessos tivemos, quantas vendas, quantos cancelamentos;
- **Logs:** diferente das métricas, os logs nos mostram o resultado de um determinado evento;
- **Tracing:** garante a ordenação dos eventos. Nos ajuda a contextualizar os logs;

## ELK Stack

- Elasticsearch
    - Search engine e analytics;
    - Os dados tem índice;
    - Ferramenta de pesquisa de dados **extremamente** rápida;
    - Possui uma API Rest: tudo que fazemos é via uma interface, podendo usar os métodos Rest;
    - Consegue trabalhar com dados geoespaciais (longitude e latitude);
    - Podemos fazer application, website e enterprise search;
    - Logging e analytics: Podemos jogar logs no elastic pois ele é extremamente rápido para encontrar os registros;
    - Trabalha de forma distribuída através de ***shards*** que possuem redundância de dados. Vamos criando várias máquinas e replicando os índices nas outras máquinas;
    - Pode escalar milhares de servidores e manipular ***petabyte*** de dados.
- Logstash
    - Engine coletora de dados em tempo real;
    - Recebe os dados de múltiplas fontes;
    - Iniciou como manipulador de logs. Serve para transformar (normalizar) os dados para melhor se encaixar ao Elasticsearch;
    - Processador de dados através de pipelines que consegue receber, transformar e enviar dados simultaneamente;
    - Envia dados para múltiplas fontes;
    - Vem sendo usada cada vez **menos**;
- Kibana
    - Permite aos usuários visualizarem os dados do elasticsearch em diversas perspectivas;
    - Integração com Machine Learning para verificar anomalias;
    - Consigo agregar e filtrar dados;
    - Logs, métricas, APM, Uptime, Segurança;
    - Fleet é algo que está em Beta, mas é a ideia de utilizar apenas um Beat;
    
    ## Diferença entre ELK e Elastic Stack?
    

A maior diferença entre os dois é algo que chamamos de **Beats.** Mas o que é um beat?

- Projeto da Elastic anunciado em 2015;
- Ele é definido como **“lightweight data shipper”**, que seria “um entregador de dados leve”;
- Cada dia temos mais fontes de dados e de diferentes formas e a ELK centralizava essa função no Logstash, porém o Logstash tem um custo muito alto de manutenção, pois precisa ter seu próprio servidor.
- Visando substituir o **Logstash**, foi lançado o **Beat**, que é um agente coletor de dados;
- O Beat pode jogar os dados para o **Logstash** ou diretamente para o **Elasticsearch**, o que é o mais comum atualmente.
- Temos diferentes tipos de **Beats**: Logs, métricas, network data, audit data e uptime monitoring;
- A tendência é que no futuro tenha todas essas funções acima citadas sejam unificadas no mesmo **Beat**;
- Você pode construir o seu próprio **Beat, ou seja**, é altamente customizável;
- Quando falamos de Elastic Stack, estamos falando em:
    - Coleta de dados: Beats e/ou Logstash
    - Armazenamento dos dados: Elasticsearch;
    - Visualização dos dados: Kibana;
