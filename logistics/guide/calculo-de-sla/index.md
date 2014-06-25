---
layout: docs
title: Cálculo de SLA
application: logistics
docType: guide
---

# Cálculo de SLA
{: #CálculoDeSLA .slug-text}

Cálculo de SLA é o processo que verifica a disponibilidade e define preço e prazo de entrega de um produto.

Para a realização do cálculo é necessário que seja informado uma coleção de SKU's, o endereço desejado de entrega e o canal de vendas.

Para que haja possibilidades de entregar uma encomenda, as premissas abaixo devem ser atendidas:

* Os produtos solicitados devem ter quantidade disponível em algum estoque

* Os estoques com os produtos disponíveis devem ter ligação a algum Centro de Distribuição que atenda ao Canal de Vendas informado

* Os Centros de Distribuição selecionados devem ser atendidos por Tramsportadoras

* As transportadoras possíveis devem conseguir entregar no endereço desejado e aceitar encomendas com o peso e volume do(s) produto(s)

## Dados de entrada
{: #DadosDeEntrega .slug-text}

| Campo             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Id** | Valor que identifica unicamente o SKU|
| **GroupItemId** |
Valor que agrupa os itens em uma determinada promoção. Os SKU's com de uma mesma promoção devem ser entregues na mesma encomenda|
| **KitItem** | Utilizado nos casos de Kit, ou seja, quando o SKU na verdade é composto por uma coleção de SKU's |
| **Quantity** | Indica quantas unidades do SKU deseja-se entregar. Caso a quantidade informada não esteja disponível, o cálculo é realizado com base na quantidade disponível em estoque |
| **Price** | Preço de venda do SKU. Em alguns casos, esse valor influencia no preço final do entrega |
| **AdditionalHandlingTime** | Valor extra de tempo a ser adicionado no tempo calculado para entrega |
| **Dimension** | As dimensões do item envolvem o peso, altura, largura e comprimento e são de extrema importancia durante o cálculo|
{: .doc-api-table }

O endereço de entrega deve conter os seguintes dados:

| Campo             | Descrição                     |
| -----------------------:| :-----------------------------|
| **ZipCode** | CEP do endereço|
| **DeliveryPointId** | Utilizado no caso em que a entrega deve ser feita em um ponto de entrega previamente cadastrado|
| **Country** | País onde a entrega será realizada|
{: .doc-api-table }

## SLA de resposta

Em contrapartida, o retorno do cálculo é uma coleção de possibilidades de entrega para cada SKU.
Se na resposta no cálculo não houver referência a algum SKU solicitado, significa que este item não pode ser entregue.

A resposta do cálculo retorna as seguintes informações:

| Campo             | Descrição                     |
| -----------------------:| :-----------------------------|
| **ItemId** | Identificador do SKU|
| **Quantity** | Quantidade solicitada para entrega|
| **AvailabilityQuantity** | Quantidade disponível para entrega|
| **SalesChannel** | Canal de vendas utilizado no cálculo|
| **SlaType** | Tipo de entrega|
| **FreightTableName** | Nome da transportadora que pode realizar a entrega|
| **FreightTableId** | Identificador da transportadora que pode realizar a entrega|
| **ListPrice** | Valor do frete|
| **TransitTime** |Tempo utilizado pela transportadora para realizar a entrega|
| **DockTime** |Tempo de manuseio no centro de distribuição|
| **TimeToDockPlusDockTime** |Tempo de manuseio no centro de distribuição mais o tempo de transito do estoque ao centro de distribuição|
| **TotalTime** |Tempo total da entrega|
| **DeliveryWindows** |Janelas de entrega agendada|
| **DockId** |Identificador do centro de distribuição|
| **Location** |Dados do endereço de entrega|
| **DeliveryOnWeekends** |Informa se o tempo da entrega é em dias úteis ou corridos|
| **CarrierSchedule** |Dias da semana em que a transportadora passa no centro de distribuição para retirada dos itens|
| **RestrictedFreight** |Informações de restrição na entrega|
{: .doc-api-table }

## Passo a passo

Para estabelecer o preço e prazo de entrega, alguns passos e validações são seguidos:

* **Disponibilidade em estoque:** a primeira etapa do cáclulo é verificar a quantidade disponível para venda. Se a quantidade desejada não for atendida, o cálculo continua com a quantidade total disponível, ou seja, é necessário haver ao menos uma unidade disponível de um dos itens desejados.

* **Montar rotas de entrega:** ao identificar os estoques com os itens disponíveis, são traçadas todas as possíveis rotas de entrega entre essses estoques e as transportadoras. Nesse sentido, é necessário observar com atenção à associação entre os estoques, docas e transportadoras cadastradas na loja. Deve haver pelo menos uma rota de entrega possível, do contrário o cálculo não será realizado.

* **Limitação das transportadoras:** para todas as transportadoras envolvidas nas rodas desobertas no passo anterior, verifica-se as seguintes premissas:
	* Entrega no endereço desejado?
	* Entrega encomendas com o peso total dos itens? Se não, é possível entregar o peso dos itens separados em pacotes mais leves?
	* Entrega encomendas com o volume total dos itens?
	* Aceita entregar encomendas com as dimensões (altura, largura e comprimento) dos itens?

* **Calcula o preço:** o preço final do frete é definido a partir da soma das seguintes variáveis:
	* Valor absoluto do frete: cadastrado na tabela de valores de frete, é o preço base para entregar o peso da encomenta no endereço desejado
	* Adicional por preço dos produtos: ao valor final do frete é adicionado um valor percentual ao preço dos produtos
	* Adicional por peso da encomenda: ao valor final do frete é adicionado a um valor por cada grama da encomenda
	* Custo de transito da encomenda entre o estoque e o centro de distribuição

* **Define prazo de entrega:** o prazo final da entrega é definido pela soma das seguintes variáveis:
* Prazo de entrega da transportadora: cadastrado na tabela de valores de frete, é o tempo utilizado pela transportadora pra realizar a entrega a partir do momento que recolhe os produtos nos centros de distribuição
* Tempo de manuseio no centro de distribuição: é o tempo de separação e manuseio dos produtos nos centros de distribição
* Tempo de manuseio no estoque: é o tempo de separação e manuseio dos produtos nos estoques

* **Ordenação:** dentre todas as possibilidades de entrega identificadas anteriormente, é retornado a melhor opção para cada tipo de entrega. O ordenação segue o seguinte método:
* **Menor número de pacotes:** a prioridade é das transportadoras que conseguem entregar a encomenda sem dividí-la em pacotes.
* **Menor preço:** a segunda ordem de priorização é o preço final do frete. Quanto mais barato, mais prioritária;
* **Menor tempo de entrega:** quanto mais rápida a entrega, mais prioritária a transportadora;
* **Prioridade:** valor informado no cadastro dos centros de distribuição e define a última ordem de priorização das rotas/transportadoras escolhidas. Quanto maior o valor cadastrado, mais prioritária é a rota.

