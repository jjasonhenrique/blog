---
title: "Testando o recurso Synthetics Canaries do Cloudwatch"
author: Jason
date: 2023-02-06
tags: [ "cloud", "AWS" ]
type: post
weight: 22
showTableOfContents: true
---
# Testando o recurso Synthetics Canaries do Cloudwatch 
O objetivo desse post é demonstrar a criação de um synthetic canary na
AWS. É um recurso que está dentro do serviços oferecidos dentro do
Cloudwatch e tem a principal função de testar se uma URL ou uma API está
retornando status de sucesso (HTTP 200) durante uma checagem a cada um
minuto por exemplo. Esse recurso existe em outras ferramentas de
Observabilidade que já trabalhei por exemplo Dynatrace e Datadog.

Para mais informações sobre esse serviço consulte esse
[link](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html){rel="noreferrer noopener"
target="_blank"}.

**Pré-Requisitos**

Para testar esse serviço da AWS foi criar uma API em Python usando o
FastAPI. Para saber mais informações sobre o que é o FastAPI e como
criar uma API usando FastAPI pode consultar esse
[link](https://pythonacademy.com.br/blog/como-usar-o-fastapi-para-construir-apis-no-python){rel="noreferrer noopener"
target="_blank"}. E também o
[link](https://fastapi.tiangolo.com/){target="_blank"
rel="noreferrer noopener"} da documentação oficial.

Para a instalação do FastAPI utilizarei uma EC2 com as seguintes
configurações abaixo:

<figure class="wp-block-image aligncenter size-large is-resized">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png?w=436"
class="wp-image-286" data-attachment-id="286"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-4/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png"
data-orig-size="436,791" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-4" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png?w=165"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png?w=436"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png?w=259 259w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png?w=83 83w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png?w=165 165w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-4.png 436w"
sizes="(max-width: 259px) 100vw, 259px" width="259" height="470" />
</figure>

**Lembrando que essa EC2 deve possuir um IP Publico.**

**Instalação FastAPI**

Após a criação da EC2 na AWS deverá ser executado os seguintes comandos
abaixo para a instalação do FastAPI:

``` wp-block-code
sudo apt update
sudo apt install python3-pip -y
pip install fastapi
pip install uvicorn
```

Para validar a instalação execute o comando abaixo:

``` wp-block-code
pip show fastapi
```

Deverá retornar algo conforme imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png?w=893"
class="wp-image-288" data-attachment-id="288"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-5/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png"
data-orig-size="893,226" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-5" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png?w=893"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png 893w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-5.png?w=768 768w"
sizes="(max-width: 893px) 100vw, 893px" width="893" height="226" />
</figure>

Após a instalação do FastAPI devemos criar o arquivo **main.py**
conforme abaixo:

``` wp-block-code
from fastapi import FastAPI

app = FastAPI()

@app.get("/")

def index():
    return "Hello World"
```

Após salvar o arquivo acima e executar o seguinte comando abaixo para
iniciar a execução do FastAPI:

``` wp-block-code
uvicorn main:app --host 0.0.0.0 --port 8080
```

No meu caso estou usando a porta 8080. Será necessário a liberação dessa
no security group da EC2 conforme imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=1024"
class="wp-image-291" data-attachment-id="291"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-6/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png"
data-orig-size="1752,128" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-6" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=1013 1013w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-6.png 1752w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="74" />
</figure>

Utilizando o dns público ou ip público da sua EC2 na porta 8080 devemos
ter um resultado similar conforme imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png?w=1024"
class="wp-image-293" data-attachment-id="293"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-7/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png"
data-orig-size="1921,1021" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-7" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-7.png 1921w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="544" />
</figure>

**Configuração Synthetics Canaries**

Para continuar a configuração do recurso Synthetic Canaries do
Cloudwatch é necessário já ter criado a API conforme seção acima.

Para iniciar a configuração você deve ir na opção **Application
Monitoring** \>\> **Synthetics Canaries** no serviço do **Cloudwatch**
conforme imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-8.png?w=220"
class="wp-image-297" data-attachment-id="297"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-8/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-8.png"
data-orig-size="220,198" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-8" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-8.png?w=220"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-8.png?w=220"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-8.png 220w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-8.png?w=150 150w"
sizes="(max-width: 220px) 100vw, 220px" width="220" height="198" />
</figure>

Será mostrado uma tela conforme abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png?w=1024"
class="wp-image-299" data-attachment-id="299"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-9/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png"
data-orig-size="1696,751" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-9" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-9.png 1696w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="453" />
</figure>

Para efetuar a criação basta clicar na opção **Create Canary** e será
apresentado a tela abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png?w=822"
class="wp-image-303" data-attachment-id="303"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-10/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png"
data-orig-size="822,522" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-10" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png?w=822"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png 822w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-10.png?w=768 768w"
sizes="(max-width: 822px) 100vw, 822px" width="822" height="522" />
</figure>

Nesta primeira seção temos a opção de usar padrões já criados pela AWS,
usar nossos próprios scripts e importar de um bucket S3. No meu caso vou
usar a primeira opção.

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-12.png?w=742"
class="wp-image-307" data-attachment-id="307"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-12/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-12.png"
data-orig-size="742,360" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-12" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-12.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-12.png?w=742"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-12.png 742w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-12.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-12.png?w=300 300w"
sizes="(max-width: 742px) 100vw, 742px" width="742" height="360" />
</figure>

