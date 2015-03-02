---
layout: docs
title: Seller Não VTEX Vendendo em Marketplace VTEX
application: marketplace
docType: guide
---
##Seller Não VTEX Vendendo em Marketplace VTEX
 
Este documento tem por objetivo auxiliar na integração e atualização de condição comercial (preço, estoque, frete, SLAs de entrega) de um SKU* entre um Seller não VTEX  para uma loja hospedada na versão smartcheckout da VTEX e também auxiliar na descida de pedido e envio de autorização de despacho para o Seller não VTEX.

##Troca de Catalogo de SKU e Atualização de Condição Comercial de SKU
{: #0 .slug-text}

Sugestão de SKU e atualização de preço, estoque, frete, SLAs de entrega:

_Fluxo de Sugestão de SKU:_

![alt text](sku-sugestion-seller-nao-vtex.png "Fluxo de descida de pedido")


###Enviar de Notificação de Mudança de Condições Comerciais de SKU
{: #1 .slug-text}

Toda vez que houver uma inserção ou alteração na condição comercial (preço, estoque, frete e SLAs de entrega) de um SKU no Seller, se o Seller vende essa SKU no marketplace VTEX, o Seller deve enviar uma notificação de mudança de SKU para a VTEX, caso a VTEX retorne em seu serviço o response status 404, significa que a SKU **não existe na VTEX**, então o Seller deve enviar a sugestão de inserção de SKU para a loja da VTEX.

###Enviar Notificação de Mudança de SKU
{: #2 .slug-text}

<a title="notificar mudança de sku no marketplace" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/CATALOG/CATALOG_Notification" target="_blank">[Developer] - Exemplo de Request de Notificação de Mudança - Endpoint da VTEX</a>


###Enviar Sugestão de SKU
{: #3 .slug-text}

Quando o serviço de notificação retornar um status 404, significa que o SKU NÂO existe no marketplace VTEX, então o Seller envia um POST com os dados da SKU que deseja sugerir para vender no marketplace VTEX. O Seller faz as sugestões de suas SKUs e o administrador do Marketplace realiza o mapeamento de marcas e categorias através do admin da loja, e aceita ou não a sugestão de SKU enviada pelo Seller.


<a title="envia sugestão de sku" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/CATALOG/CATALOG_Sugestion" target="_blank">[Developer] - Exemplo de Request de Inserção de Sugestão de SKU - Endpoint da VTEX</a>


###Atualização de Condição Comercial de SKU
{: #4 .slug-text}

Toda vez que houver uma alteração na condição comercial de um SKU (preço, estoque, frete e SLAs de entrega), o Seller NÂO VTEX deve enviar uma notificação de mudança de SKU para a VTEX, caso a VTEX retorne em seu serviço o response status 200 ou 202, significa que a SKU **existe** na VTEX, então a VTEX vai no Seller consultar as novas condições comerciais oferecidas pelo Seller.


<a title="busca de condições comerciais no seller" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/FULFILLMENT/FULFILLMENT_Simulation" target="_blank">[Developer] - Exemplo de Request de Busca de Condições Comerciais - Endpoint do Seller</a> 


###Simular Compra, Checar Disponibilidade do Carrinho no Seller
{: #5 .slug-text}

Quando um produto é inserido no carrinho no marketplace VTEX, ou faz se alguma edição no carrinho, uma consulta de simulaçao de carrinho é feita no Seller para checar a validade das condiçoes comerciais (preço, estoque, frete e SLAs de entrega). Quando se navega para a página de pagamento um outra checagem é realizada no Seller (_repare que é o mesmo endpoint usado na troca de condições comercias do catalogo_).  


<a title="checar disponibilidade do carrinho no seller" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/FULFILLMENT/FULFILLMENT_Simulation" target="_blank">[Developer] - Simular Compra, Checar Disponibilidade do Carrinho no Seller - Endpoint do Seller</a> 


### Enviar Pedido e Autorizar Despacho
{: #6 .slug-text}

Este documento tem por objetivo auxiliar o Seller não VTEX a receber um pedido, e receber a autorização para despachar o pedido

_Fluxo_ 

![alt text](order-seller-nao-vtex.png "Fluxo de pedido") 

####Enviar Pedido

Quando o pedido é fechado no ambiente VTEX, um POST é feito no Seller não VTEX, para que este possa receber a ordem de pedido.  
 
###Exemplos de Request de Descida de Pedido - Endpoint do Seller###

endpoint: **https://sellerendpoint/api/fulfillment/pvt/orders?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc serve para destacar o canal por onde o pedido entrou

_request:_  

{% highlight json %}	
{
"marketplaceOrderId": "959311095",
"marketplaceServicesEndpoint": "https://urlmarketplace/", //leia o tópico Invocando MarketplaceServicesEndpoint Actions
"marketplacePaymentValue": 11080,
"items": [
  {
    "id": "2002495",
    "quantity": 1,
    "seller": "1",
    "commission": 0,
    "freightCommission": 0,
    "price": 9990,
    "bundleItems": [], //serviços. Ex: embalagem pra presente.
    "itemAttachment": { 
      "name": null,
      "content": {}
    },
    "attachments": [], //customização do item, Ex:camisa com o numero 10
    "priceTags": [],
    "measurementUnit": null, unidade de medida
    "unitMultiplier": 0, unidade multipladora,Ex: venda por quilo
    "isGift": false
  }
],
"clientProfileData": {
  "id": "clientProfileData",
  "email": "32172239852@gmail.com.br",
  "firstName": "Jonas",
  "lastName": "Alves de Oliveira",
  "documentType": null,
  "document": "3244239851",
  "phone": "399271258",
  "corporateName": null,
  "tradeName": null,
  "corporateDocument": null,
  "stateInscription": null,
  "corporatePhone": null,
  "isCorporate": false,
  "userProfileId": null
},
"shippingData": {
  "id": "shippingData",
  "address": {
    "addressType": "Residencial",
    "receiverName": "Jonas Alves de Oliveira",
    "addressId": "Casa",
    "postalCode": "13476103",
    "city": "Americana",
    "state": "SP",
    "country": "BRA",
    "street": "JOÃO DAMÁZIO GOMES",
    "number": "311",
    "neighborhood": "SÃO JOSÉ",
    "complement": null,
    "reference": "Bairro Praia Azul / Posto de Saúde 17",
    "geoCoordinates": []
  },
  "logisticsInfo": [
    {
      "itemIndex": 0,
      "selectedSla": "Normal",
      "lockTTL": "8d",
      "shippingEstimate": "7d",
      "price": 1090,
      "deliveryWindow": null
    }
  ]
},
"openTextField": null,
"marketingData": null,
"paymentData":null
}
{% endhighlight %}  

_response:_  

{% highlight json %}
{
"marketplaceOrderId": "959311095",
"orderId": "123543123",
"followUpEmail": "75c70c09dbb3498c9b3bbdee59bf0687@ct.vtex.com.br",
"items": [
  {
    "id": "2002495",
    "quantity": 1,
    "seller": "1",
    "commission": 0,
    "freightCommission": 0,
    "price": 9990,
    "bundleItems": [],
    "priceTags": [],
    "measurementUnit": "un",
    "unitMultiplier": 1,
    "isGift": false
  }
],
"clientProfileData": {
  "id": "clientProfileData",
  "email": "5c77abaea35f4cb6824b9326942c00e5@ct.vtex.com.br",
  "firstName": "JONAS",
  "lastName": "ALVES DE OLIVEIRA",
  "documentType": "cpf",
  "document": "32133239851",
  "phone": "1592712979",
  "corporateName": null,
  "tradeName": null,
  "corporateDocument": null,
  "stateInscription": null,
  "corporatePhone": null,
  "isCorporate": false,
  "userProfileId": null
},
"shippingData": {
  "id": "shippingData",
  "address": {
    "addressType": "Residencial",
    "receiverName": "JONAS ALVES DE OLIVEIRA",
    "addressId": "Casa",
    "postalCode": "13476103",
    "city": "Americana",
    "state": "SP",
    "country": "BRA",
    "street": "JOÃO DAMÁZIO GOMES",
    "number": "121",
    "neighborhood": "SÃO JOSÉ",
    "complement": null,
    "reference": "Bairro Praia Azul / Posto de Saúde 17",
    "geoCoordinates": []
  },
  "logisticsInfo": [
    {
      "itemIndex": 0,
      "selectedSla": "Normal",
      "lockTTL": "8d",
      "shippingEstimate": "5d",
      "price": 1090,
      "deliveryWindow": null
    }
  ]
},
"paymentData":null
}
{% endhighlight %}

_Exemplo do Retorno de Erro:_  

{% highlight json %}
{
	"error": {
	"code": "1",
	"message": "O verbo 'GET' não é compatível com a rota '/api/fulfillment/pvt/orders'",
	"exception": null
	}
}
{% endhighlight %} 

####Enviar Autorização Para Despachar

Quando o pagamento do pedido é concluído no marketplace VTEX (pagamento válido), envia se ao Seller não VTEX uma autorização para dar andamento no processo de fulfillmente do pedido.  
 
*Exemplos de Request de Autorização - Endpoint da Seller*

endpoint: **https://sellerendpoint/api/fulfillment/pvt/orders/[orderid]/fulfill?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc é o canal de vendas cadastrado no marketplace, serve para destacar o canal por onde o pedido entrou.  

_request:_    

{% highlight json %}
{
	"marketplaceOrderId": "959311095" //id do pedido originado no canal de vendas
}
{% endhighlight %} 

_response:_  

{% highlight json %}
{
	"date": "2014-10-06 18:52:00",
	"marketplaceOrderId": "959311095",
	"orderId": "123543123",
	"receipt": "e39d05f9-0c54-4469-a626-8bb5cff169f8",
}
{% endhighlight %} 

###Invocando Marketplace Services Endpoint Actions
{: #7 .slug-text}

O MarketplaceServicesEndpoint serve para receber informações do seller referentes a nota fiscal e tracking de pedido. É permitido o envio de notas fiscais parciais, obrigando assim ao informador informar além dos valores da nota fiscal,os items ele está mandando na nota fiscal parcial. O pedido na VTEX só andará pra o status FATURADO quando o valor total de todas as notas fiscais de um pedido forem enviadas.

####Exemplos de Request Para Informar Nota Fiscal - Endpoint VTEX###

endpoint: **https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  

_request:_  

{% highlight json %}
{
    "type": "Output", //hard code
    "invoiceNumber": "NFe-00001", //numero da nota fiscal
    "courier": "", //quando é nota fiscal, dados de tracking vem vazio
    "trackingNumber": "", //quando é nota fiscal, dados de tracking vem vazio
    "trackingUrl": "",//quando é nota fiscal, dados de tracking vem vazio
    "items": [ //itens da nota
      {
        "id": "345117",
        "quantity": 1,
        "price": 9003
      }
    ],
    "issuanceDate": "2013-11-21T00:00:00", //data da nota
    "invoiceValue": 9508 //valor da nota
}
{% endhighlight %} 

_response:_  

{% highlight json %}
{
    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
}
{% endhighlight %} 

####Exemplos de Request Para Informar Tracking - Endpoint VTEX###

endpoint: **https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  

_request:_  
  
{% highlight json %}
{
    "type": "Output",
    "invoiceNumber": "NFe-00001",
    "courier": "Correios", //transportadora
    "trackingNumber": "SR000987654321", /tracking number
    "trackingUrl": "http://traking.correios.com.br/sedex/SR000987654321", url de tracking
    "items": [
      {
        "id": "345117",
        "quantity": 1,
        "price": 9003
      }
    ],
    "issuanceDate": "2013-11-21T00:00:00",
    "invoiceValue": 9508
}
{% endhighlight %} 

_response:_  

{% highlight json %}
{
    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
}
{% endhighlight %} 

**A Nota Fiscal e o Tracking podem ser enviados na mesma chamada, basta prenncher todos os dados do DTO de POST.

###Enviar Solicitação de Cancelamento
{: #8 .slug-text}

Uma solicitação de cancelamento pode ser enviada, caso o pedido se encontre em um estado que se possa cancelar, o pedido será cancelado. 
 
*Exemplos de Request de Cancelamento - Endpoint VTEX*

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderid]/cancel**  
verb: **GET**

---

Autor: _Jonas Bolognim_  
Propriedade: _VTEX_