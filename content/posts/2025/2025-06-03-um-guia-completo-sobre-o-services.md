---
title: "Um Guia Completo sobre o services no kubernetes"
author: Jason
date: 2025-06-03
tags: [ "Cloud", "AWS", "Security" ]
type: post
weight: 31
showTableOfContents: true
---
![kubernetes](https://jjasonhenrique.github.io/blog/images/kubernetes.jpg)

No Kubernetes, os **Services** desempenham um papel fundamental ao permitir a comunicação entre os componentes dentro de um cluster, bem como entre o cluster e o mundo externo. Eles funcionam como um ponto de acesso estável para aplicativos que estão em constante mudança, garantindo que você possa se comunicar com pods, mesmo quando eles são criados ou destruídos.

Neste post, vamos explorar o que são os Services no Kubernetes, seus tipos principais, e como configurá-los.

## Pré Requisitos

Para demonstrar a utlizacao dos **services** do kubernetes serao necessarios os seguintes pre requisitos

- Kind instalado. Mais informacoes sobre como instalar o kind nesse [link](https://jjasonhenrique.github.io/blog/posts/2023/2023-11-15-criando-um-cluster-local-de-kubernetes-com-o-kind/)
- E sera necessario ter um deploy de uma aplicacao para os testes. Vamos usar o deploy abaixo:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx  namespace: app  labels:
    app: nginx
spec:
  replicas: 10
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21.1
        ports:
        - containerPort: 80
```
Deve ser criado um arquivo chamado **deploy.yaml** com o conteudo acima. Para fazer a apply do arquivo deploy.yaml executar o comando abaixo:

``` bash
kubectl apply -f deploy.yaml
```

Para entender sobre um deploy no kubernetes voce pode ler esse [artigo](https://jjasonhenrique.github.io/blog/posts/2024/2024-10-12-kubernetes-entenda-daemonset-deployment-e-statefulset/).

## O que é um Service no Kubernetes?

Um **Service** no Kubernetes é um recurso que expõe um conjunto de pods como um serviço de rede, garantindo um ponto de entrada estável para acessar os pods. Isso é crucial porque os pods no Kubernetes são efêmeros, podendo ser recriados a qualquer momento com novos endereços IP.

Os Services utilizam um *selector* para identificar os pods que devem ser incluídos no serviço, com base em *labels*. Quando um cliente se comunica com o Service, ele é automaticamente redirecionado para um dos pods associados.

## Principais Tipos de Services

Existem quatro tipos principais de Services no Kubernetes, cada um com um caso de uso específico:

### ClusterIP

O tipo `ClusterIP` é usado para expor o Service apenas dentro do cluster. Ele cria um IP interno acessível apenas por outros recursos dentro do cluster.

**Uso:** Comunicação interna entre serviços.

**Exemplo de configuração:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

### NodePort

O `NodePort` expõe o Service em cada nó do cluster em uma porta fixa (entre 30000 e 32767). Esse tipo permite o acesso ao Service de fora do cluster, utilizando o IP de qualquer nó e a porta configurada.

**Uso:** Acesso externo básico sem precisar de um balanceador de carga.

**Exemplo de configuração:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30007
  type: NodePort
```

### LoadBalancer

O `LoadBalancer` cria automaticamente um balanceador de carga externo (geralmente fornecido pelo provedor de nuvem) que distribui o tráfego para os pods do Service.

**Uso:** Exposição de serviços na internet com balanceamento de carga integrado.

**Exemplo de configuração:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

### ExternalName

O `ExternalName` mapeia um Service para um nome DNS externo, sem alocar um cluster IP. Ele é usado para redirecionar o tráfego para um serviço externo.

**Uso:** Redirecionamento para serviços externos ao cluster.

**Exemplo de configuração:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-externalname-service
spec:
  type: ExternalName
  externalName: example.com
```

## Como o Service se Conecta aos Pods?

Os Services utilizam uma combinação de *selectors* e *labels* para identificar quais pods devem ser incluídos. Isso permite uma vinculação dinâmica entre o Service e os pods, que pode ser ajustada apenas alterando os rótulos.

Além disso, os Services usam um mecanismo chamado **Endpoints**, que mantém a lista de IPs dos pods associados. Você pode inspecionar os endpoints usando o comando:

```bash
kubectl get endpoints <service-name>
```

## Conclusão

Os **Services** são um componente essencial para gerenciar a comunicação dentro e fora de um cluster Kubernetes. Com os diferentes tipos de Services, é possível atender a diversas necessidades, desde comunicação interna até exposição pública segura e escalável.

Se você está começando a explorar o Kubernetes ou já trabalha com ele, dominar o uso de Services é um passo fundamental para construir e operar aplicações distribuídas de forma eficiente.

**Dica:** Experimente configurar diferentes tipos de Services no seu ambiente de testes para entender melhor seus comportamentos.