Nesta segunda seção você irá definir o nome do Canary e o endpoint URL
que será checado.

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-13.png?w=742"
class="wp-image-308" data-attachment-id="308"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-13/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-13.png"
data-orig-size="742,602" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-13" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-13.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-13.png?w=742"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-13.png 742w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-13.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-13.png?w=300 300w"
sizes="(max-width: 742px) 100vw, 742px" width="742" height="602" />
</figure>

Nesta terceira seção será escolhido o runtime que será executado. No meu
caso eu optei pelo runtime que utiliza o python3.8 + selenium + chromium
mas existem outras opções. Também é possível configurar variáveis de
ambiente para a execução do script.

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-14.png?w=737"
class="wp-image-310" data-attachment-id="310"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-14/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-14.png"
data-orig-size="737,427" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-14" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-14.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-14.png?w=737"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-14.png 737w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-14.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-14.png?w=300 300w"
sizes="(max-width: 737px) 100vw, 737px" width="737" height="427" />
</figure>

Nesta quarta seção você irá configurar de quanto em quanto tempo será
executado a checagem do endpoint URL.

Outras opções que podem ser configuradas nessa mesma tela:

-   Tempo de retenção dos resultados;
-   Bucket onde será salvo os resultados;
-   Criação de uma IAM Role;
-   Criação de um alarme no Cloudwatch;
-   Permite que a checagem só execute dentro de uma determinada VPC;
-   Configuração de Tags;

Após a criação será apresentado esse dashboard conforme imagens abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png?w=1024"
class="wp-image-311" data-attachment-id="311"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-15/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png"
data-orig-size="1662,670" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-15" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png?w=1022 1022w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-15.png 1662w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="412" />
</figure>

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png?w=1024"
class="wp-image-314" data-attachment-id="314"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-16/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png"
data-orig-size="1660,582" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-16" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-16.png 1660w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="359" />
</figure>

Simulei parar a execução da API para verificar se o mesmo é alertado
conforme imagem abaixo:

<figure class="wp-block-image aligncenter size-large">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png?w=1024"
class="wp-image-316" data-attachment-id="316"
data-permalink="https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/image-17/"
data-orig-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png"
data-orig-size="1655,657" data-comments-opened="1"
data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}"
data-image-title="image-17" data-image-description=""
data-image-caption=""
data-medium-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png?w=300"
data-large-file="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png?w=1024"
loading="lazy"
srcset="https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png?w=1024 1024w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png?w=150 150w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png?w=300 300w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png?w=768 768w, https://jjasonhenrique.me/wp-content/uploads/2023/02/image-17.png 1655w"
sizes="(max-width: 1024px) 100vw, 1024px" width="1024" height="406" />
</figure>

Também é possível criar o Synthetics Canaries através do Terraform
conforme informado nesse
[link](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/synthetics_canary){target="_blank"
rel="noreferrer noopener"}.

**Conclusão**

O intuito desse post foi demonstrar a criação de uma simples API e a
checagem dela utilizando o recurso Synthetics Canaries do Cloudwatch.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a7ffd8df60}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-281" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-281" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-281" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-281-679a7ffd8e86e .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=281&origin=jjasonhenrique.wordpress.com&obj_id=205418077-281-679a7ffd8e86e&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-281-679a7ffd8e86e" data-title="Curtir ou reblogar"}
::: {.likes-widget-placeholder .post-likes-widget-placeholder style="height: 55px;"}
[Curtir]{.button} [Carregando...]{.loading}
:::

[]{.sd-text-color}[]{.sd-link-color}
::::
::::::::
::::::::::

:::::::::: wp-block-template-part
::::::::: {.wp-block-group .is-layout-constrained .wp-block-group-is-layout-constrained}
:::::::: {.wp-block-group .post-meta .is-layout-flex .wp-block-group-is-layout-flex}
:::: {.is-style-post-author-icon .wp-block-post-author style="font-size:var(--wp--custom--font-sizes--x-small);"}
::: wp-block-post-author__content
Jason Henrique
:::
::::

::: {.is-style-post-date-icon .wp-block-post-date style="font-size:var(--wp--custom--font-sizes--x-small);"}
[6 de fevereiro de
2023](https://jjasonhenrique.me/2023/02/06/testando-o-recurso-synthetics-canaries-do-cloudwatch/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Cloud
Computing](https://jjasonhenrique.me/category/cloud-computing/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/tag/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloudcomputing](https://jjasonhenrique.me/tag/cloudcomputing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[observability](https://jjasonhenrique.me/tag/observability/){rel="tag"}
:::
::::::::
:::::::::
::::::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

::::::::::::: {.wp-block-group .is-layout-constrained .wp-container-core-group-is-layout-6 .wp-block-group-is-layout-constrained style="padding-top:30px;padding-right:20px;padding-bottom:0px;padding-left:20px"}
::::::: {.wp-block-columns .alignwide .next-prev-links .is-layout-flex .wp-container-core-columns-is-layout-1 .wp-block-columns-is-layout-flex}
:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-previous .wp-block-post-navigation-link}
[←]{.wp-block-post-navigation-link__arrow-previous .is-arrow-arrow
aria-hidden="true"}[Minha Jornada para certificação
AWS Developer](https://jjasonhenrique.me/2023/01/28/minha-jornada-para-certificacao-aws-developer/){rel="prev"}
:::
::::
