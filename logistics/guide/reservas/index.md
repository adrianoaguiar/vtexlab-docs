---
layout: docs
title: Reservas
application: logistics
docType: guide
---

# Reservas

Reserva é o processo de assegurar ou priorizar temporariamente o direito de compra de um produto disponível em estoque.

A reserva permanece ativa e autorizada enquanto não é confirmada, o que na maior parte dos casos ocorre quando o solicitante recebe a confirmação de pagamento do pedido.

A qualquer momento uma reserva pode ser cancelada, transformando os itens novamente disponíveis para venda.'

# Fluxo da reserva
{: #FluxoDaReserva .slug-text}

## Reservas e inventário

No VTEX Logistics, um produto pode ser associado a diferentes estoques e com diferentes quantidades.
A quantidade disponível para venda é exatamente a diferença entre a quantidade total do item e a quantidade reservada.

A imagem abaixo ilustra o cenário de um produto que está associado a dois estoques e sem unidades reservadas.

Quando uma reserva é realizada, a quantidade disponível para venda é automaticamente diminuída.
Simulando a reserva de três unidades do produto do exemplo anterior, o cenário de disponibilidade deste fica o seguinte:

Se a reserva for confirmada e em seguida reconhecida pelo ERP, a quantidade total do produto é diminuída em três e a quantidade reservada torna-se zero, como na ilustração:

Caso a reserva seja cancelada, a quantidade total do produto volta ao valor anterior à realização da reserva e a quantidade reservada torna-se zero.
