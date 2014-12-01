---
layout: docs
title: Integração de Pedidos e Tracking
application: erp
docType: guide
---

# Integração de Pedidos e Tracking
{: #Integração de Pedidos e Trackingo .slug-text}

Este documento tem por objetivo auxiliar o integrador na integração pedidos entre ERP e uma loja hospedada na versão smartcheckout da VTEX. Ler os pedidos na VTEX, inserir os pedidos no ERP, enviar as informações de nota fiscal e tracking e ou cancelamento de pedido.

_Exemplo do Fluxo:_

![alt text](pedido-tracking-vtex-erp.PNG "Title")

## Pedidos

Obter a lista de pedidos na VTEX e inserir os pedidos no ERP, atualizando a VTEX que o pedido já está no ERP.

### Obter a Lista de Pedidos por Status na API do OMS
{: #Obter a Lista de Pedidos por Status .slug-text}

Através da API do OMS pegar a lista de pedidos pagos paginados:

**GET /api/oms/pvt/orders?f\_status=ready-for-handling&per\_page=30**

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **f\_status**           | **Number** <br>Define o status em que se deseja obter os pedido [ready-for-handling]. |
| ***per\_page**        | **Number** <br> Define o numero de pedidos que virá na lista. |
{: .doc-api-table }

### Exemplo

### Response
{% highlight json %}
		{
	    "list": [
	        {
	            "orderId": "v1233363wlmr-02", //id do pedido, campo usado para busca o pedido
	            "creationDate": "2014-09-16T19:40:46",
	            "clientName": "Saul Goodman",
	            "items": [
	                {
	                    "seller": "1",
	                    "quantity": 1,
	                    "description": "Notebook Samsung Intel® Core™ i3 380M, RV411-AD2, 2GB, HD 320GB, 14,1\", HDMI, Bluetooth, Webcam - Windows® 7 Home Basic",
	                    "ean": null,
	                    "refId": "14568"
	                }
	            ],
	            "totalValue": 28880,
	            "paymentNames": "American Express",
	            "status": "ready-for-handling",
	            "statusDescription": "Pronto para o manuseio",
	            "marketPlaceOrderId": null,
	            "sequence": "1233365",
	            "salesChannel": "1",
	            "affiliateId": "",
	            "origin": "Marketplace",
	            "workflowInErrorState": false,
	            "workflowInRetry": false,
	            "lastMessageUnread": "VTEX MKP - QA Seu pagamento foi aprovado! pedido realizado em: 16/09/2014 Olá, Saul. Seu pagamento foi efetuado com sucesso. Você receberá o",
	            "ShippingEstimatedDate": "2014-10-02T19:46:23",
	            "orderIsComplete": true,
	            "listId": null,
	            "listType": null
	        }
	    ],
		}
{% endhighlight %}

Esse exemplo retorna uma lista com o resumo de cada pedido (2 pedidos), onde para cada pedido, deve fazer uma chamada na API REST do OMS para pegar o pedido completo, passando o "orderId" do pedido. Segue exemplo de chamada a API REST para pegar um pedido.

### Obter um Pedido na API do OMS
{: #Obter um Pedido na API do OMS .slug-text}

**GET /api/oms/pvt/orders/[orderId]**  

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **orderId**           | **Number** <br>Id do pedido. |
{: .doc-api-table }

### Exemplo

### Response
{% highlight json %}
	{
	    "orderId": "v1233363wlmr-02", //id do pedido
	    "sequence": "1233365", //id numerico do pedido
	    "marketplaceOrderId": "", //se foi um pedido originado no em marketplace, terá o id do pedido no marketplace
	    "marketplaceServicesEndpoint": "", //end point do marketplace para informação de Tracking e NF
	    "sellerOrderId": "00-v1233363wlmr-02", //id do pedido no seller
	    "origin": "Marketplace", origin do pedido: Marketplace(própria loja) | Fulfillment(pedido que veio por canal de vendas
	    "affiliateId": "", //id do affiliado que fez pedido.
	    "salesChannel": "1", //canal de vendas por onde entrou o pedido: 1= lojaprincipal
	    "merchantName": null, // relacionado ao gatway por onde foi o pagamento
	    "status": "ready-for-handling", //status do pedido
	    "statusDescription": "Pronto para o manuseio", //descrição do status do pedido
	    "value": 28880, //valor total do pedido multiplicado por 100
	    "creationDate": "2014-09-16T19:40:46.492703Z", //data de criação
	    "lastChange": "2014-09-16T19:48:46.0139779Z", //data da ultima mudança
	    "orderGroup": "v1233363wlmr", //se foi originado de um pedido splitado, esse é o agrupador
	    "totals": [ //totais do pedido
	        {
	            "id": "Items", //valor do itens
	            "name": "Items Total",
	            "value": 30000
	        },
	        {
	            "id": "Discounts", valor dos descontos
	            "name": "Discounts Total",
	            "value": -1120
	        },
	        {
	            "id": "Shipping", valor de custo de entrega
	            "name": "Shipping Total",
	            "value": 0
	        },
	        {
	            "id": "Tax", //impostos
	            "name": "Tax Total",
	            "value": 0
	        }
	    ],
	    "items": [ //itens do pedido
	        {
	            "id": "270851", // id da sku
	            "productId": "100057806", //id do produto pai da sku
	            "ean": null, //código de barras
	            "lockId": "00-v1233363wlmr-02", // id da reserva
	            "itemAttachment": { //adendos ao item: Exemplo(camisa número 10 com nome customizado)
	                "content": {},
	                "name": null
	            },
	            "itemAttachments": null,
	            "quantity": 1,
	            "seller": "1",
	            "name": "Notebook Samsung Intel® Core™ i3 380M, RV411-AD2, 2GB, HD 320GB, 14,1\", HDMI, Bluetooth, Webcam - Windows® 7 Home Basic",
	            "refId": "14568",
	            "price": 30000,
	            "listPrice": 129800,
	            "priceTags": [
	                {
	                    "name": "discount@price-203#DiscountResource",
	                    "value": -120,
	                    "isPercentual": false,
	                    "identifier": "203"
	                },
	                {
	                    "name": "discount@shipping-138#DiscountResource",
	                    "value": -49,
	                    "isPercentual": false,
	                    "identifier": "138"
	                },
	                {
	                    "name": "discount@price-220#DiscountResource",
	                    "value": -1000,
	                    "isPercentual": false,
	                    "identifier": "220"
	                },
	                {
	                    "name": "discount@shipping-167#DiscountResource",
	                    "value": -252,
	                    "isPercentual": false,
	                    "identifier": "167"
	                },
	                {
	                    "name": "discount@shipping-218#DiscountResource",
	                    "value": -238,
	                    "isPercentual": false,
	                    "identifier": "218"
	                }
	            ],
	            "imageUrl": "/arquivos/ids/959883-55-55/270851_292_292.jpg",
	            "detailUrl": "/270851-notebook-14-intel-core-i3-processor-380m/p",
	            "components": [],
	            "bundleItems": [],
	            "params": [],
	            "offerings": [],
	            "sellerSku": "270851",
	            "priceValidUntil": null,
	            "commission": 0,
	            "tax": 0,
	            "preSaleDate": null,
	            "additionalInfo": {
	                "brandName": "Samsung",
	                "brandId": "3025",
	                "categoriesIds": "/247/254/3058/",
	                "productClusterId": "136,138",
	                "commercialConditionId": "1"
	            },
	            "measurementUnit": "un",
	            "unitMultiplier": 1,
	            "sellingPrice": 28880,
	            "isGift": false,
	            "shippingPrice": null
	        }
	    ],
	    "marketplaceItems": [],
	    "clientProfileData": {
	        "id": "clientProfileData",
	        "email": "4f1431877bb6410798bebf34c6e46b28@ct.vtex.com.br",
	        "firstName": "Saul",
	        "lastName": "Goodman",
	        "documentType": "cpf",
	        "document": "77564538910",
	        "phone": "+552199998888",
	        "corporateName": null,
	        "tradeName": null,
	        "corporateDocument": null,
	        "stateInscription": null,
	        "corporatePhone": null,
	        "isCorporate": false,
	        "userProfileId": "dd013258-7be9-4555-bbfe-7207fadcdd62"
	    },
	    "giftRegistryData": null,
	    "marketingData": null,
	    "ratesAndBenefitsData": {
	        "id": "ratesAndBenefitsData",
	        "rateAndBenefitsIdentifiers": [
	            {
	                "description": "",
	                "featured": false,
	                "id": "220",
	                "name": "Teste Desconto preco carrinho"
	            },
	            {
	                "description": "Teste frete Nominal",
	                "featured": false,
	                "id": "167",
	                "name": "Teste frete Nominal"
	            },
	            {
	                "description": "",
	                "featured": true,
	                "id": "218",
	                "name": "N2 Absoluto"
	            },
	            {
	                "description": "Teste frete nominal fulfillment por valor acumulado",
	                "featured": false,
	                "id": "203",
	                "name": "Teste frete nominal fulfillment por valor acumulad"
	            },
	            {
	                "description": "Teste frete percentual fulfillment",
	                "featured": true,
	                "id": "138",
	                "name": "Teste frete percentual fulfillment"
	            }
	        ]
	    },
	    "shippingData": {
	        "id": "shippingData",
	        "address": {
	            "addressType": "residential",
	            "receiverName": "Gustavo Almeida",
	            "addressId": "2210176a84224e109874e6ed29a6471d",
	            "postalCode": "20010-060",
	            "city": "Rio De Janeiro",
	            "state": "RJ",
	            "country": "BRA",
	            "street": "Rua Visconde De Itaboraí",
	            "number": "70",
	            "neighborhood": "Centro",
	            "complement": null,
	            "reference": null
	        },
	        "logisticsInfo": [
	            {
	                "itemIndex": 0,
	                "selectedSla": "performance",
	                "lockTTL": "8d",
	                "price": 0,
	                "deliveryWindow": null,
	                "deliveryCompany": "Teste de Performance",
	                "shippingEstimate": "12bd",
	                "shippingEstimateDate": "2014-10-02T19:46:23.686688Z",
	                "slas": [ //tipos de entrega
	                    {
	                        "id": "performance",
	                        "name": "performance",
	                        "shippingEstimate": "12bd",
	                        "deliveryWindow": null,
	                        "price": 0
	                    },
	                    {
	                        "id": "Entrega Agendada",
	                        "name": "Entrega Agendada",
	                        "shippingEstimate": "4bd",
	                        "deliveryWindow": null,
	                        "price": 15
	                    },
	                    {
	                        "id": "PAC",
	                        "name": "PAC",
	                        "shippingEstimate": "8d",
	                        "deliveryWindow": null,
	                        "price": 990
	                    }
	                ],
	                "shipsTo": [
	                    "BRA"
	                ],
	                "sellingPrice": 0,
	                "deliveryIds": [
	                    {
	                        "courierId": "1427277",
	                        "courierName": "Teste de Performance",
	                        "dockId": "1_1_1",
	                        "quantity": 1,
	                        "warehouseId": "1_1"
	                    }
	                ]
	            }
	        ]
	    },
	    "paymentData": { //pode ser nulo se o pagamento foi feito em market place
	        "transactions": [
	            {
	                "isActive": true,
	                "transactionId": "33CD3CC4D11A4FA49A2C9EE20D771F98",
	                "payments": [
	                    {
	                        "id": "CCAA6D854D934ADD9DAC301859D19D22",
	                        "paymentSystem": "1",
	                        "paymentSystemName": "American Express",
	                        "value": 28880,
	                        "installments": 1,
	                        "referenceValue": 28880,
	                        "cardHolder": null,
	                        "cardNumber": null,
	                        "firstDigits": "376441",
	                        "lastDigits": "2018",
	                        "cvv2": null,
	                        "expireMonth": null,
	                        "expireYear": null,
	                        "url": null,
	                        "giftCardId": null,
	                        "giftCardName": null,
	                        "giftCardCaption": null,
	                        "redemptionCode": null,
	                        "group": "creditCard",
	                        "tid": "872b5b22-92ab-4062-abff-3325cf2bdae3",
	                        "dueDate": null,
	                        "connectorResponses": {
	                            "Tid": "872b5b22-92ab-4062-abff-3325cf2bdae3",
	                            "ReturnCode": "0",
	                            "Message": null,
	                            "authId": "783994",
	                            "transactionIdentifier": "322363",
	                            "orderKey": "872b5b22-92ab-4062-abff-3325cf2bdae3",
	                            "nsu": "185140",
	                            "acquirer": "Simulator"
	                        }
	                    }
	                ]
	            }
	        ]
	    },
	    "packageAttachment": {
	        "packages": []
	    },
	    "sellers": [
	        {
	            "id": "1",
	            "name": "WalmartV5",
	            "logo": "http://portal.vtexcommerce.com.br/arquivos/logo.jpg"
	        }
	    ],
	    "callCenterOperatorData": null,
	    "followUpEmail": "81acb5b2b0e942e6aac05490a6351278@ct.vtex.com.br",
	    "lastMessage": "VTEX MKP - QA Seu pagamento foi aprovado! pedido realizado em: 16/09/2014 Olá, Saul. Seu pagamento foi efetuado com sucesso. Você receberá o",
	    "hostname": "walmartv5",
	    "changesAttachment": null,
	    "openTextField": null
	}
{% endhighlight %}

### Obter Informações de Pagamento de Pedido
{: #Obter Informações de Pagamento de Pedido .slug-text}

Caso necessário obter informações sobre informações de pagamento de um pedido (como endereço de cobrança por exemplo), deve se acessar a API REST de **Payments** passando o *TID ("paymentData.transactions.transactionId": "33CD3CC4D11A4FA49A2C9EE20D771F98") do gateway VTEX.

No retorno, além de um resumo da transação, poderá obter se as URLs de acesso aos detalhes transação. Segue exemplo de chamada a API REST para pegar as informações de pagamento de um pedido.

**GET /api/pvt/transactions/[transactionId]**

### Exemplo

### Response

{% highlight json %}
	{
	    "id": "[transactionId]",
	    "transactionId": "[transactionId]",
	    "referenceKey": "1233365", //sequence do pedido
	    "interactions": {
	        "href": "/api/pvt/transactions/[transactionId]/interactions" //url para consultar interações
	    },
	    "settlements": {
	        "href": "/api/pvt/transactions/[transactionId]/settlements" //url para consultar capturas
	    },
	    "payments": {
	        "href": "/api/pvt/transactions/[transactionId]/payments" //url para consultar pagamento
	    },
	    "refunds": {
	        "href": "/api/pvt/transactions/[transactionId]/refunds" //url para consultar reeembolsos
	    },
	    "timeoutStatus": 1,
	    "totalRefunds": 0,
	    "status": "Finished",
	    "value": 47368,
	    "receiverUri": null,
	    "startDate": "2014-09-28T09:58:19",
	    "authorizationToken": "F64624BCEA8940D3BE09D8D7E20FECWS",
	    "authorizationDate": "2014-09-28T09:58:27",
	    "commitmentToken": "986A3EBAE0224D8792DF896B9B4ED491",
	    "commitmentDate": "2014-09-30T12:48:31",
	    "refundingToken": null,
	    "refundingDate": null,
	    "cancelationToken": null,
	    "cancelationDate": null,
	    "fields": [
	        {
	            "name": "channel",
	            "value": "COSMETICOS"
	        },
	        {
	            "name": "parentMerchant",
	            "value": "2c13dfe4-22b6-4b0e-9a66-c0f2c3cb7c9t"
	        },
	        {
	            "name": "ip",
	            "value": "187.11.77.002"
	        },
	        {
	            "name": "cart",
	            "value": "{\"items\":[{\"id\":\"6601\",\"name\":\"Ck One Eau de Toilette Calvin Klein - Perfume Unissex 200ml\",\"value\":22440,\"quantity\":1,\"priceTags\":[{\"name\":\"discount@price-669#baaefcdb-887e-4c92-81c5-11d1deb17b83\",\"value\":\"-44,88\"},{\"name\":\"discount@shipping-1#38025ead-9eb3-4735-a2af-17aaa7138d92\",\"value\":\"1\"}],\"shippingDiscount\":0,\"discount\":-4488},{\"id\":\"7098\",\"name\":\"Cool Water Eau de Toilette Davidoff - Perfume Masculino 125ml\",\"value\":19600,\"quantity\":1,\"priceTags\":[{\"name\":\"discount@price-669#baaefcdb-887e-4c92-81c5-11d1deb17b83\",\"value\":\"-39,2\"},{\"name\":\"discount@shipping-1#38025ead-9eb3-4735-a2af-17aaa7138d92\",\"value\":\"1\"}],\"shippingDiscount\":0,\"discount\":-3920},{\"id\":\"6320\",\"name\":\"Animale For Men Eau de Toilette Animale - Perfume Masculino 100ml\",\"value\":17170,\"quantity\":1,\"priceTags\":[{\"name\":\"discount@price-669#baaefcdb-887e-4c92-81c5-11d1deb17b83\",\"value\":\"-34,34\"},{\"name\":\"discount@shipping-1#38025ead-9eb3-4735-a2af-17aaa7138d92\",\"value\":\"1\"}],\"shippingDiscount\":0,\"discount\":-3434}],\"freight\":0,\"shippingdate\":null,\"shippingestimated\":\"5bd\",\"orderUrl\":\"http://www.epocacosmeticos.com.br/admin/checkout/#/orders?q=v1089278epcc\",\"tax\":0}"
	        },
	        {
	            "name": "clientProfileData",
	            "value": "{\"email\":\"jonasalves@hotmail.com\",\"firstName\":\"JONAS\",\"lastName\":\"ALVES\",\"document\":\"29159555837\",\"phone\":\"+551120123496\",\"corporateName\":null,\"tradeName\":null,\"corporateDocument\":null,\"stateInscription\":null,\"postalCode\":\"03205000\",\"address\":{\"receiverName\":\"JONAS ALVES\",\"postalCode\":\"03205000\",\"city\":\"São Paulo\",\"state\":\"SP\",\"country\":\"BRA\",\"street\":\"Rua Doutor Luís Carlos\",\"number\":\"4130\",\"neighborhood\":\"Vila Aricanduva\",\"complement\":\"apt 34\",\"reference\":null},\"gender\":null,\"birthDate\":null,\"corporatePhone\":null,\"isCorporate\":false,\"documentType\":null}"
	        },
	        {
	            "name": "shippingData",
	            "value": "{\"receiverName\":\"JONAS ALVES\",\"postalCode\":\"03205000\",\"city\":\"São Paulo\",\"state\":\"SP\",\"country\":\"BRA\",\"street\":\"Rua Doutor Luís Carlos\",\"number\":\"4350\",\"neighborhood\":\"Vila Aricanduva\",\"complement\":\"apt 34\",\"reference\":null}"
	        },
	        {
	            "name": "waitingApproval",
	            "value": "true"
	        }
	    ],
	    "ipAddress": "187.11.77.002",
	    "antifraudTid": null,
	    "channel": "COSMETICOS",
	    "urn": null,
	    "softDescriptor": null
	}
{% endhighlight %}

### Pedido Está no ERP - Preparando Entrega
{: #Preparando Entrega .slug-text}

Uma vez tendo os dados de pedidos obtidas na API do OMS da VTEX, guarda se o pedido
no respectivo ERP e informa se a VTEX que o pedido está sendo tratado pelo processos de preparar entrega.

Exemplo da chamada ao OMS avisando que o pedido está em preparo de entrega:

POST /api/oms/pvt/orders/[orderid]/start-handling

### Exemplo

### Response

204


## Nota Fiscal
{: #Nota Fiscal .slug-text}

Uma vez o pedido no ERP e o status do pedido na loja VTEX como preparando entrega, vem a parte da Nota Fiscal.
Após receber o pedido, o ERP emite a Nota Fiscal do Pedido e informa a loja VTEX sobre a mesma.

O envio de notas fiscais pode ser parcial, obrigando assim ao informador informar além dos valores da nota fiscal, os items ele está mandando na nota fiscal parcial. Exemplo do POST feito no OMS da VTEX para informar uma Nota Fiscal.

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **orderId**           | **Number** <br>Id do pedido. |
{: .doc-api-table }

 **POST/api/oms/pub/orders/[orderId]/invoice**

### Exemplo

#### Request
{% highlight json %}
	{
	    "type": "Output", // Output|Input (Venda|Devolução)
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

### Exemplo

### Response
{% highlight json %}
	{
	    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
	    "orderId": "123543123",
	    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
  	}
{% endhighlight %}

## Tracking
{: #Tracking .slug-text}

Uma vez informado a Nota Fiscal, vem a parte de ratreamento da Entrega.
O ERP ou a transportadora podem enviar informações de como e onde anda o pedido através da API do OMS VTEX

Exemplo do POST feito no OMS da VTEX para informar Tracking de um pedido.

**POST /api/oms/pub/orders/[orderId]/invoice**

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **an**         					| **String** <br> . O nome do gateway da loja que ta enviando o pagamento |
| **orderId**           | **Number** <br>Id do pedido. |

{: .doc-api-table }


### Exemplo

### Request

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

### Response

{% highlight json %}
	{
	    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
	    "orderId": "123543123",
	    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
  	}
{% endhighlight %}

**A Nota Fiscal** e o **Tracking** podem ser enviados na mesma chamada, basta prenncher todos os dados do POST.

### Cancelamento

O pedido desceu pro ERP, mas por alguma motivo foi cancelado. O ERP invoca uma solicitação de cancelamento
para a API do OMS da loja VTEX. Caso o pedido ainda esteja num estado em que se possa cancelar, o mesmo será cancelado.
Exemplo de request feito no OMS da VTEX para solicitar o cancelamento de um pedido.

GET https://[nomedaloja]/api/oms/pvt/orders/[orderid]/cancel  
