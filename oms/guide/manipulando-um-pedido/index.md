---
layout: docs
title: Manipulando um pedido
application: oms
docType: guide
---

#Manipulando um pedido

Ao consultar um pedido, você encontrará todas as informações e ações disponíveis para a situação em que ele se encontra.

### Informações de um pedido
{: #InformaçõesDeUmPedido .slug-text}

| Informação             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Número identificador ou Order ID** | Identificador único dentro da loja, que deve ser usado para comunicação com o cliente|
| **Número sequencial** | Número utilizado para identificação e integração do pedido com o ERP |
| **Data e hora** | Data e hora em que o pedido foi efetuado |
| **Nome do Seller** | Nome do Seller responsável pelo pedido |
| **Nome do Operador de televendas** | Nome do Operador de televendas responsável pelo pedido. (Apenas nas compras efetuadas por um Operador de televendas) |
| **Dados do cliente** | Dados de Pessoa Física ou Pessoa Jurídica |
| **Endereço de entrega** | Nome do destinatário, CEP, Logradouro, número, complemento, referência, bairro, cidade e estado |
| **Nome da Lista de presentes** | Nome da Lista de presentes a qual o pedido está vinculado. (Apenas nas compras efetuadas para uma Lista de presentes) |
| **Valores totais** | Valor do pedido, dos descontos, das taxas e da entrega |
| **Status do pedido** | Situação do pedido no momento. Veja <a href="/docs/oms/guide/fluxo-do-pedido/">como ocorre o fluxo do pedido</a> para mais detalhes |
| **Status da transação** | Situação da transação financeira no momento. Veja como ocorre o fluxo da transação para mais detalhes. |
| **Interações** |Detalhamneto técnico sobre o andamento do pedido, com informações sobre todas as ocorrências registradas. |
| **Faturas** | Informações sobre as faturas e respectivos itens que as compõem. As faturas podem ser geradas manualmente no OMS ou via integração. (Apenas quando há itens faturados). |
| **Itens** | Lista dos produtos que compõem o pedido e/ou a fatura. |
| **Dados de pagamento** | Lista com a(s) forma(s) de pagamento utilizada(s) e informações complementares. (Apenas quando há pagamentos registrados). |
| **Endereço de cobrança** | Endereço de cobrança do cliente. (Apenas quando há pagamentos registrados). |
{: .doc-api-table }

### Ações para um pedido
{: #AçõesParaUmPedido .slug-text}

Para entender bem as ações descritas a seguir, sugerimos que você consulte [como ocorre o fluxo do pedido](/docs/oms/guide/fluxo-do-pedido/).

| Ação             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Cancelar pedido** | A ação de cancelar pode ser solicitada em qualquer momento antes que o pedido esteja Faturado. Em alguns casos, o cancelamento passará por uma aceitação do Seller. |
| **Forçar retentativa quando em situação de erro** | Em algumas situações, pode haver problemas no andamento do pedido. Nesses casos, o sistema faz até 5 tentativas de continuar com o andamento automaticamente. Caso o problema persista, é necessária uma intervenção manual, forçando a retentativa. |
| **Alterar status para Autorizar despacho** | Quando o pedido está em <em>Aguardando autorização para despachar</em>, é possível forçar a autorização, mudando o pedido para <em>Autorizar despacho</em>. |
| **Alterar status para Pronto para manuseio** | Quando o pedido está em <em>Carência para cancelamento</em>, é possível forçar o fim da carência, mudando o pedido para <em>Pronto para manuseio</em>. |
| **Alterar status para Iniciar manuseio** | Quando o pedido está em <em>Pronto para manuseio</em>, é possível forçar o início do manuseio, mudando o pedido para <em>Iniciar manuseio</em>. |
| **Registro manual de Nota Fiscal** | Quando o pedido está em <em>Preparando entrega</em> ou <em>Cancelamento solicitado</em>, dados na Nota Fiscal podem ser inseridos manualmente através do OMS, gerando uma nova fatura. |
{: .doc-api-table }