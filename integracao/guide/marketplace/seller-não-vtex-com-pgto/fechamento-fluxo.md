# Simulação de Carrinho e Consulta Parcelamento

Este documento tem por objetivo auxiliar o integrador na simulação de carrinho entre o marketplace VTEX  com uma loja não VTEX. Simular um pedido e consultar as formas de parcelamento.

##1 - No Carrinho e no Pagamento##
Quando um produto é inserido no carrinho no marketplace VTEX, ou faz se alguma edição no carrinho, uma consulta de simulaçao de carrinho é feita no Seller para checar a validade das condiçoes comerciais(preço, estoque, frete e SLAs de entrega).  

*Exemplo do fuxo de chamadas no carrinho:*  

![alt text](fechamento-fluxo.png "Title")  

###1.1 - Exemplos de Request de Simulação de Carrinho - Endpoint do Seller###

endpoint: **https://sellerendpoint/pvt/orderForms/simulation?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc é o id do canal de vendas


*Exemplo do Request:*  

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


*Exemplo do Response:*

	    {
        "items": [                                                     //pode vir um array vazio
            {
                "id": "287611",                                        //obrigatório, string
                "requestIndex": 0,                                     //obrigatório, int - representa a posição desse item no array original (request)
                "price": 7390,                                         // Os dois dígitos menos significativos são os centavos //obrigatório, int
                "listPrice": 7490,                                     // Os dois dígitos menos significativos são os centavos //obrigatório, int
                "quantity": 1,                                         //obrigatório, int
                "seller": "1",                                         // Id do seller cadastrado na loja // obrigatório, string,
            	"merchantName": "shopfacilfastshop",				   //**devolver o parametro an, so deve ser preenchido quando o pagamento for processado no seller.
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
				"merchantName": "shopfacilfastshop",	
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

**somente mandar esse campo quando o pagamento for processado no Seller.

###1.2 - Exemplos de Request de Consulta de Opções de Parcelamento - Endpoint do Seller###

endpoint: **https://sellerendpoint/installments/options?an=[nomedaloja]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **an=nomedaloja**

*Exemplo do Request:*  

	{
	  "PaymentSystemsIds":[1,2], //ids das formas de pagamento
	  "SubtotalAsInt":27280, //total que deseja parcelar
	  "Items":[
	    {
	      	"PriceAsInt":24800, //preço do SKU
	     	"Quantity":1, //quantidade do SKU
	     	"Id":1940388, //id do SKU
	     	"SellerId":"seller1",
	    	"SalesChannel":2 //id do canal de vendas criado para o seller
	    }
	  ],
	  "postalCode":"22051030" //CEP
	}

*Exemplo do Response:*

	[
	    {
	        "paymentSystem": 2,
	        "name": "",
	        "value": 27280,
	        "installments": [
	            {
	                "count": 1,
	                "value": 27280,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 2,
	                "value": 13640,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 3,
	                "value": 9093,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 4,
	                "value": 6820,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 5,
	                "value": 5456,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 6,
	                "value": 4547,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 7,
	                "value": 3897,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 8,
	                "value": 3410,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 9,
	                "value": 3031,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 10,
	                "value": 2728,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 11,
	                "value": 2630,
	                "interestRate": 99,
	                "hasInterestRate": true
	            },
	            {
	                "count": 12,
	                "value": 2422,
	                "interestRate": 99,
	                "hasInterestRate": true
	            }
	        ]
	    },
	    {
	        "paymentSystem": 1,
	        "name": "",
	        "value": 27280,
	        "installments": [
	            {
	                "count": 1,
	                "value": 27280,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 2,
	                "value": 13640,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 3,
	                "value": 9093,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 4,
	                "value": 6820,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 5,
	                "value": 5456,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 6,
	                "value": 4547,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 7,
	                "value": 3897,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 8,
	                "value": 3410,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 9,
	                "value": 3031,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 10,
	                "value": 2728,
	                "interestRate": 0,
	                "hasInterestRate": false
	            },
	            {
	                "count": 11,
	                "value": 2630,
	                "interestRate": 99,
	                "hasInterestRate": true
	            },
	            {
	                "count": 12,
	                "value": 2422,
	                "interestRate": 99,
	                "hasInterestRate": true
	            }
	        ]
	    }
	]


##2 Versão:Beta 1.1##
Essa versão de documentação suporta a integração na versão da plataforma VTEX smartcheckout. Ela foi escrita para auxiliar um integração e a idéia e que através dela, não  restem nenhuma dúvida de como se integrar com a VTEX. Se recebeu essa documentação e ainda restaram dúvidas, por favor, detalhe as suas dúvidas abaixo no comentário, para chegarmos a um documento rico e funcional.


autor: Jonas Bolognim