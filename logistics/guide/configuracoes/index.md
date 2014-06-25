---
layout: docs
title: Configurações
application: logistics
docType: guide
---

# Configurações

## Transportadoras
{: #Transportadora .slug-text}

Transportadora é o operador logístico que de fato realiza a entrega. É ela a responsável por retirar o produto em um centro de distribuição e levá-lo a um determinado endereço.No VTEX Logistics, uma transportadora pode estar associada a uma ou mais docas, ou seja, uma mesma transportadora pode retirar produtos de uma ou mais docas.

Associado às transportadoras deve haver uma coleção de valores de frete, que indicam, dentre outras coisas, o preço e o prazo cobrado pela transportadora para realizar a entrega em um determinado código postal.

O cadastro da entidade transportadora exige os seguintes dados:

| Campo             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Id** | Identificador único da transportadora. Valor alfanumérico, mas não é permitido caracteres especiais. Ex: 12345, transp01, correios_sedex, 10cabd5, etc.|
| **Nome** | Nome da transportadora. Ex: Transportadora XYZ, etc. |
| **Tipo da entrega** | Campo livre que melhor descreve a forma de entrega da transportadora. Ex: Normal, Expressa, PAC, SEDEX, Entrega Agendada, etc. Para entregas do tipo Agendada, é necessário configurar os dias, horários e taxas de agendamento. |
| **Fator cúbico de peso** | Valor utilizado pela transportadora para equilibrar a relação peso x espaço ocupado pela carga transportada. É um decimal positivo com até 6 casas decimais. Ex: 0.000167, 1.0 e 1.001. |
| **Peso cúbico mínimo** | **Beta** Valor mínimo para que o peso cúbico da carga possa ser utilizado no cálculo do frete.É um decimal positivo com até 6 casas decimais. Ex:100.99, 2000.00 e 30000. |
| **Entrega aos finais semana** | **Beta** Informa se a transportadora trabalha aos finais de semana e feriados ou apenas em dias úteis. Essa informação é utilizada na estimativa final do tempo de entrega. Imaginando uma compra realizada numa sexta-feira com estimativa de 3 dias para entrega, ocorre que:- Se a transportadora funciona aos finais de semana, a entrega será feita na segunda-feira, ou seja, o prazo para entrega permanece em 3 dias..- Se não funciona aos finais de semana, a entrega será concluída apenas na quarta-feira, ou seja, é acrescido 2 dias na estimativa final da entrega |
| **Datas de corte** | **Beta** Dias e horários da semana em que a transportadora recolhe a carga nos centros de distribuição. Esse dado é utilizado na estimativa final do tempo de entrega. Imaginando que a transportadora recolhe a carga às segundas e quartas, às 18:00, e uma compra com estimativa inicial de entrega de 2 dias, ocorre que:- Se a compra for feita numa segunda-feira antes de 18:00, a entrega ocorrerá na quarta-feira, ou seja, o prazo para entrega permanece em 2 dias.- Se a compra for feita na segunda-feira depois das 18:00, a entrega ocorrerá apenas na sexta-feira, acrescentando 2 dias na estimativa final da entrega.|
| **Modais** | **Beta** Indica que a transportadora realiza entrega de cargas diferenciadas. A coleção de modais disponíveis deve ser montada em Configurações Gerais. Os modais possíveis são Químico, Refrigerado e Líquido. Aos acostumados com os modais Leve e Pesado, essa é uma característica das tabelas de frete, que delimitam a entrega pelo peso da carga.|
| **Dimensões máximas** | **Beta** São as medidas máximas admitidas pra entrega de um pacote. É necessário informar a dimensão da maior aresta e a soma máxima de todas as arestas do pacote. Como exemplo, os correios exigem que a encomenda tenha no máximo 105cm em um dos lados, mas a soma de todos os lados não deve ultrapassar 200cm.|
| **Valores de fretes** | Tabela com os valores cobrados para entrega. |
| **Fretes com restrição** | **Beta** Tabela de faixas de CEP com restrições de entrega. É usualmente utilizado nos casos em que o serviço de entrega é prejudicado, como inundações, greves ou problemas de segurança. Necessário cadastrar uma mensagem informativa e um valor de dias a ser acrescido no tempo final da entrega. |
{: .doc-api-table }


## Tabelas de Frete
{: #TabelasDeFrete .slug-text}

Tabelas de frete são as coleções com os valores de preço e prazo de entrega de uma transportadora.
Cada valor de frete habilita uma entrega em uma faixa de códigos postais, de itens que estejam em uma determinada faixa de peso e limitados em um volume.

No VTEX Logistics, o cadastro de um valor de frete exige as seguintes informações.

| Propriedade             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Faixa de CEP's** | É necessário informar um CEP inicial e um CEP final que compõem uma faixa de CEP's atendidos pela transportadora. O valor de frete será utilizado somente se o CEP da entrega estiver contido na faixa de CEP's|
| **País** | País onde será entregue o item|
| **Faixa de peso** | é necessário informar um peso inicial e um peso final que compõem uma faixa de pesos atendidos pela transportadora. O valor de frete será utilizado somente se o peso do item calculado estiver contido na faixa de peso|
| **Preço do frete** | Valor base a ser cobrado|
| **Adicional por percentual do preço** | Ao preço do frete, será acrescido x% do preço do item calculado|
| **Adicional por peso extra** | Ao preço do frete, será acrescido x% do preço do item calculado|
| **Volume máximo** | O valor de frete será utilizado somente se o volume do item calculado não ultrapassar este valor|
| **Tempo de entrega** | Prazo dado pela transportadora pra entregar o produto, contato a partir da retirada do item no centro de distribuição|
{: .doc-api-table }

## Docas
{: #Docas .slug-text}

As docas são os operadores logisticos responsáveis por centralizar o armazenamento de produtos. No VTEX Logistics, as docas estão no centro do processo de confecção das rotas de entrega, ou seja, os produtos saem de um estoque, seguem para uma doca e em seguida para uma transportadora.

Além das informações de roteamento com as transportadoras, as docas possuem informações sobre os canais de venda atendidos, tempo de manuseio do produto e prioridade de escolha desta doca em relação a outras docas.

A entidade Doca possui as seguintes propriedades:

| Propriedade             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Id** | Identificador único da doca. Valor alfanumérico, mas não é permitido caracteres especiais. Ex: 12345, doca01, centro_de_distribuicao_01, 10cabd5, etc|
| **Nome** | Nome da doca. Ex: Doca XYZ, Doca Principal, etc|
| **Canais de venda** | Coleção de canais de venda atendidos pela doca. No mínimo 1 canal de venda deve ser associado.|
| **Transportadoras** | Lista de transportadoras que recolhem produtos na doca cadastrada. As transportadoras selecionadas compõem as rotas de entrega.|
| **Tempo** | Tempo de manuseio do produto na doca. Esse valor compoe o tempo final da estrega.|
| **Overhead de tempo** | Valor utilizado para atrasar a entrega e consequentemente desempatar rotas com mesmo tempo. Quanto maior o overhead de tempo,mais demorada é a entrega, permitindo a escolha de rotas mais rápidas. Esse valor não é acrescido no prazo final da entrega, ele serve apenas para desempate entre docas com mesmo tempo.|
| **Prioridade** | Valor utilizado para priorizar a entrega pela doca de maior prioridade. Quanto menor o valor de prioridade, mais prioritária é a doca.|
{: .doc-api-table }

## Estoques
{: #Estoques .slug-text}

De forma genérica, os estoques são os operadores responsáveis pelo armazenamento/agrupamento dos produtos. 

No VTEX Logistics, cada estoque tem rotas para uma ou mais docas, e em cada rota é possível estabelecer custo e tempo de manuseio e entrega, valores estes que serão acrescidos nos valores finais do frete calculado.

A entidade estoque possui as seguintes propriedades:

| Propriedade             | Descrição                     |
| -----------------------:| :-----------------------------|
| **Id** | Identificador único do estoque. Valor alfanumérico, mas não é permitido caracteres especiais. Ex: 12345, estoque01, 10cabd5, etc.|
| **Nome** | Nome do estoque. Ex: Estoque XYZ, Estoque Principal, Estoque Virtual, etc.|
| **Docas** | Docas às quais o estoque está associado. Para cada doca asssociada é possível informar preço e prazo de manuseio, que comporão os valores finais da entrega.|
{: .doc-api-table }

## Inventário
{: #Inventário .slug-text}

Por padrão, todo SKU cadastrado no sistema de catálogo é associado a todos os estoques cadastrados no VTEX Logistics com quantidade igual a zero.

Pelo ambiente de administração do VTEX Logistics, a atualização dos itens pode ocorrer de duas maneiras:

* De forma individual, onde é possível acrescentar ou diminuir a quantidade total de um item em cada estoque
* Atualização em massa, que permite o upload de uma planilha com os identificadores dos itens, dos estoques e a quantidade total em cada um deles.

A quantidade total de um item não necessariamente representa a quantidade disponível para venda deste. 
Quando um produto é reservado, uma unidade deste item torna-se indisponível para venda, ou seja, a quantidade disponível é exatamente a diferença entre a quantidade total e a quantidade reservada.

## Configurações Gerais
{: #ConfiguraçõesGerais .slug-text}

**Adicional nos Fretes:** valor percentual aplicado ao preço final de cada cálculo de frete. Por exemplo, se o valor final de um frete for R$ 10,00 e o adicional nos fretes for de 2%, será cobrado R$ 10,20 ao cliente

**Agrupamento de produtos:** é um valor utilizado para otimizar a montagem de rotas entre docas e estoques através do agrupamento daquelas com tempo de entrega próximos. Por exemplo, se o valor escolhido para agrupamento for 2, agrupa-se as rotas com tempos múltiplos de 2, ou seja, rotas com tempo 0 e 1 formam um grupo, rotas com tempo 2 e 3 formam outro grupo, com tempo 4 e 5 mais um grupo e daí por diante.

Imaginando um cenário com uma doca (Doca 01) e dois estoques (Estoque 01 e Estoque 02), onde o tempo total da rota Doca 01/Estoque 01 é de 4 dias e o tempo da rota Doca01/Estoque 02 é de 5 dias, no Estoque 01 há um mouse e no Estoque 02 um teclado, na compra conjunta de um mouse e um teclado ocorre que: 

* Se o valor de agrupamento é 1, a entrega é dividida em duas remessas, uma com prazo de 4 dias para entregar o teclado e outra com 5 dias para entregar o mouse. 

* Se o valor do agrupamento é 3, onde tempos 4 e 5 ficam em grupos diferentes, mouse e teclado saem em apenas uma remessa com o pior tempo de entrega, nesse caso 5 dias.

**Seleção de Modais:** Apenas os modais escolhidos aqui estarão disponíveis no cadastro das transportadoras.