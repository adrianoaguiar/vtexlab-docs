---
layout: docs
title: Integração de Pedidos e Tracking
application: erp
docType: guide
---

# Integração de Pedidos e Tracking

Este documento tem por objetivo auxiliar o integrador na integração pedidos entre ERP e uma loja hospedada na versão smartcheckout da VTEX. Ler os pedidos na VTEX, inserir os pedidos no ERP, enviar as informações de nota fiscal e tracking e ou cancelamento de pedido.

_Exemplo do Fluxo:_

![alt text](pedido-tracking-vtex-erp.PNG "Title")

## Pedidos

Obter a lista de pedidos na VTEX e inserir os pedidos no ERP, atualizando a VTEX que o pedido já está no ERP.

### Obter a Lista de Pedidos por Status na API do OMS
{: #1 .slug-text}

Através da API do OMS pegar a lista de pedidos pagos paginados:

<a title="obter lista de pedidos por status" href="http://vtex-bridge-1-0-beta.elasticbeanstalk.com/vtex.bridge.web_deploy/swagger/ui/index.html#!/OMS/OMS_Orders" target="_blank">Exemplo de chamada para obter uma lista de pedidos por status no SWAGGER</a>


Esse exemplo retorna uma lista com o resumo de cada pedido (2 pedidos), onde para cada pedido, deve fazer uma chamada na API REST do OMS para pegar o pedido completo, passando o "orderId" do pedido. Segue exemplo de chamada a API REST para pegar um pedido.

### Obter um Pedido na API do OMS
{: #Obter-um Pedido na API do OMS .slug-text}

<a title="obter pedido por identificador" href="http://vtex-bridge-1-0-beta.elasticbeanstalk.com/vtex.bridge.web_deploy/swagger/ui/index.html#!/OMS/OMS_Order_0" target="_blank">Exemplo de chamada para obter um  pedidos pelo identificador.</a> 


### Obter Informações de Pagamento de Pedido
{: #2 .slug-text}

Caso necessário obter informações sobre informações de pagamento de um pedido (como endereço de cobrança por exemplo), deve se acessar a API REST de **Payments** passando o *TID ("paymentData.transactions.transactionId": "33CD3CC4D11A4FA49A2C9EE20D771F98") do gateway VTEX.

No retorno, além de um resumo da transação, poderá obter se as URLs de acesso aos detalhes transação. Segue exemplo de chamada a API REST para pegar as informações de pagamento de um pedido.

<a title="obter dados possiveis de pagamento" href="http://vtex-bridge-1-0-beta.elasticbeanstalk.com/vtex.bridge.web_deploy/swagger/ui/index.html#!/PCI/PCI_Get" target="_blank">Exemplo de chamada para obter dados possiveis de pagamento de um pedido</a> 

### Pedido Está no ERP - Preparando Entrega
{: #3 .slug-text}

Uma vez tendo os dados de pedidos obtidas na API do OMS da VTEX, guarda se o pedido
no respectivo ERP e informa se a VTEX que o pedido está sendo tratado pelo processos de preparar entrega.

<a title="pedido sendo tratado" href="http://vtex-bridge-1-0-beta.elasticbeanstalk.com/vtex.bridge.web_deploy/swagger/ui/index.html#!/OMS/OMS_StartHandling" target="_blank">Exemplo de chamada para avisar OMS que o pedido já se encontra no ERP</a> 


###Envio de Nota Fiscal
{: #4 .slug-text}

Uma vez o pedido no ERP e o status do pedido na loja VTEX como preparando entrega, vem a parte da Nota Fiscal.
Após receber o pedido, o ERP emite a Nota Fiscal do Pedido e informa a loja VTEX sobre a mesma.

O envio de notas fiscais pode ser parcial, obrigando assim ao informador informar além dos valores da nota fiscal, os items ele está mandando na nota fiscal parcial. Exemplo do POST feito no OMS da VTEX para informar uma Nota Fiscal.

<a title="enviando Nota Fiscal para o MOS" href="http://vtex-bridge-1-0-beta.elasticbeanstalk.com/vtex.bridge.web_deploy/swagger/ui/index.html#!/OMS/OMS_Order" target="_blank">Exemplo de chamada para enviar Nota Fiscal para o OMS</a> 

###Envio de Tracking
{: #5 .slug-text}

Uma vez informado a Nota Fiscal, vem a parte de ratreamento da Entrega.
O ERP ou a transportadora podem enviar informações de como e onde anda o pedido através da API do OMS VTEX

<a title="enviando tracking para o OMS" href="http://vtex-bridge-1-0-beta.elasticbeanstalk.com/vtex.bridge.web_deploy/swagger/ui/index.html#!/OMS/OMS_Order" target="_blank">Exemplo de chamada para enviar Tracking de Entrega para o OMS</a> 

**A Nota Fiscal** e o **Tracking** podem ser enviados na mesma chamada, basta prenncher todos os dados do POST.

### Solicitando Cancelamento
{: #6 .slug-text}

O pedido desceu pro ERP, mas por alguma motivo foi cancelado. O ERP invoca uma solicitação de cancelamento
para a API do OMS da loja VTEX. Caso o pedido ainda esteja num estado em que se possa cancelar, o mesmo será cancelado.

<a title="solicitando cancelamento" href="http://vtex-bridge-1-0-beta.elasticbeanstalk.com/vtex.bridge.web_deploy/swagger/ui/index.html#!/OMS/OMS_Cancel" target="_blank">Exemplo de chamada para solicitar cancelamento no OMS</a>  

autor:_Jonas Bolognim_  
propriedade: _VTEX_