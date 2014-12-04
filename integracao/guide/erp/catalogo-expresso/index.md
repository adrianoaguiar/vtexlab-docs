---
layout: docs
title: Integração Rápida
application: erp
docType: guide
---

# Integração Rápida de Catálogo e Condições Comerciais

Este documento tem por objetivo auxiliar o integrador na integração de catálogo, condição comercial(preço e estoque) do ERP para a uma loja hospedada na versão smartcheckout da VTEX, de uma maneira rápida.

Nesse tipo de integração a adminstração da loja está no admin da VTEX, sendo o ERP apenas uma fonte de onde nascem os produstos e SKUs.

## Catalogo Fluxo Básico (Express)
{: #Catalogo Fluxo Básico .slug-text}

Nesse cenário de fluxo básico, apenas os dados básicos de produtos e SKUs são manipulados pelo ERP, e todo o enriquecimento (marca, fornecedor, imagens, categoria, ativação, etc.) será feito pelo admin da loja na plataforma VTEX.

Para o ERP integrar se ao catálogo da loja na VTEX, deverá usar o webservice da própria loja, que por definição atenderá em [https:webservice-nomedaloja-vtexcommerce.com.br/service.svc?wsdl](https:webservice-nomedaloja-vtexcommerce.com.br/service.svc?wsdl "web service da loja"). As credenciais de acesso ao webservice deverão ser solicitadas junto ao administrador da loja.

Futuramente além do serviço SOAP (webservice) estaremos também oferecendo integração de catálogo por APIs REST (JSON) bem definidas e de alta performance.

## Organização dos Produtos Dentro da Loja
{: #Organização dos Produtos Dentro da Loja.slug-text}

Geralmente, os produtos são organizados dentro da loja em estruturas mercadológicas formadas por:

1. **Departamento** - categoria cujo id de categoria pai é **nulo**,
2. **Categoria** - categoria cujo id de categoria pai é um **departamento**,
3. **SubCategoria**. categoria cujo id de categoria pai é um **categoria**

### Exemplo
* Departamento/Categoria/SubCategoria/Produto
* Ferramentas/Eletricas/Furradeiras/Super Drill

O cadastro da estrutura mercadologica deve ser feito diretamente no admin da própria loja (_http://sualoja.com.br/admin/Site/Categories.aspx_), e para atender a integração vinda do ERP, é criado um departamento padrão para produtos que vem do ERP, ou seja, todos os produtos caem no admin da loja nesse departamento padrão, e depois no momento do enriquecimento é colocado na categoria desejada.

### Produtos e SKUs
{: #Produtos e SKUs.slug-text}

> Qual é a diferença entre produto e SKU?

  O **Produto** é uma definição mais genérica de algo que é ofertado ao cliente, por exemplo, *Geladeria*, *Camiseta*, *Bola*.

  O **SKU** é uma sigla em ingles de "Stock Keeping Unit", em português Unidade de Manutenção de Estoque, ou seja, uma SKU define uma variação de um produto, por exemplo, *Geladeira Branca 110V*, *Camiseta Amarela Grande*

  No modelo de cadastro de Produtos e SKUs da VTEX, um SKU sempre será filha de um Produto (não existe SKU sem produto), mesmo que esse produto não tenha variçãoes, e nesse caso será 1 SKU para 1 produto, por exemplo, produto *Bola Jabulani* com a *SKU Bola Jabulani*.

### Integração de Produtos e SKUs
{: #Integração de Produtos e SKUs.slug-text}

Após definida as variações e a estrutura mecadológica da loja, o próximo passo é enviar os produtos e as SKUs do ERP para a loja VTEX.

![alt text](ERP-catalogo-expresso.PNG "Fluxo Básico")


## Produto
{: #Produtoss.slug-text}

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **BrandId**           | **Number** <br> ID da marca. |
| **CategoryId**        | **Number** <br> ID da categoria. |
| **DepartmentId**      | **Number** <br> ID do departamento. |
| **Description**       | **String** <br> Descrição. |
| **DescriptionShort**  | **String** <br> Resumo. |
| **IsActive**  				| **Bool** <br> Ativo. |
| **IsVisible**  				| **Bool** <br> Visivel. |
| **KeyWords**  				| **String** <br> Palavras chaves. |
| **ListStoreId**  			| **Number** <br> . |
| **MetaTagDescription** | **String** <br> . |
| **Name** | **String** <br> Nome do produto. |
| **RefId** | **String** <br> refId. |
| **Title** | **String** <br> Titulo do produto. |
{: .doc-api-table }

### Exemplo

### Request

{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:arr="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:ProductInsertUpdate>
	         <tem:productVO>
	            <vtex:BrandId>2000011</vtex:BrandId> //id da marca
	            <vtex:CategoryId>1000020</vtex:CategoryId> //id da categoria
	            <vtex:DepartmentId>1000018</vtex:DepartmentId> //id do departamento
	            <vtex:Description>Vaso de barro vermelho, feito a mão com barro do mar vermelho</vtex:Description> //descrição
	            <vtex:DescriptionShort>Vaso de barro vermelho artesanal</vtex:DescriptionShort> descrição curta
	            <vtex:IsActive>true</vtex:IsActive> // true
	            <vtex:IsVisible>true</vtex:IsVisible> // vai ser visível no site
	            <vtex:KeyWords> Barro, vaso, vermelho</vtex:KeyWords> //palavras chaves
	            <vtex:LinkId>vaso_barro_vermelho</vtex:LinkId> //link do produto na loja
	            <vtex:ListStoreId> //pra qual canal de vendas = loja principal = 1
	               	<arr:int>1</arr:int>
		       		<arr:int>2</arr:int>
	            </vtex:ListStoreId>
	            <vtex:MetaTagDescription>Vaso de barro vermelho, feito a mão com barro do mar vermelho</vtex:MetaTagDescription>
	            <vtex:Name>Vaso Artesanal de Barro Vermelho</vtex:Name> //nome
	             <vtex:RefId>1234567890</vtex:RefId> //id do produto no ERP
	            <vtex:Title>Vaso Artesanal de Barro Vermelho</vtex:Title>
	         </tem:productVO>
	      </tem:ProductInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}

### Response
{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <ProductInsertUpdateResponse xmlns="http://tempuri.org/">
	         <ProductInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	            <a:AdWordsRemarketingCode i:nil="true"/>
	            <a:BrandId>2000011</a:BrandId>
	            <a:CategoryId>1000020</a:CategoryId>
	            <a:DepartmentId>1000018</a:DepartmentId>
	            <a:Description>Vaso de barro vermelho, feito a mão com barro do mar vermelho</a:Description>
	            <a:DescriptionShort>Vaso de barro vermelho artesanal</a:DescriptionShort>
	            <a:Id>31018369</a:Id>
	            <a:IsActive>false</a:IsActive>
	            <a:IsVisible>true</a:IsVisible>
	            <a:KeyWords>Barro, vaso, vermelho</a:KeyWords>
	            <a:LinkId>vaso_barro_vermelho</a:LinkId>
	            <a:ListStoreId xmlns:b="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
	               <b:int>1</b:int>
	               <b:int>2</b:int>
	            </a:ListStoreId>
	            <a:LomadeeCampaignCode i:nil="true"/>
	            <a:MetaTagDescription>Vaso de barro vermelho, feito a mão com barro do mar vermelho</a:MetaTagDescription>
	            <a:Name>Vaso Artesanal de Barro Vermelho</a:Name>
	            <a:RefId>1234567890</a:RefId>
	            <a:ReleaseDate i:nil="true"/>
	            <a:ShowWithoutStock>true</a:ShowWithoutStock>
	            <a:SupplierId i:nil="true"/>
	            <a:TaxCode i:nil="true"/>
	            <a:Title>Vaso Artesanal de Barro Vermelho</a:Title>
	         </ProductInsertUpdateResult>
	      </ProductInsertUpdateResponse>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}


### SKU
{: #Sku.slug-text}

Uma vez inseridos todos os produtos, que teoricamente são os pais das SKUs, chegou o momento de enviar as SKUs.
Exemplo dos request para inserir uma SKU na VTEX no webservice.

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **CubicWeight**         | **Number** <br> . |
| **Height**        			| **Number** <br> Altura. |
| **IsActive**  					| **Bool** <br> Ativo. |
| **IsAvaiable**  				| **Bool** <br> Disponível. |
| **IsKit**  							| **Bool** <br> Define que a SKU será um KIT - **irreversível**. |
| **Length**  						| **Number** <br> . |
| **ListPrice** 					| **Number** <br> Define o preço DE do KIT. |
| **ModalId** 						| **Number** <br> Define o preço DE do KIT. |
| **Name** 								| **String** <br> Nome do SKU do KIT. |
| **Price** 							| **Number** <br> Preço. |
| **ProductId** 					| **Number** <br> ID do produto. |
| **RealHeight** 					| **Number** <br> Altura. |
| **RealLength** 					| **Number** <br> Comprimento. |
| **RealWeightKg** 				| **Number** <br> Peso. |
| **RealWidth** 					| **Number** <br> Largura. |
| **RefId** 							| **Number** <br> refId. |
| **RewardValue** 				| **Number** <br> . |
| **UnitMultiplier** 			| **Number** <br> . |
| **WeightKg** 						| **Number** <br> . |
| **Width** 							| **Number** <br> . |
{: .doc-api-table }

### Request

{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:StockKeepingUnitInsertUpdate>
	         <tem:stockKeepingUnitVO>
	            <vtex:CubicWeight>100</vtex:CubicWeight>
	            <vtex:Height>15</vtex:Height>
	            <vtex:IsActive>true</vtex:IsActive>
	            <vtex:IsAvaiable>true</vtex:IsAvaiable>
	            <vtex:IsKit>false</vtex:IsKit>
	            <vtex:Length>15</vtex:Length>
				<vtex:ListPrice>150.0</vtex:ListPrice> **/ler obs
	            <vtex:ModalId>1</vtex:ModalId>
	            <vtex:ModalType>Vidro</vtex:ModalType>
	            <vtex:Name>Vaso Artesanal de Barro Vermelho Escuro </vtex:Name>
   				<vtex:Price>110.0</vtex:Price> **/ler obs
	            <vtex:ProductId>31018369</vtex:ProductId>
	            <vtex:RealHeight>17</vtex:RealHeight>
	            <vtex:RealLength>17</vtex:RealLength>
	            <vtex:RealWeightKg>10</vtex:RealWeightKg>
	            <vtex:RealWidth>17</vtex:RealWidth>
	            <vtex:RefId>00123456</vtex:RefId>
	            <vtex:RewardValue>0</vtex:RewardValue>
	            <vtex:StockKeepingUnitEans>
	               <vtex:StockKeepingUnitEanDTO>
	                  <vtex:Ean>0123456789123</vtex:Ean>
	               </vtex:StockKeepingUnitEanDTO>
	            </vtex:StockKeepingUnitEans>
	            <vtex:UnitMultiplier>1</vtex:UnitMultiplier>
	            <vtex:WeightKg>9</vtex:WeightKg>
	            <vtex:Width>15</vtex:Width>
	         </tem:stockKeepingUnitVO>
	      </tem:StockKeepingUnitInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}

### Response
{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <StockKeepingUnitInsertUpdateResponse xmlns="http://tempuri.org/">
	         <StockKeepingUnitInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	            <a:CommercialConditionId i:nil="true"/>
	            <a:CostPrice>1</a:CostPrice>
	            <a:CubicWeight>100</a:CubicWeight>
	            <a:DateUpdated>2014-10-29T19:03:17.718427</a:DateUpdated>
	            <a:EstimatedDateArrival i:nil="true"/>
	            <a:Height>15</a:Height>
	            <a:Id>31018371</a:Id>
	            <a:InternalNote i:nil="true"/>
	            <a:IsActive>false</a:IsActive>
	            <a:IsAvaiable>false</a:IsAvaiable>
	            <a:IsKit>false</a:IsKit>
	            <a:Length>15</a:Length>
	            <a:ListPrice>150.0</a:ListPrice>
	            <a:ManufacturerCode i:nil="true"/>
	            <a:MeasurementUnit>un</a:MeasurementUnit>
	            <a:ModalId>1</a:ModalId>
	            <a:ModalType>Vidro</a:ModalType>
	            <a:Name>Vaso Artesanal de Barro Vermelho Escuro</a:Name>
	            <a:Price>110.0</a:Price>
	            <a:ProductId>31018369</a:ProductId>
	            <a:ProductName>Vaso Artesanal de Barro Vermelho</a:ProductName>
	            <a:RealHeight>17</a:RealHeight>
	            <a:RealLength>17</a:RealLength>
	            <a:RealWeightKg>10</a:RealWeightKg>
	            <a:RealWidth>17</a:RealWidth>
	            <a:RefId>00123456</a:RefId>
	            <a:RewardValue>0</a:RewardValue>
	            <a:StockKeepingUnitEans>
	               <a:StockKeepingUnitEanDTO>
	                  <a:Ean>0123456789123</a:Ean>
	               </a:StockKeepingUnitEanDTO>
	            </a:StockKeepingUnitEans>
	            <a:UnitMultiplier>1</a:UnitMultiplier>
	            <a:WeightKg>9</a:WeightKg>
	            <a:Width>15</a:Width>
	         </StockKeepingUnitInsertUpdateResult>
	      </StockKeepingUnitInsertUpdateResponse>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}

**Obersevação:** O preço da SKU pode não ser enviado no momento da inserção da SKU. Quando um preço não é enviado no momento da criação de uma SKU, na tabela d SKU por obrigatoriedade é criado um preço fictício de 99999.00, e no sistema de "Pricing" da VTEX não é inserido o preço.

## Preço e Estoque
{: #Preço e Estoque.slug-text}

Uma vez cadastradas os produtos e as SKUs na loja da VTEX, é necessário alimentar o estoque e acertar o preço na tabela de preço (se no momento de inserir a SKU não enviou o preço).


## Preço
{: #Preço.slug-text}

Se no momento sa inserção da SKU não foi enviado um preço válido para a SKU é necessário inserir o preço da mesma. Isso pode ser feito direto no admin da loja na VTEX (_urldaloja/admin/Site/SkuTabelaValor.aspx_), ou usando a API REST do sistema de **Pricing**.

O primeiro passo a ser tomado para acessar as APIs da VTEX é solicitar os token de acesso (X-VTEX-API-AppToken e X-VTEX-API-AppKey) ao administrador da loja. Após isso fazer um POST como segue o exemplo:

**POST api/pricing/pvt/price-sheet**

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **id**         		    | **Number** <br> Caso saiba o id que vai alterar.|
| **itemId**        		| **Number** <br> Id do sku que deseja manipular.|
| **salesChannel**      | **String** <br> Canal de vendas onde vai vender.|
| **price**  						| **Number** <br> fileId.|
| **listPrice**  				| **Number** <br> Preço de.|
| **validFrom**  				| **Number** <br> Data validade de|
| **validTo**  				  | **Number** <br> Data de validade até|
{: .doc-api-table }

### Exemplo

### Request

{% highlight json %}
	[
	  	{
	    	"Id": null,
	    	"itemId": 11,
	    	"salesChannel": 1,
	    	"price": 241.0,
	    	"listPrice": 239.0,
	    	"validFrom": "2013-12-05T17:00:03.103",
	    	"validTo": "2113-12-05T17:00:03.103"
	  	}
	]
{% endhighlight %}

A documentação completa sobre a API de **Pricing** se encontra em:
http://lab.vtex.com/docs/logistics/api/latest/carrier/index.html

### Estoque
{: #Estoque.slug-text}

Isso pode ser feito direto no admin da loja na VTEX (_urldaloja/admin/logistics/#/dashboard_), maneira rápida:

1. Criar o estoque,  
2. Criar a transpotadora,  
3. Criar a doca,
4. Colocar estoque nos itens  

Criar o estoque, criar a transpotadora e criar a doca no admin da VTEX, e depois usar a API REST do
**Logistics** para manipular o estoque, como segue exemplo.


**POST /api/logistics/pvt/inventory/warehouseitems/setbalance**

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **wareHouseId**         | **Number** <br> Id do estoque.|
| **itemId**        	    | **Number** <br> id do sku que vai manipular.|
| **quantity**            | **String** <br> Quantidade do estoque que deseja atualizar.|
{: .doc-api-table }

### Exemplo

### Request
{% highlight json %}
	[
  		{
    		"wareHouseId": "1_1",
    		"itemId": "12",
    		"quantity": 100
  		}
	]
{% endhighlight %}

A documentação completa sobre a API de **Logistics** se encontra em:
[http://lab.vtex.com/docs/pricing/api/latest/pricing/index.html](http://lab.vtex.com/docs/pricing/api/latest/pricing/index.html)
