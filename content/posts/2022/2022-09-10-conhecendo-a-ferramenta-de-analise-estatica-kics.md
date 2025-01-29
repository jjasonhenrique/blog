---
title: "Conhecendo a ferramenta de analise estática kics"
author: Jason
date: 2022-09-10
tags: [ "cloud", "AWS" ]
type: post
weight: 28
showTableOfContents: true
---
Nesse artigo irei demonstrar a utilização de um ferramenta
chamada **kics** ou **keeping infrastructure as code secure** tem a
função de executar uma analise estática em códigos de Infraestrutura
como Código como por exemplo Terraform, Cloudformation e outros.

O kics é uma ferramenta Open Source desenvolvida em Golang usando OPA
(Open Policy Agent). Para mais informações sobre OPA consulte
esse [link](https://www.openpolicyagent.org/docs/latest/#overview){rel="noreferrer noopener"
target="_blank"}. O OPA permite você aplicar políticas para seus
microsserviços, kubernetes, pipelines de CI/CD, API Gateways e muitas
outras opções.

O kics faz uma analise estática procurando por vulnerabilidades de
segurança, questões de compliance e erros de configurações em suas
ferramentas de Infraestrutura como Código. No momento que escrevo esse
artigo as seguintes ferramentas abaixo são suportadas:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/04b7b-1ckruewcyskouno6qxozcrg.png" />
</figure>

Através do kics você pode criar novas queries conforme a necessidade do
seu ambiente. Mais informações
nesse [link](https://docs.kics.io/latest/creating-queries/){rel="noreferrer noopener"
target="_blank"}.

# Instalação {#7f94 .wp-block-heading .has-large-font-size}

Para a utilização do kics você tem duas opções instalar através do
código fonte ou utilizar o kics através de uma imagem docker. No meu
caso vou utilizar uma imagem docker.

## Docker {#1e8c .wp-block-heading .has-medium-font-size}

``` wp-block-preformatted
docker pull checkmarx/kics:latestdocker run -t -v "{path_to_host_folder_to_scan}":/path checkmarx/kics scan -p "/path" -o "/path/"
```

Para mais informações sobre a instalação pode consultar
esse [link](https://docs.kics.io/latest/getting-started/){rel="noreferrer noopener"
target="_blank"}.

# Demonstração {#c872 .wp-block-heading .has-large-font-size}

Para essa demonstração irei utilizar o
seguinte [repositório](https://github.com/addamstj/tfcourse){rel="noreferrer noopener"
target="_blank"} . Por padrão o kics gera um arquivo json para melhorar
a visualização vamos gerar um arquivo html.

Para executar basta executar o comando abaixo:

``` wp-block-preformatted
nerdctl run -it -v "C:\Users\jhmrj\Documents\tfcourse":/tmp checkmarx/kics scan -p "/tmp/" -o "/tmp/results" --report-formats "html"
```

**Observação:** Estou utilizando o utilitário nerdctl pois na minha
máquina estou executando o containerd. No docker irá funcionar da mesma
forma.

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/5b9c2-1iqiratd8crnylstzsswwww.png" />
</figure>

Abaixo algumas das regras identificadas pelo kics:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/f75fd-1azh-tb6sgmelu4qm28enew.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/2257f-1v1z0txtgitjp19ozgklzhg.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/f9fcd-12rls3zzo5vgeq9oecfhrfq.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/4256c-1ruvj-1y8kmkwpvvmjvinfg.png" />
</figure>

Foi gerado um arquivo html no caminho /tmp/results/results.html. Abaixo
segue alguns prints desse report.

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/57c89-1f5vptycoewsnk7okhsfa3a.png" />
</figure>

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/03467-1tfjqavrebvgysuukuhmgxa.png" />
</figure>

# Integração {#c0ea .wp-block-heading .has-large-font-size}

Com o kics você consegue integrar com várias ferramentas de ci/cd e
também existe um plugin para vscode.
Nesse [link](https://docs.kics.io/latest/integrations/){rel="noreferrer noopener"
target="_blank"} você consegue verificar todas as integrações
disponíveis.

# Referências {#46ca .wp-block-heading .has-large-font-size}

-   **Github
    kics:** [https://github.com/Checkmarx/kics](https://github.com/Checkmarx/kics){rel="noreferrer noopener"
    target="_blank"}
-   **Documentação
    Oficial:** [https://docs.kics.io/latest/getting-started/](https://docs.kics.io/latest/getting-started/){rel="noreferrer noopener"
    target="_blank"}

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a8588a7072}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-de-analise-estatica-kics/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-170" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-de-analise-estatica-kics/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-170" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-de-analise-estatica-kics/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-170" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-de-analise-estatica-kics/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-de-analise-estatica-kics/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-170-679a8588a78a0 .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=170&origin=jjasonhenrique.wordpress.com&obj_id=205418077-170-679a8588a78a0&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-170-679a8588a78a0" data-title="Curtir ou reblogar"}
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
[28 de janeiro de
2023](https://jjasonhenrique.me/2023/01/28/conhecendo-a-ferramenta-de-analise-estatica-kics/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Kubernetes](https://jjasonhenrique.me/category/kubernetes/){rel="tag"}[,
]{.wp-block-post-terms__separator}[sast](https://jjasonhenrique.me/category/sast/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Terraform](https://jjasonhenrique.me/category/terraform/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloudcomputing](https://jjasonhenrique.me/tag/cloudcomputing/){rel="tag"}[,
]{.wp-block-post-terms__separator}[kics](https://jjasonhenrique.me/tag/kics/){rel="tag"}[,
]{.wp-block-post-terms__separator}[sast](https://jjasonhenrique.me/tag/sast/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Terraform](https://jjasonhenrique.me/tag/terraform/){rel="tag"}
:::
::::::::
:::::::::
::::::::::
