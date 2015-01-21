##Simulação de Carrinho e Página de Pagamento

Este tópico tem por objetivo auxiliar o na simulação de carrinho, e consulta de formas de pagamento e  parcelamentos entre um canal de vendas não VTEX com uma loja VTEX.

###No Carrinho e no Pagamento
Quando um produto é inserido no carrinho no canal de vendas não VTEX, ou faz se alguma edição no carrinho, uma consulta de simulaçao de carrinho deverá ser feita na loja VTEX para checar a validade das condiçoes comerciais (preço, estoque, frete e SLAs de entrega). Quando o cliente vai para o pagamento, uma consulta as formas de pagamento, aos parcelmentos e uma outra simulçao de carrinho deverá ser realizada.

*Fluxo de chamadas no carrinho e no pagamento:*  

![alt text](fechamento-canal-nao-vtex-com-pgto.png "fechamento do pedido no marketplace")  

####Exemplos de Request de Simulação de Carrinho - Endpoint loja VTEX###

endpoint: **https://[loja].vtexcommercestable.com.br/api/fulfillment/pvt/orderForms/simulation?sc=[idcanal]&affiliateId=[idafiliado]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc** // sc é o canal de vendas  
Parametro: **affiliateId** // affiliateId é o id do afiliado cadastrado na loja VTEX

*request:*  

	{
        "postalCode":"22251-030",            //obrigatório se country estiver preenchido, string
        "country":"BRA",                     //obrigatório se postalCode estiver preenchido, string      
        "items": [                           //obrigatório: deve conter pelo menos um objeto item
            {
                "id":"287611",               //obrigatório, string
                "quantity":1,                //obrigatório-quantidade do item a ser simulada, int
                "seller":"seller1"           //sigla do do seller criado no admin // obrigatório, string
            },
            {
                "id":"5837",
                "quantity":5,
                "seller":"seller1"
            }
        ]
    }


*response:*

	    {
        "items": [                                                     //pode vir um array vazio
            {
                "id": "287611",                                        //obrigatório, string
                "requestIndex": 0,                                     //obrigatório, int - representa a posição desse item no array original,
                "price": 7390,                                         // Os dois dígitos menos significativos são os centavos //obrigatório, int
                "listPrice": 7490,                                     // Os dois dígitos menos significativos são os centavos //obrigatório, int
                "quantity": 1,                                         //obrigatório, int
                "seller": "1",                                         // Id do seller cadastrado na loja // obrigatório, string,
				"merchantName": "epoca",							   // name do gateway de pagamento, será usado ao enviar o pedido
                "priceValidUntil": "2014-03-01T22:58:28.143"           //data, pode ser nulo
                "offerings":[                                           //Array opcional, porém não pode ser nulo: enviar array vazio ou não enviar a propriedade
                    {
                        "type":"Garantia",                               //obrigatório, string
                        "id":"5",                                       //obrigatório, string
                        "name":"Garantia de 1 ano",                       //obrigatório, string
                        "price":10000                                   //Os dois dígitos menos significativos são os centavos //obrigatório, int
                    },
                    {
                        "type":"Embalagem de Presente",
                        "id":"6",
                        "name":"Embalagem de Presente",
                        "price":250                                       
                    }
                ]
            },
            {
                "id": "5837",
                "requestIndex": 1,
                "price": 890,                                          // Os dois dígitos menos significativos são os centavos
                "listPrice": 990,                                      // Os dois dígitos menos significativos são os centavos
                "quantity": 5,
                "seller": "1",
				"merchantName": "epoca",							   
                "priceValidUntil": null
            }
        ],
        "logisticsInfo": [                                            //obrigatório (se vier vazio é considerado que o item não está disponível) -  todos os itens devem ter os mesmos SLAs
            {
                "itemIndex": 0,                                       //obrigatório, int - representa os dados de sla do item de resposta (response)
                "stockBalance": 99,                                   //obrigatório  quando o CEP foi passado no request, estoque, int
                "quantity": 1,                                        //obrigatório quando o CEP foi passado no request, qauntidade pasada no request, int
                "shipsTo": [ "BRA", "USA" ],                          //obrigatório, array de string com as siglas dos países de entrega
                "slas": [                                             //obrigatório quando o CEP foi passado no request. Pode ser um array vazio
                    {
                        "id": "Expressa",                             //obrigatório, id tipo entrega, string
                        "name": "Entrega Expressa",                   //obrigatório, nome do tipo entrega, string
                        "shippingEstimate": "2bd",                    // bd == "business days" //obrigatório, string
                        "price": 1000                                 // Os dois dígitos menos significativos são os centavos, obrigatório, int
                        "availableDeliveryWindows": [                 //opcional, podendo ser um array vazio
                        ]
                    },
                    {
                        "id": "Agendada",
                        "name": "Entrega Agendada",
                        "shippingEstimate": "5d",                     // d == "days, bd == "business days"
                        "price": 800,
                        "availableDeliveryWindows": [
                             {
                                "startDateUtc": "2013-02-04T08:00:00+00:00",       //date, obrigatório se for enviado delivery window
                                "endDateUtc": "2013-02-04T13:00:00+00:00",         //date, obrigatório se for enviado delivery window
                                "price": 0        //int, obrigatório se for enviado delivery window - o valor total da entrega agendada é o valor base mais o valor desse campo
                            },
                        ]
                    }
                ]
            },
            {
                "itemIndex": 1,
                "stockBalance": 1237,
                "quantity": 5,
                "shipsTo": [ "BRA" ],
                "slas": [
                    {
                        "id": "Normal",
                        "name": "Entrega Normal",
                        "shippingEstimate": "5bd",                                  // bd == "business days"
                        "price": 200
                    }
                ]
            }
        ],
        "country":"BRA",                                           //string, nulo se não enviado
        "postalCode":"22251-030"                                   //string, nulo se não enviado    
    }


