---
title: O que são Containers
date: 2022-12-11 18:00:00
categories: [architecture]
tags: [go, golang, grpc, microsservice, pt]
---

# O que são Containers

- Um container é um padrão de unidade de software que empacota código e todas as dependências de uma aplicação fazendo que a mesma seja executada rapidamente de forma confiável de um ambiente computacional para o outro.
- Quando falamos de Cointaners, estamos falando de processos **ISOLADAS**. Esses processos isolados possuem **PROCESSOS FILHOS**. Se o processo principal for encerrado, os processos filhos são encerrados também.
- Anatomia de um processos:
    - Pid: como se fosse o id do processo;
    - User: Posso fazer a segregação de usuários por namespace;
    - Network: Posso isolar processos dentro do mesmo network;
- Bases do Docker:
    - **Namespaces**: ****Isola os processos, mas não os recursos.
    - **Cgroups** (criado pelo Google): Serve para controlar os recursos computacionais (memory e cpu_shares - parte do CPU, por exemplo) do container, evitando que um container use mais recursos do que deveria usar, evitando leak of memory da máquina;
    - **File System**: Quando trabalhamos no contexto de Docker, trabalhamos com Overlay File System (OFS). A grande sacada é quando precisamos gerar uma nova imagem, vamos gerar apenas da DIFERENÇA, o resto se mantém igual, o que transforma a criação de imagens muito mais rápido. Com OFS, trabalhamos com layers, ou seja, camadas.
- Imagens:
    - Ficam dentro de um “Image Registry”, um repositório para as imagens;
    - São criadas a partir de camadas;
    - Caso eu tenha um erro em uma camada, não preciso baixar tudo novamente, apenas a camada afetada;
    - As imagens possuem nomes e é boa prática colocar uma versão;
    - Imagens são definidas pelo Dockerfile;
    - A imagem é IMUTÁVEL, porém quando rodamos o Docker temos uma camada de leitura e escrita. Quando você derrubar o container, essa camada de read/write será apagada;
    - Toda vez que eu rodar um Dockerfile, cria-se uma nova imagem;
- Dockerfile
    - Arquivo declarativo usado para construir imagens. Caso não precise criar uma imagem, você não precisa do dockerfile.
