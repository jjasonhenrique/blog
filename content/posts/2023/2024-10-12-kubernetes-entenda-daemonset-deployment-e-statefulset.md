---
layout: post
title: "Kubernetes: Entenda DaemonSet, Deployment e StatefulSet"
author: Jason
categories: [ containers ]
---

O objetivo desse artigo é explicar e demonstrar sobre os objectos
DaemonSet, Deployment e StatefulSet no Kubernetes, a principal
ferramenta de orquestração de containers. Para os testes iremos utilizar
um cluster kubernetes local usando o kind.

Para a criação do seu cluster local usando kind você pode seguir esse
[link](https://jjasonhenrique.me/2023/11/15/criando-um-cluster-local-de-kubernetes-com-o-kind/).
Nesse cluster será criado um nó como ControlPlane e outros dois nós como
DataPlane.

## Pré Requisitos 

Para conseguir acompanhar esse artigo será necessário os seguintes pré
requisitos abaixo:

-   Ter um cluster kubernetes no nossso caso utilizaremos o kind.
-   Ter o kubectl instalado.

Para os exemplos iremos criar um namespace onde serão feitos nossos
testes. Para criar esse namespace executar o comando abaixo:

``` 
kubectl create ns app
```

## DaemonSet 

### O que é? 

O **DaemonSet** é um tipo de controlador no Kubernetes que garante que
uma cópia de um determinado Pod seja executada em cada node de um
cluster. Ele é especialmente útil para rodar serviços que precisam estar
presentes em todos os nós, como agentes de monitoramento, logs ou
sistemas de rede.

### Quando usar? 

-   **Monitoramento e Logging**: Se você precisa coletar logs ou
    métricas de todos os nós, o DaemonSet é ideal.
-   **Networking**: Usado para implantar ferramentas de rede, como
    CNI(Container Network Interface) por exemplo o weave-net, que devem
    estar presentes em todos os nós.

### Exemplo de uso: 

Para esse primeiro exemplo vamos criar um arquivo chamado daemonset.yaml
com o conteúdo abaixo:

``` 
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluent-bit
  namespace: app
spec:
  selector:
    matchLabels:
      name: fluent-bit
  template:
    metadata:
      labels:
        name: fluent-bit
    spec:
      containers:
      - name: fluent-bit
        image: fluent/fluent-bit:latest
        resources:
          limits:
            memory: 200Mi
            cpu: 100m
```

Para aplicar o exemplo acima executar o comando abaixo:

``` 
kubectl apply -f daemonset.yaml
```

Neste exemplo, o DaemonSet implementa um agente do fluent-bit para
coletar logs de cada nó do cluster. Esse exemplo é apenas para mostrar o
funcionamento do daemonset não pretendemos fazer testes com o agente do
fluent-bit.

Para verificar o funcionamento dos pods executar o comando abaixo:

``` wp-block-code
kubectl get pods -n app
```

Deverá retornar uma saída similar a imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image.png?w=487"
class="wp-image-1164" data-attachment-id="1164"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-58/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image.png"
data-orig-size="487,77" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image.png?w=487"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image.png 487w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image.png?w=300 300w"
sizes="(max-width: 487px) 100vw, 487px" width="487" height="77" />
</figure>

Como temos apenas dois nós que executam como DataPlane foram criados
apenas dois pods do fluent-bit. Para retornar o nome dos nós que os pods
estão executando adicionar a flag -o wide conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=1024"
class="wp-image-1166" data-attachment-id="1166"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-59/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png"
data-orig-size="1160,88" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=1015 1015w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-1.png 1160w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="77" />
</figure>

O comando abaixo mostra quantos daemonsets estão executando no cluster:

``` 
kubectl get daemonset -n app
```

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png?w=791"
class="wp-image-1167" data-attachment-id="1167"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-60/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png"
data-orig-size="791,70" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png?w=791"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png 791w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-2.png?w=768 768w"
sizes="(max-width: 791px) 100vw, 791px" width="791" height="70" />
</figure>

## Deployment 

### O que é? 

O **Deployment** é o controlador mais utilizado no Kubernetes para
gerenciar a criação e a escalabilidade de réplicas de Pods.

### Quando usar? 

-   **Aplicações Stateless**: Ideal para a maioria das aplicações web,
    APIs ou microservices que não mantêm estado interno entre diferentes
    réplicas.

### Exemplo de uso: 

Nesse segundo exemplo vamos criar um arquivo chamado deployment.yaml com
o conteúdo abaixo:

```
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

Para aplicar o manifesto acima executar o comando:

``` 
kubectl apply -f deployment.yaml
```

Aqui, o Deployment define dez réplicas de um container NGINX. Em caso de
falha de um Pod, o Deployment cria automaticamente uma nova réplica para
garantir a disponibilidade.

Para verificar a execução dos pods executar o comando abaixo:

``` 
kubectl get pods -n app
```

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-3.png?w=554"
class="wp-image-1172" data-attachment-id="1172"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-61/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-3.png"
data-orig-size="554,234" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-3.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-3.png?w=554"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-3.png 554w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-3.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-3.png?w=300 300w"
sizes="(max-width: 554px) 100vw, 554px" width="554" height="234" />
</figure>

Na configuração default do kubernetes podemos verificar que os pods se
distribuem entre os nós conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=1024"
class="wp-image-1174" data-attachment-id="1174"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-62/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png"
data-orig-size="1248,242" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-4.png 1248w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="198" />
</figure>

O comando abaixo mostra informações do deployment:

``` 
kubectl get deployment -n app
```

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-5.png?w=433"
class="wp-image-1176" data-attachment-id="1176"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-63/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-5.png"
data-orig-size="433,79" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-5.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-5.png?w=433"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-5.png 433w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-5.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-5.png?w=300 300w"
sizes="(max-width: 433px) 100vw, 433px" width="433" height="79" />
</figure>

## StatefulSet 

### O que é? 

O **StatefulSet** é semelhante ao Deployment, mas é utilizado para
gerenciar aplicações stateful, onde a persistência dos dados são
importantes. O StatefulSet garante que os Pods sejam implantados e
escalonados de maneira previsível.

### Quando usar? 

-   **Bancos de Dados**: Ideal para bancos de dados distribuídos, como
    MySQL, Postgress ou qualquer aplicação que exija persistência de
    dados entre os pods. Também utilizado para aplicações que necessitam
    de um sequência lógica.

### Exemplo de uso: 

Nesse terceiro exmeplo vamos criar um arquivo chamado statefulset.yaml
com o conteúdo abaixo:

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
  namespace: app
spec:
  serviceName: "nginx"
  replicas: 3
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
        image: k8s.gcr.io/nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

Neste exemplo, um StatefulSet é usado para criar três réplicas de um
container NGINX, cada uma com um volume de armazenamento persistente
separado.

Para verificar a execução dos pods executar o comando abaixo:

``` 
kubectl get pods -n app
```

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-6.png?w=437"
class="wp-image-1179" data-attachment-id="1179"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-64/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-6.png"
data-orig-size="437,116" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-6.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-6.png?w=437"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-6.png 437w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-6.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-6.png?w=300 300w"
sizes="(max-width: 437px) 100vw, 437px" width="437" height="116" />
</figure>

Outro ponto que podemos verificar é a sequência lógica na subida dos
pods sendo que o primeiro pods foi o web-0, o segundo web-1 e o terceiro
web-2 isso pode ser uma premissa para alguma aplicação.

O comando abaixo mostra informações do statefulset:

``` 
kubectl get statefulset -n app
```

## Bônus: Replicaset 

O **ReplicaSet** é um objecto que tem como responsabilidade manter um
número especifico de réplicas num deployment. Ele é criado
automaticamente quando criamos um deployment. Abaixo o comando para
verificar o replicaset:

``` 
kubectl get replicaset -n app
```

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-7.png?w=546"
class="wp-image-1182" data-attachment-id="1182"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-65/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-7.png"
data-orig-size="546,80" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-7.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-7.png?w=546"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-7.png 546w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-7.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-7.png?w=300 300w"
sizes="(max-width: 546px) 100vw, 546px" width="546" height="80" />
</figure>

Outro ponto do replicaset é que sempre que é feito um novo deploy sempre
será criado um novo replicaset e as replicas estarão disponiveis no novo
replicaset. Abaixo vamos similar um novo deployment executando o comando
abaixo:

``` 
kubectl set image deploy/nginx nginx=nginx:1.19 -n app
```

Na imagem abaixo podemos perceber que foi criado um novo replicaset e os
pods do deployment foram transferidos para esse novo replicaset.

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-8.png?w=546"
class="wp-image-1184" data-attachment-id="1184"
data-permalink="https://jjasonhenrique.me/2024/10/12/kubernetes-entenda-daemonset-deployment-e-statefulset/image-66/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-8.png"
data-orig-size="546,95" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-8.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-8.png?w=546"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2024/10/image-8.png 546w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-8.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2024/10/image-8.png?w=300 300w"
sizes="(max-width: 546px) 100vw, 546px" width="546" height="95" />
</figure>

## Conclusão 

O principal objetivo desse artigo foi demonstrar o que é e como utilizar
os seguintes objectos deployment, daemonset, stateful e replicaset no
kubernetes.

Espero que esse artigo tenha sido útil pra você que mexe há pouco ou
muito tempo com Kubernetes.
