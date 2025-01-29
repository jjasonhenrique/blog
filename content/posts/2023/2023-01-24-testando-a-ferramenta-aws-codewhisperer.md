---
title: "Testando a ferramenta AWS CodeWhisperer"
author: Jason
date: 2023-01-24
tags: [ "cloud", "AWS" ]
type: post
weight: 23
showTableOfContents: true
---
O objetivo desse post é demonstrar a utilização do serviço AWS
CodeWhisperer que é uma ferramenta que sugere códigos para um
desenvolvedor usando Machine Learning e Inteligência Artificial. Essa
ferramenta é similar ao Github Copilot.

Essa ferramenta suporta as seguintes linguagens:

-   C#
-   Java
-   JavaScript
-   Python
-   TypeScript

E se integra com as seguintes IDEs:

-   Intellij IDEA
-   PyCharm
-   VS Code
-   AWS Cloud9
-   AWS Lambda

Para essa demonstração irei utilizar o VS Code. Para mais informações
sobre esse serviço da AWS acessar esse
[link](https://aws.amazon.com/pt/codewhisperer/){rel="noreferrer noopener"
target="_blank"}.

O serviço também permite fazer um scan de segurança do seu código.

### Pré-Requisitos {#pré-requisitos .wp-block-heading .has-large-font-size}

Para testar esse serviço da AWS você deve ter os seguintes
pré-requisitos:

-   VS Code instalado. Para fazer o download acessar esse
    [link](https://code.visualstudio.com/download){rel="noreferrer noopener"
    target="_blank"}.
-   Instalar a extensão AWS Toolkit no VS Code conforme imagem abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/d6dc3-1e0zyna3-ngxo_ovpsn1lig.png" />
</figure>

### Instalação e Configuração {#instalação-e-configuração .wp-block-heading .has-large-font-size}

Apos a instalação da extensão da AWS Toolkit irá aparecer um ícone da
aws no lado esquerdo similar a imagem abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/90ff2-1y_l14blrhveslbontgdlvw.png" />
</figure>

Para ativar a utilização do serviço AWS CodeWhisperer será necessário
configurar a conexão com a AWS.

Eu utilizei a opção **"Use a personal email to sign up and sign in with
AWS Builder ID "** assim que clicar nessa opção você será redirecionada
para uma página web para efetuar a criação do seu AWS Builder ID.

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/c61aa-14tdqkfhlg1aywx-tnmbgig.png" />
</figure>

### Demonstração {#demonstração .wp-block-heading .has-large-font-size}

Para a demonstração eu criei um arquivo com a extensão .py e adicionei
um comentário sobre o que gostaria de criar por exemplo "Create simple
API with fastAPI" com isso ele vai dando sugestões de código conforme
imagem abaixo:

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/41427-1ojrhlvaa9c3ue7aex8yxhw.png" />
</figure>

Esse foi o resultado final apresentado pelo AWS CodeWhisperer

<figure class="wp-block-image">
<img
src="https://jjasonhenrique.me/wp-content/uploads/2023/01/a267a-1usutb-uqdub8x7ahdcmerw.png" />
</figure>

### Conclusão {#conclusão .wp-block-heading .has-large-font-size}

Achei bem legal esse serviço da AWS pode ser uma alternativa bem
interessante ao Github Copilot.

[]{#wordads-inline-marker style="display: none;"}

::: {#atatags-2054180771-679a813283db2}
:::

:::::::: {#jp-post-flair .sharedaddy .sd-like-enabled}
::::: {.sharedaddy .sd-sharing-enabled}
:::: {.robots-nocontent .sd-block .sd-social .sd-social-icon .sd-sharing}
### Compartilhe isso: {#compartilhe-isso .sd-title}

::: sd-content
-   [[Clique para compartilhar no LinkedIn(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-aws-codewhisperer/?share=linkedin "Clique para compartilhar no LinkedIn"){.share-linkedin
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-linkedin-26" target="_blank"}
-   [[Clique para compartilhar no Facebook(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-aws-codewhisperer/?share=facebook "Clique para compartilhar no Facebook"){.share-facebook
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-facebook-26" target="_blank"}
-   [[Clique para compartilhar no Twitter(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-aws-codewhisperer/?share=twitter "Clique para compartilhar no Twitter"){.share-twitter
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="sharing-twitter-26" target="_blank"}
-   [[Clique para compartilhar no Telegram(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-aws-codewhisperer/?share=telegram "Clique para compartilhar no Telegram"){.share-telegram
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   [[Clique para compartilhar no WhatsApp(abre em nova
    janela)]{.sharing-screen-reader-text}](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-aws-codewhisperer/?share=jetpack-whatsapp "Clique para compartilhar no WhatsApp"){.share-jetpack-whatsapp
    .sd-button .share-icon .no-text rel="nofollow noopener noreferrer"
    shared="" target="_blank"}
-   
:::
::::
:::::

:::: {#like-post-wrapper-205418077-26-679a8132846ff .sharedaddy .sd-block .sd-like .jetpack-likes-widget-wrapper .jetpack-likes-widget-unloaded data-src="//widgets.wp.com/likes/index.html?ver=20250129#blog_id=205418077&post_id=26&origin=jjasonhenrique.wordpress.com&obj_id=205418077-26-679a8132846ff&domain=jjasonhenrique.me" data-name="like-post-frame-205418077-26-679a8132846ff" data-title="Curtir ou reblogar"}
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
2023](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-aws-codewhisperer/)
:::

::: {.taxonomy-category .is-style-post-category-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/category/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[Cloud
Computing](https://jjasonhenrique.me/category/cloud-computing/){rel="tag"}
:::

::: {.taxonomy-post_tag .is-style-post-tag-icon .wp-block-post-terms style="font-size:var(--wp--custom--font-sizes--x-small)"}
[aws](https://jjasonhenrique.me/tag/aws/){rel="tag"}[,
]{.wp-block-post-terms__separator}[cloud](https://jjasonhenrique.me/tag/cloud/){rel="tag"}
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
:::
::::

:::: {.wp-block-column .is-layout-flow .wp-block-column-is-layout-flow}
::: {.post-navigation-link-next .has-text-align-right .wp-block-post-navigation-link}
[Testando a
ferramenta HardenEKS](https://jjasonhenrique.me/2023/01/28/testando-a-ferramenta-hardeneks/){rel="next"}[→]{.wp-block-post-navigation-link__arrow-next
.is-arrow-arrow aria-hidden="true"}
:::
::::
:::::::

::: {.wp-block-spacer style="height:150px" aria-hidden="true"}
:::

:::::: wp-block-comments-query-loop
::::: {#respond .comment-respond .wp-block-post-comments-form}
### Deixe um comentário [[Cancelar resposta](/2023/01/28/testando-a-ferramenta-aws-codewhisperer/#respond){#cancel-comment-reply-link rel="nofollow" style="display:none;"}]{.small} {#reply-title .comment-reply-title}

::: {.comment-form__verbum .transparent}
:::

