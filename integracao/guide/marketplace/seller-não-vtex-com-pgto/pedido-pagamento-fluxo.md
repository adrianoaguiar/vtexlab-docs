# Enviar Pedido e Informar Pagamento

Este documento tem por objetivo auxiliar o Seller não VTEX a receber um pedido, receber o respectivo pagamento do pedido, e comunicar a atualização de status de pagamento.


*Exemplo do fuxo de chamadas de descida de pedido, pagamento e atualização de status de pagamento:*  

![alt text](pedido-pagamento-fluxo.png "Title") 

##1 - Enviar Pedido##
Quando o pedido é fechado no ambiente VTEX, um POST é feito no Seller não VTEX, para que este possa receber a ordem de pedido.  
 
###1.1 - Exemplos de Request de Descida de Pedido - Endpoint do Seller###

endpoint: **https://sellerendpoint/pvt/orders?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc serve para destacar o canal por onde o pedido entrou


*Exemplo do Request:*  

	[
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
		    "paymentData":{
				"merchantName":"shopfacilfastshop" //gateway de redirect na vtex.
			}
		  }
		]

*Exemplo do Response:*

	    [
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
		   "paymentData":{
				"merchantName":"shopfacilfastshop", //devover o parametro recebido no request
				"merchantPaymentReferenceId":"123543123" //inteiro id do pagamento, número que será enviado junto com o pagamento para conciliação.
			}
		  }
		]

*Exemplo do Retorno de Erro:*

	{
		"error": {
		"code": "1",
		"message": "O verbo 'GET' não é compatível com a rota '/api/fulfillment/pvt/orders'",
		"exception": null
		}
	}

##2 - Enviar Pagamento##
Quando o pagamento do pedido é concluído no ambiente VTEX, um POST é feito no marketplace não VTEX, para que este possa receber os dados referente ao pagamento do respectivo pedido.  
 
###2.1 - Exemplos de Request de Descida de Pagamento - Endpoint do Seller###

endpoint: **https://sellerendpoint/pvt/payment?an=shopfacilfastshop**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **an=shopfacilfastshop** // an é o nome do gateway da loja que ta enviando o pagamento


*Exemplo do Request:*  

	{
		"referenceId": "123543123", //merchantPaymentReferenceId retornado no request do place order
		"transactionId": "D3AA1FC8372E430E8236649DB5EBD08E",
		"paymentData": {
			"id": "F5C1A4E20D3B4E07B7E871F5B5BC9F91",
			"paymentSystem": 2,os ids dos tipos de pagamento, 
			"cardNumber": "4444333322221111",
			"cardHolder": "JONAS ALVES DE OLIVEIRA",
			"expiryMonth": 11,
			"expiryYear": 16,
			"value": 11080,
			"installments": 3,
			"cvv2": "123",
			"billingAddress": {
				"addressType": "residential",
				"street": "Rua Cinco De Julho",
				"number": "176",
				"complement": "801",
				"postalCode": "22051-030",
				"city": "Rio De Janeiro",
				"state": "RJ",
				"country": "BRA",
				"neighborhood": ""
			}
		},
		"clientData": {
			"firstName": "JONAS",
			"lastName": "ALVES DE OLIVEIRA",
			"document": "08081268731",
			"corporateName": "",
			"tradeName": "",
			"corporateDocument": "",
			"isCorporate": "false"
		},
		"shippingValue": 3691, 
		"callbackUrl": ""https://nomedaloja.vtexpayments.com.br/api/pvt/callback/vtxstd/transactions/D3AA1FC8372E430E8236649DB5EBD08E/payments/F5C1A4E20D3B4E07B7E871F5B5BC9F91/return" //**url para falar de volta com o gateway de pagamento do marketplace
	}

*Exemplo do Response e do POST Feito na CallbackUrl de Pagamento :*


	{
	  	"paymentId" : "F5C1A4E20D3B4E07B7E871F5B5BC9F91",   // string, not null, Payment identifier sent on authorization request
		"status" : "",    // string, not null, [approved | denied | undefined]
	  	"authorizationId": "", //id da autorização quando aprovado
	  	"bankIssueInvoiceUrl":"urldoboleto" //url do boleto bancario
	}

**O response de pagamento pode ser respondido como "undefined" enquanto o Seller não tem a informação sobre o pagamento. Em caso de marketplace e seller aceitarem boleto, quando recebido um post de pagamento com o paymentSystem igual a boleto, o seller deve gerar o boleto e responder imediatamente com a url de boleto preenchida.
	    

##2.2 - Enviar Autorização Para Despachar##
Quando o pagamento do pedido é concluído no Seller (pagamento válido), um POST deverá ser feito na "callbackUrl" do pagamento, informando sucesso do pagamento ("status":"approved"), nesse momento a loja VTEX envia autorização para despachar o respectivo pedido no Seller.  
 
*Exemplos de Request de Autorização - Endpoint da Seller*

endpoint: **https://sellerendpoint/pvt/orders/[orderid]/fulfill?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc é o canal de vendas cadastrado no marketplace, serve para destacar o canal por onde o pedido entrou.  

*Exemplo do Request:*  

	{
		"marketplaceOrderId": "959311095" //id do pedido originado no canal de vendas
	}

*Exemplo do Response:*

	{
		"date": "2014-10-06 18:52:00",
		"marketplaceOrderId": "959311095",
		"orderId": "123543123",
		"receipt": "e39d05f9-0c54-4469-a626-8bb5cff169f8",
	}
  

##3 Invocando Marketplace Services Endpoint Actions##
O MarketplaceServicesEndpoint serve para receber informações do seller referentes a nota fiscal e tracking de pedido. É permitido o envio de notas fiscais parciais, obrigando assim ao informador informar além dos valores da nota fiscal,os items ele está mandando na nota fiscal parcial. O pedido na VTEX só andará pra o status FATURADO quando o valor total de todas as notas fiscais de um pedido forem enviadas.

###3.1 - Exemplos de Request Para Informar Nota Fiscal - Endpoint VTEX###

endpoint: **https://marketplaceServicesEndpoint/pub/orders/{orderId}/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **an=shopfacilfastshop** // an é o nome do gateway da loja que ta enviando o pagamento


*Exemplo do Request:*  

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

*Exemplo do Response:*

	{
	    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
	    "orderId": "123543123",
	    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
  	}


###3.2 - Exemplos de Request Para Informar Tracking - Endpoint VTEX###

endpoint: **https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  


*Exemplo do Request:*  

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

*Exemplo do Response:*

	{
	    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
	    "orderId": "123543123",
	    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
  	}

**A Nota Fiscal e o Tracking podem ser enviados na mesma chamada, basta prenncher todos os dados do POST.


###3.3 - Enviar Solicitação de Cancelamento###
Uma solicitação de cancelamento pode ser enviada, caso o pedido se encontre em um estado que se possa cancelar, o pedido será cancelado. 
 
*Exemplos de Request de Cancelamento - Endpoint VTEX*

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderid]/cancel**  
verb: **GET**

##4 Versão:Beta 1.1##
Essa versão de documentação suporta a integração na versão da plataforma VTEX smartcheckout. Ela foi escrita para auxiliar um integração e a idéia e que através dela, não  restem nenhuma dúvida de como se integrar com a VTEX. Se recebeu essa documentação e ainda restaram dúvidas, por favor, detalhe as suas dúvidas abaixo no comentário, para chegarmos a um documento rico e funcional.


autor: Jonas Bolognim