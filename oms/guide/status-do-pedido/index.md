---
layout: docs
title: Status do pedido
application: oms
docType: guide
---

# Status do pedido
{: #StatusDoPedido .slug-text}

Para entender melhor os status, consulte também o [Fluxo do pedido](/docs/oms/guide/fluxo-do-pedido/).

### Mudanças de status
{: #MudançaDeStatus .slug-text}

A maior parte das mudanças de status são automáticas, com algumas exceções em que é necessária uma interação manual, seja através do OMS ou de uma integração.

A passagem de um *status de ação *(ou* status de ação de cancelamento*) para um *status principal* é sempre automática. Um pedido só se mantém em um *status de ação* caso haja algum erro.

### Situações de erro
{: #SituaçõesDeErro .slug-text}

Erros podem ocorrer em qualquer status. Em situações de erro, o sistema efetua até cinco tentativas de seguir para o próximo status. Após isso, o pedido fica parado, aguardando uma ação manual para tentar seguir para o próximo status.

### Entendendo os status
{: #EntendendoOsStatus .slug-text}

| Status             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Aguardando confirmação do Seller** | O Marketplace recebe o pedido, solicita e aguarda a confirmação de recebimento do pedido pelo Seller. Neste momento, os produtos são reservados no sistema de Logística|
| **Pagamento pendente** | Após aceitação do Seller, o Marketplace aguarda a aprovação do pagamento pelo PCI Gateway.|
| **Pagamento aprovado** | Aprovado o pagamento pelo PCI Gateway
| **Fluxo completo do pedido**| o pedido segue automaticamente para Carência para cancelamento.|
| **Fluxo do pedido do Marketplace**| o Seller recebe uma autorização para despachar e o pedido fica aguardando a confirmação de fatura do Seller para seguir para Faturado|
| **Aguardando decisão do Seller** | O pedido fica aguardando a confirmação do cancelamento.|
| **Aguardando autorização para despachar** | Fluxo do pedido do Seller: o pedido aguarda o Marketplace enviar uma autorização para despachar.|
| **Carência para cancelamento** | O pedido fica, por padrão, 30 minutos nesse status. Durante esse tempo, o cliente poderá, caso tenha comprado errado ou se arrependido, fazer o cancelamento do pedido antes que este siga para o ERP. Após esse tempo, o pedido segue automaticamente para Pronto para manuseio.|
| **Pronto para manuseio** | Passado o tempo de carência, o pedido fica aguardando seu manuseio. Em geral, é nesse momento em a integração com o ERP é feita.|
| **Preparando entrega** | As reservas dos produtos são confirmadas no sistema de Logística. O pedido fica em manuseio, aguardando notificações de fatura (invoice ou nota fiscal), geralmente vindas do ERP.|
| **Cancelamento solicitado** | O pedido fica aguardando a confirmação do cancelamento. O vendedor decidirá de acordo com a stiuação do manuseio do pedido se ainda é possível realizar o cancelamento, pois algum item pode já estar seguindo para a transportadora.|
| **Cancelado** | O pedido é finalizado sem sucesso. A transação de pagamento no PCI Gateway é cancelada juntamente.|
| **Faturado** | Pedido faturado |
| **Fluxo completo do pedido** |O pedido é finalizado com sucesso.|
| **Fluxo do pedido do Marketplace** | O pedido é finalizado com sucesso.|
| **Fluxo do pedido do Seller** | O pedido é finalizado com sucesso e o Marketplace recebe a confirmação de fatura.|
{: .doc-api-table }