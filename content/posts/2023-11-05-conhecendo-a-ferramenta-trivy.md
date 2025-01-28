---
layout: post
author: Jason
categories: [ containers ]
title: Conhecendo a ferramenta trivy
---

O trivy é uma ferramenta desenvolvido pela empresa Aqua security e
tambem é um dos topicos cobrados no exame da CKS (Certified Kubernetes
Security Specialist) para quem pretende tirar. Abaixo alguns links com
mais informações do trivy:

-   <https://github.com/aquasecurity/trivy>
-   <https://aquasecurity.github.io/trivy/>

Com o trivy você consegue fazer as seguintes verificações:

-   Scan de Vulnerabilidades
-   Erros de configuração em IaC
-   Detectar dados sensíveis

É possível escanear os seguintes targets:

-   Imagem de Container
-   Repositório de algum sistema de versionamento de código como por
    exemplo o GitHub
-   Sistema de Arquivos
-   Clusters Kubernetes
-   Contas AWS

O objectivo desse artigo é demonstrar o scan de vulnerabilidades em uma
imagem de container e em um repositório do github usando o Trivy.

## Instalação 

Para usar o trivy você pode instalar diretamente na sua máquina ou usar
o trivy como um container do Docker.

# Instalação no Linux 

No meu caso vou demonstrar a instalação em uma máquina Linux. Para
verificar a última versão do trivy consultar esse
[link](https://github.com/aquasecurity/trivy/releases). Abaixo os
comandos que você deve executar para fazer o download e a instalação:

``` 
wget https://github.com/aquasecurity/trivy/releases/download/v0.46.0/trivy_0.46.0_Linux-64bit.deb
sudo dpkg -i trivy_0.46.0_Linux-64bit.deb
```

Para validar a instalação executar o comando abaixo:

``` 
trivy version
```

# Docker 

Para utilizar o trivy usando um container basta usar a imagem
**aquasec/trivy:latest**. Por exemplo para verificar qual a versão do
Trivy executar o comando abaixo:

``` 
docker run aquasec/trivy:latest version
```

## Scan de Imagem de Container 

Para executar um scan de vulnerabilidades em alguma imagem de container
basta executar o comando abaixo

``` 
trivy image nginx

docker run aquasec/trivy:latest image nginx
```

Onde **nginx** é o nome da imagem de container

Deverá aparecer uma saída similar abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png?w=1024"
class="wp-image-455" data-attachment-id="455"
data-permalink="https://jjasonhenrique.me/2023/11/05/conhecendo-a-ferramenta-trivy/image-31/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png"
data-orig-size="1533,1009" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image" data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image.png 1533w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="673" />
</figure>

Você pode filtar por severidade HIGH e CRITICAL usando o comando abaixo:

``` 
trivy image nginx --severity=HIGH --severity=CRITICAL
```

Agora deverá aparecer somente as vulnerabilidades com a severidade HIGH
e CRITICAL conforme imagem abaixo:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png?w=1024"
class="wp-image-457" data-attachment-id="457"
data-permalink="https://jjasonhenrique.me/2023/11/05/conhecendo-a-ferramenta-trivy/image-1-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png"
data-orig-size="1467,821" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-1" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png?w=1024"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/image-1.png 1467w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="573" />
</figure>

Na coluna **status** podemos verificar que o status esta como
**affected** isso quer dizer que não existe uma solução para essas
vulnerabilidades ainda.

## Scan de repositório 

Agora vamos fazer um scan de vulnerabilidades no seguinte
[repositório](https://github.com/prometheus/prometheus) executando o
comando abaixo filtrando apenas as vulnerabilidades com severidade HIGH.

``` 
trivy repository https://github.com/prometheus/prometheus --severity=HIGH
```

Abaixo segue a saída do comando:

<figure class="wp-block-image size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=1024"
class="wp-image-461" data-attachment-id="461"
data-permalink="https://jjasonhenrique.me/2023/11/05/conhecendo-a-ferramenta-trivy/screenshot-from-2023-11-05-13-17-58/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png"
data-orig-size="1838,567" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="screenshot-from-2023-11-05-13-17-58"
data-image-description="" data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=1021 1021w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/11/screenshot-from-2023-11-05-13-17-58.png 1838w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="315" />
</figure>

Nesse caso a coluna **status** esta com o status **fixed** isso quer
dizer que já existe uma solução para a vulnerabilidade. Na coluna
**Fixed Version** está a versão que a vulnerabilidade foi corrigida.

## Integração 

O trivy faz integração com diversas alicações e ferramentas que permitem
adicionar segurança ao workflow da sua aplicação como por exemplo
ferramentas de CI/CD e com IDEs. Para mais informações sobre essas
integrações consultar esse
[link](https://aquasecurity.github.io/trivy/v0.46/ecosystem/).

## Conclusão 

O objetivo desse artigo foi demonstrar um pouco da utilização do trivy
para efetuar scan de vulnerabilidades.