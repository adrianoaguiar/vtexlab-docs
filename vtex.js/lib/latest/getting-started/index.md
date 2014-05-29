---
layout: docs
title: Guia
application: vtex.js
docType: lib
version: latest
---

# Getting Started

Este artigo irá introduzí-lo a:

 - **Promises**, nosso jeito de lidar com resultados de operações assíncronas (como chamadas AJAX);
 - **Eventos**, que são lançados pelo vtex.js e ajudam a manter seu componente atualizado com os dados mais recentes;
 - **Templates**, nossa recomendação para uma experiência sem frustrações na construção do HTML do seu template;
 - Os **Módulos** do vtex.js, e suas particularidades.

## Promises
{: #Promises .slug-text}

*Em breve.*

## Eventos
{: #Eventos .slug-text}

*Em breve.*

## Templates
{: #Templates .slug-text}

*Em breve.*

## Módulos
{: #Modulos .slug-text}

O vtex.js é composto de vários módulos, que contém funções que servem para se comunicar com os serviços da VTEX.

Os módulos residem no objeto global `vtexjs`.
Quando você inclui o script de um módulo, é criado um objeto com todos os métodos para acesso às APIs desse módulo.
Por exemplo, ao incluir o módulo do Checkout, você agora tem o objeto `vtexjs.checkout`, com diversos métodos para acessar a API do Checkout.


### Módulo Checkout - vtexjs.checkout
{: #ModuloCheckout .slug-text}

O módulo Checkout manipula dados referentes à compra do cliente.

Naturalmente, o Checkout agrega os mais diversos dados necessários para o fechamento de uma compra: dados pessoais, de endereço, de frete, de items, entre outros.

O OrderForm é a estrutura responsável por esse aglomerado de dados.
Ele é composto de diversas seções, cada uma com informações úteis que podem ser acessadas, manipuladas e (possivelmente) alteradas.
Se tiver qualquer dúvida quanto a suas seções, consulte a [documentação do OrderForm](../checkout/order-form.html).

Veja a documentação completa de todos os métodos desse módulo [aqui](checkout.md).


### Módulo Catalog - vtexjs.catalog
{: #ModuloCatalog .slug-text}

O módulo Catalog obtém dados referentes aos produtos da loja.

Veja a documentação completa de todos os métodos desse módulo [aqui](../catalog/index.html).


## Versões
{: #Versoes .slug-text}

O vtex.js segue o princípio de [versionamento semântico](http://semver.org/). Isso significa que, em dada versão x.y.z:

 - **x** é chamado de **major** e só muda se houver uma quebra de compatibilidade, ex: `1.5.2 --> 2.0.0`.
 - **y** é chamado de **minor** e muda quando há adição de funcionalidades, ex: `1.5.2 --> 1.6.0`.
 - **z** é chamado de **patch** e muda quando há correções de bugs, ex: `1.5.2 --> 1.5.3`.

Ao incluir os scripts do vtex.js, você pode indicar o **major** ou o **major.minor** que você deseja incluir.
Automaticamente será servido a última versão que atende ao critério.

Não é possível especificar versões no nível de **patch**.


## Instalação
{: #Instalacao .slug-text}

O vtex.js depende do jQuery, então certifique-se que ele está incluído na página antes do vtex.js.

Você pode incluir, em sua loja, todos os módulos do vtex.js:

{% highlight html %}
<script src="//io.vtex.com.br/vtex.js/0/vtex.min.js"></script>
{% endhighlight %}

Ou incluir módulos individualmente:

{% highlight html %}
<script src="//io.vtex.com.br/vtex.js/0/catalog.min.js"></script>
<script src="//io.vtex.com.br/vtex.js/0/checkout.min.js"></script>
{% endhighlight %}

Pronto! Agora você tem nos objetos `vtexjs.catalog` e `vtexjs.checkout` acesso a vários métodos para acesso às APIs da VTEX.

### Selecionando versões

O fragmento `/0/` na URL acima indica que você aceita a versão 0.y.z mais atual, isso é, no major 0.

Você pode indicar uma versão mais específica com `/0.5/`: a versão 0.5.z mais atual, isso é, no major.minor 0.5.
