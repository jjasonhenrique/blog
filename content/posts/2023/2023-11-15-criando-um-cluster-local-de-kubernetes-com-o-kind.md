---
title: Criando um cluster local de kubernetes com o kind
author: Jason
date: 2023-11-15
tags: [ "containers" ]
type: post
weight: 2
showTableOfContents: true
---
O objetivo desse artigo é fazer um passo-a-passo de como subir um
cluster kubernetes localmente com o kind para que você possa fazer algum
teste ou estudar sobre kubernetes.

Lembrando que esse tutorial está focado na criação de um cluster
kubernetes local executando em uma máquina Linux (No meu caso uma
distrubuição Debian rs).

## Instalção Docker 

Primeiro passo é a instalação do Docker para conseguir subir o kind.
Para efetuar a instalação do Docker devemos executar o comando abaixo:

``` 
curl -fsSL https://get.docker.com | sh
```

Para verificar se o Docker foi instalado com sucesso executar o comando
abaixo:

``` 
docker 
```

## Instalação kubectl 

Devemos fazer a instalação do utilitário de linha de comando kubectl
para conseguir administrar o cluster kubernetes via linha de comando.
Abaixo segue os comandos que devem ser executados:

``` 
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
```

Para validar a instalação do kubectl executar o comando abaixo:

``` 
kubectl
```

## Instalação kind 

Para a instalação do kind devemos executar os comandos abaixo:

``` 
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/
```

Para a verificação da instalação do kind executar o comando abaixo:

``` 
kind 
```

## Criação cluster k8s 

Agora vamos fazer a criação de um cluster kubernetes localmente
executando o kind. Vou fazer a criação do cluster de forma declarativa
usando um arquivo yaml conforme abaixo:

``` 
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: nome-do-cluster
runtimeConfig:
  "api/alpha": "false"
nodes:
- role: control-plane
- role: worker
- role: worker
```

Pegar o conteúdo acima criar um arquivo chamado **create_cluster.yaml**
e executar o comando abaixo:

``` 
kind create cluster --config create_cluster.yaml
```

Para verificar se o cluster foi criado corretamente executar os comandos
abaixo:

``` 
kubectl get nodes
kubectl get pods -A
```

Para deletar o cluster criado basta executar o comando abaixo:

``` wp-block-code
kind delete clusters nome-do-cluster
```

## Instalação Helm 

Agora vamos instalar o helm que é um gerenciador de pacotes para o
kubernetes para a instalação de aplicativos. Primeiramente deve ser
feito o download do pacote da última release do helm usando esse
[link](https://github.com/helm/helm/releases).

Depois executar os comandos abaixo:

``` 
tar -zxvf nome-do-pacote
sudo mv linux-amd64/helm /usr/local/bin
```

Para verificar se o helm foi instalado com sucesso, executar o comando
abaixo:

``` 
helm
```

## Conclusão 

O objetivo desse artigo foi criar um tutorial para a criação de um
cluster kubernetes localmente e também alguns pacotes essenciais para
administração do cluster. Abaixo alguns links para te ajudar em caso de
dúvida:

-   <https://helm.sh/docs/intro/install/>
-   <https://kind.sigs.k8s.io/docs/user/quick-start/>
-   <https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/>