####Exemplos de Request de Consulta de Forma de Pagamento - Endpoint loja VTEX

endpoint: **https://sandboxintegracao.vtexpayments.com.br/api/pvt/merchants/payment-systems**  
verb: **GET**  
Content-Type: **application/json**  
Accept: **application/json**  

*response:*  

	[
	    {
	        "id": 6,
	        "name": "Boleto Bancário",
	        "connectorId": 0,
	        "requiresDocument": false,
	        "implementation": "Vtex.PaymentGateway.BankIssuedInvoice.BankIssuedInvoicePayment",
	        "connectorImplementation": "Vtex.PaymentGateway.Connectors.BankIssuedInvoiceBBConnector",
	        "groupName": "bankInvoice",
	        "isCustom": false,
	        "isSelfAuthorized": false,
	        "allowInstallments": false,
	        "isAvailable": true,
	        "description": null,
	        "validator": {
	            "regex": null,
	            "mask": null,
	            "cardCodeMask": null,
	            "cardCodeRegex": null,
	            "weights": null,
	            "useCvv": false,
	            "useExpirationDate": false,
	            "useCardHolderName": false,
	            "useBillingAddress": false,
	            "validCardLengths": null
	        },
	        "dueDate": "2015-01-19T14:49:14.4767186Z"
	    },
	    {
	        "id": 2,
	        "name": "Visa",
	        "connectorId": 0,
	        "requiresDocument": true,
	        "implementation": "Vtex.PaymentGateway.CreditCard.Visa",
	        "connectorImplementation": "Vtex.PaymentGateway.Connectors.AdyenConnector",
	        "groupName": "creditCard",
	        "isCustom": false,
	        "isSelfAuthorized": false,
	        "allowInstallments": true,
	        "isAvailable": true,
	        "description": null,
	        "validator": {
	            "regex": "^4",
	            "mask": "9999 9999 9999 9999",
	            "cardCodeMask": "999",
	            "cardCodeRegex": "^[0-9]{3}$",
	            "weights": [2,1,2,1,2,1,2,1,2,1,2,1,2,1,2,1,2,1,2],
	            "useCvv": true,
	            "useExpirationDate": true,
	            "useCardHolderName": true,
	            "useBillingAddress": true,
	            "validCardLengths": null
	        },
	        "dueDate": "2015-01-17T14:49:14.4767186Z"
	    },
	    {
	        "id": 4,
	        "name": "Mastercard",
	        "connectorId": 0,
	        "requiresDocument": true,
	        "implementation": "Vtex.PaymentGateway.CreditCard.Mastercard",
	        "connectorImplementation": "Vtex.PaymentGateway.Connectors.AdyenConnector",
	        "groupName": "creditCard",
	        "isCustom": false,
	        "isSelfAuthorized": false,
	        "allowInstallments": true,
	        "isAvailable": true,
	        "description": null,
	        "validator": {
	            "regex": "^5(1(0(0(0([0-9])|[1-9][0-9])|[1-9][0-9]{0})|[1-9][0-9]{0})|3(0(4(0([0-9]))|[0-3][0-9]{0}))|2[0-9]{0})|^5(3(0(4(2([0-9])|[3-9][0-9])|[5-9][0-9]{0})|[1-9][0-9]{0})|5(9(9(9([0-9])|[0-8][0-9])|[0-8][0-9]{0})|[0-8][0-9]{0})|4[0-9]{0})",
	            "mask": "9999 9999 9999 9999",
	            "cardCodeMask": "999",
	            "cardCodeRegex": "^[0-9]{3}$",
	            "weights": [2,1,2,1,2,1,2,1,2,1,2,1,2,1,2,1,2,1,2],
	            "useCvv": true,
	            "useExpirationDate": true,
	            "useCardHolderName": true,
	            "useBillingAddress": true,
	            "validCardLengths": null
	        },
	        "dueDate": "2015-01-17T14:49:14.4767186Z"
	    }
	]



####Exemplos de Request de Consulta de Parcelamento Por Forma Pagamento e SKU - Endpoint loja VTEX

endpoint: **https://sandboxintegracao.vtexpayments.com.br/api/pvt/installments/options**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  

_request:_  

	{
	  "PaymentSystemsIds":[2,4],
	  "SubtotalAsInt":81200,
	  "Items":[
	    {
	      	"PriceAsInt":81200,
	     	"Quantity":1,
	     	"Id":2000037,
	     	"SellerId":"1",
	    	"SalesChannel":1
	    }
	  ]
	}

_response:_  

	[
	    {
	        "paymentSystem": 2,
	        "name": "Visa 3 vezes sem juros",
	        "groupName": "creditCard",
	        "value": 81200,
	        "installments": [
	            {
	                "count": 3,
	                "value": 27066,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 2,
	                "value": 40600,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 1,
	                "value": 81200,
	                "interestRate": 0,
	                "hasInterestRate": false
	            }
	        ]
	    },
	    {
	        "paymentSystem": 4,
	        "name": "Mastercard 3 vezes sem juros",
	        "groupName": "creditCard",
	        "value": 81200,
	        "installments": [
	            {
	                "count": 3,
	                "value": 27066,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 2,
	                "value": 40600,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 1,
	                "value": 81200,
	                "interestRate": 0,
	                "hasInterestRate": false
	            }
	        ]
	    }
	]

##2 Versão:Beta 1.1##
Essa versão de documentação suporta a integração na versão da plataforma VTEX smartcheckout. Ela foi escrita para auxiliar um integração e a idéia e que através dela, não  restem nenhuma dúvida de como se integrar com a VTEX. Se recebeu essa documentação e ainda restaram dúvidas, por favor, detalhe as suas dúvidas abaixo no comentário, para chegarmos a um documento rico e funcional.

 
autor: _Jonas Bolognim_  
propriedade: _VTEX_