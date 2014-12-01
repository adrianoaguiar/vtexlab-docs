---
layout: docs
title: Integração de Lista
application: erp
docType: guide
---

# Integração de Lista

Este documento tem por objetivo auxiliar o integrador na integração de KIT, do ERP para uma loja hospedada na versão smartcheckout da VTEX.

## KIT
Um KIT no sistema VTEX é uma SKU como outra qualquer, só que nesse caso existem outras SKUS realcionadas a SKU do tipo KIT.

Uma vez uma SKU marcada como KIT no sistema VTEX, ela sempre será KIT, pois não como reverter um KIT.

Para cadastrar um KIT é necessário seguir os seguintes fluxos:

![alt text](erp-catalogo-completo-kit.PNG "Fluxo de KIT")

### Cadastrar o Produto KIT
{: #Cadastrar o Produto KIT .slug-text}

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

#### Request

{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" 						            xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts"
		xmlns:arr="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:ProductInsertUpdate>
	         <tem:productVO>
	            <vtex:BrandId>2000011</vtex:BrandId>
	            <vtex:CategoryId>1000020</vtex:CategoryId>
	            <vtex:DepartmentId>1000018</vtex:DepartmentId>
	            <vtex:Description>Pa e rastelo para jardinagem</vtex:Description>
	            <vtex:DescriptionShort>Pa e rastelo para jardinagem</vtex:DescriptionShort>
	            <vtex:IsActive>true</vtex:IsActive>
	            <vtex:IsVisible>true</vtex:IsVisible>
	            <vtex:KeyWords>pa rastelo</vtex:KeyWords>
	            <vtex:ListStoreId>
	               <arr:int>1</arr:int>
	            </vtex:ListStoreId>
	            <vtex:MetaTagDescription>Pa e rastelo para jardinagem</vtex:MetaTagDescription>
	            <vtex:Name>Kit pa e rastelo</vtex:Name>
	            <vtex:RefId>2234567890</vtex:RefId>
	            <vtex:Title>Kit pa e rastelo</vtex:Title>
	         </tem:productVO>
	      </tem:ProductInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}

#### Response

{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <ProductInsertUpdateResponse xmlns="http://tempuri.org/">
	         <ProductInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	            <a:AdWordsRemarketingCode i:nil="true"/>
	            <a:BrandId>2000011</a:BrandId>
	            <a:CategoryId>1000020</a:CategoryId>
	            <a:DepartmentId>1000018</a:DepartmentId>
	            <a:Description>Pa e rastelo para jardinagem</a:Description>
	            <a:DescriptionShort>Pa e rastelo para jardinagem</a:DescriptionShort>
	            <a:Id>31018370</a:Id>
	            <a:IsActive>false</a:IsActive>
	            <a:IsVisible>true</a:IsVisible>
	            <a:KeyWords>pa rastelo</a:KeyWords>
	            <a:LinkId>Kit-pa-e-rastelo</a:LinkId>
	            <a:ListStoreId xmlns:b="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
	               <b:int>1</b:int>
	            </a:ListStoreId>
	            <a:LomadeeCampaignCode i:nil="true"/>
	            <a:MetaTagDescription>Pa e rastelo para jardinagem</a:MetaTagDescription>
	            <a:Name>Kit pa e rastelo</a:Name>
	            <a:RefId>2234567890</a:RefId>
	            <a:ReleaseDate i:nil="true"/>
	            <a:ShowWithoutStock>true</a:ShowWithoutStock>
	            <a:SupplierId i:nil="true"/>
	            <a:TaxCode i:nil="true"/>
	            <a:Title>Kit pa e rastelo</a:Title>
	         </ProductInsertUpdateResult>
	      </ProductInsertUpdateResponse>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}

### Cadastrar a SKU KIT
{: #Cadastrar a SKU KIT .slug-text}

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

### Exemplo

#### Request

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
	            <vtex:IsKit>true</vtex:IsKit> /
	            <vtex:Length>15</vtex:Length>
		        	<vtex:ListPrice>50.0</vtex:ListPrice>
	            <vtex:ModalId>1</vtex:ModalId>
	            <vtex:Name>SKU do KIT</vtex:Name>
		        	<vtex:Price>40.0</vtex:Price> //define o preço POR do KIT
	            <vtex:ProductId>31018370</vtex:ProductId>
	            <vtex:RealHeight>17</vtex:RealHeight>
	            <vtex:RealLength>17</vtex:RealLength>
	            <vtex:RealWeightKg>10</vtex:RealWeightKg>
	            <vtex:RealWidth>17</vtex:RealWidth>
	            <vtex:RefId>30123456</vtex:RefId>
	            <vtex:RewardValue>0</vtex:RewardValue>
	            <vtex:StockKeepingUnitEans>
	               <vtex:StockKeepingUnitEanDTO>
	                  <vtex:Ean>3123456789123</vtex:Ean>
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


#### Response
{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <StockKeepingUnitInsertUpdateResponse xmlns="http://tempuri.org/">
	         <StockKeepingUnitInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	            <a:CommercialConditionId>1</a:CommercialConditionId>
	            <a:CostPrice>1.0000</a:CostPrice>
	            <a:CubicWeight>100</a:CubicWeight>
	            <a:DateUpdated>2014-11-03T13:58:10.3061928</a:DateUpdated>
	            <a:EstimatedDateArrival i:nil="true"/>
	            <a:Height>15</a:Height>
	            <a:Id>31018373</a:Id>
	            <a:InternalNote i:nil="true"/>
	            <a:IsActive>false</a:IsActive>
	            <a:IsAvaiable>false</a:IsAvaiable>
	            <a:IsKit>true</a:IsKit>
	            <a:Length>15</a:Length>
	            <a:ListPrice>50.0</a:ListPrice>
	            <a:ManufacturerCode i:nil="true"/>
	            <a:MeasurementUnit>un</a:MeasurementUnit>
	            <a:ModalId>1</a:ModalId>
	            <a:ModalType i:nil="true"/>
	            <a:Name>SKU do KIT</a:Name>
	            <a:Price>40.0</a:Price>
	            <a:ProductId>31018370</a:ProductId>
	            <a:ProductName>Kit pa e rastelo</a:ProductName>
	            <a:RealHeight>17</a:RealHeight>
	            <a:RealLength>17</a:RealLength>
	            <a:RealWeightKg>10</a:RealWeightKg>
	            <a:RealWidth>17</a:RealWidth>
	            <a:RefId>30123456</a:RefId>
	            <a:RewardValue>0</a:RewardValue>
	            <a:StockKeepingUnitEans>
	               <a:StockKeepingUnitEanDTO>
	                  <a:Ean>3123456789123</a:Ean>
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

### Cadastrar a Imagem do SKU KIT
{: #Cadastrar a Imagem do SKU KIT .slug-text}

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **urlImage**         		| **String** <br> Url da imagem.|
| **imageName**        		| **String** <br> Nome da imagem.|
| **stockKeepingUnitId**  | **Number** <br> Id da SKU.|
| **fileId**  						| **Number** <br> fileId.|
{: .doc-api-table }

### Exemplo

#### Request
{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:>
	         <tem:urlImage>http://img.ph2-jpg.posthaus.com.br/Web/posthaus/foto/brinquedos/jogos-e-outros-brinquedos/brinquedo-pa-e-rastelo_56855_600_1.jpg</tem:urlImage>
	         <tem:imageName>Pa_Rastelo</tem:imageName>
	         <tem:stockKeepingUnitId>31018373</tem:stockKeepingUnitId>
	         <tem:fileId>31018373</tem:fileId>//id da sku
	      </tem:ImageServiceInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}

#### Response
{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <ImageServiceInsertUpdateResponse xmlns="http://tempuri.org/"/>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}

### Cadastrar os produtos (pai da SKUS) do KIT
{: #Cadastrar os produtos do KIT .slug-text}

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
| **LinkId**  			| **Number** <br> . |
| **ListStoreId** | **String** <br> . |
| **MetaTagDescription** | **String** <br> Nome do produto. |
| **Name** | **String** <br> . |
| **RefId** | **String** <br> refId. |
| **Title** | **String** <br> Titulo do produto. |
{: .doc-api-table }

### Exemplo

#### Request
{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts"
	xmlns:arr="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:ProductInsertUpdate>
	         <tem:productVO>
	            <vtex:BrandId>2000011</vtex:BrandId>
	            <vtex:CategoryId>1000020</vtex:CategoryId>
	            <vtex:DepartmentId>1000018</vtex:DepartmentId>
	            <vtex:Description>Pa</vtex:Description>
	            <vtex:DescriptionShort>Pa de jardim</vtex:DescriptionShort>
	            <vtex:IsActive>true</vtex:IsActive>
	            <vtex:IsVisible>false</vtex:IsVisible>
	            <vtex:KeyWords>Pa de jardim</vtex:KeyWords>
	            <vtex:LinkId>pa_jardim</vtex:LinkId>
	            <vtex:ListStoreId>
	               <arr:int>1</arr:int>
	            </vtex:ListStoreId>
	            <vtex:MetaTagDescription>Pa de jardim</vtex:MetaTagDescription>
	            <vtex:Name>Pa de jardim</vtex:Name>
	            <vtex:RefId>5234567891</vtex:RefId>
	            <vtex:Title>Pa de jardim</vtex:Title>
	         </tem:productVO>
	      </tem:ProductInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}

#### Response

{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <ProductInsertUpdateResponse xmlns="http://tempuri.org/">
	         <ProductInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	            <a:AdWordsRemarketingCode i:nil="true"/>
	            <a:BrandId>2000011</a:BrandId>
	            <a:CategoryId>1000020</a:CategoryId>
	            <a:DepartmentId>1000018</a:DepartmentId>
	            <a:Description>Pa</a:Description>
	            <a:DescriptionShort>Pa de jardim</a:DescriptionShort>
	            <a:Id>31018371</a:Id>
	            <a:IsActive>false</a:IsActive>
	            <a:IsVisible>false</a:IsVisible>
	            <a:KeyWords>Pa de jardim</a:KeyWords>
	            <a:LinkId>pa_jardim</a:LinkId>
	            <a:ListStoreId xmlns:b="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
	               <b:int>1</b:int>
	            </a:ListStoreId>
	            <a:LomadeeCampaignCode i:nil="true"/>
	            <a:MetaTagDescription>Pa de jardim</a:MetaTagDescription>
	            <a:Name>Pa de jardim</a:Name>
	            <a:RefId>5234567891</a:RefId>
	            <a:ReleaseDate i:nil="true"/>
	            <a:ShowWithoutStock>true</a:ShowWithoutStock>
	            <a:SupplierId i:nil="true"/>
	            <a:TaxCode i:nil="true"/>
	            <a:Title>Pa de jardim</a:Title>
	         </ProductInsertUpdateResult>
	      </ProductInsertUpdateResponse>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}

### Cadastrar as SKUs do KIT
{: #Cadastrar as SKUs do KIT .slug-text}

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

### Exemplo

#### Request

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
	            <vtex:ModalId>1</vtex:ModalId>
	            <vtex:Name>Pa para jardim</vtex:Name>
	            <vtex:ProductId>31018371</vtex:ProductId>
	            <vtex:RealHeight>17</vtex:RealHeight>
	            <vtex:RealLength>17</vtex:RealLength>
	            <vtex:RealWeightKg>10</vtex:RealWeightKg>
	            <vtex:RealWidth>17</vtex:RealWidth>
	            <vtex:RefId>40123457</vtex:RefId>
	            <vtex:RewardValue>0</vtex:RewardValue>
	            <vtex:StockKeepingUnitEans>
	               <vtex:StockKeepingUnitEanDTO>
	                  <vtex:Ean>1223456789123</vtex:Ean>
	               </vtex:StockKeepingUnitEanDTO>
	            </vtex:StockKeepingUnitEans>
	            <vtex:UnitMultiplier>1</vtex:UnitMultiplier>
	            <vtex:WeightKg>1</vtex:WeightKg>
	            <vtex:Width>15</vtex:Width>
	         </tem:stockKeepingUnitVO>
	      </tem:StockKeepingUnitInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}


#### Response

{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <StockKeepingUnitInsertUpdateResponse xmlns="http://tempuri.org/">
	         <StockKeepingUnitInsertUpdateResult
					xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts"
					 xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	            <a:CommercialConditionId i:nil="true"/>
	            <a:CostPrice>1</a:CostPrice>
	            <a:CubicWeight>100</a:CubicWeight>
	            <a:DateUpdated>2014-11-03T14:16:52.5863835</a:DateUpdated>
	            <a:EstimatedDateArrival i:nil="true"/>
	            <a:Height>15</a:Height>
	            <a:Id>31018374</a:Id>
	            <a:InternalNote i:nil="true"/>
	            <a:IsActive>false</a:IsActive>
	            <a:IsAvaiable>false</a:IsAvaiable>
	            <a:IsKit>false</a:IsKit>
	            <a:Length>15</a:Length>
	            <a:ListPrice>99999</a:ListPrice> //qdo naum mnada preço retorn simbólico
	            <a:ManufacturerCode i:nil="true"/>
	            <a:MeasurementUnit>un</a:MeasurementUnit>
	            <a:ModalId>1</a:ModalId>
	            <a:ModalType i:nil="true"/>
	            <a:Name>Pa para jardim</a:Name>
	            <a:Price>99999</a:Price> //qdo naum mnada preço retorn simbólico
	            <a:ProductId>31018371</a:ProductId>
	            <a:ProductName>Pa de jardim</a:ProductName>
	            <a:RealHeight>17</a:RealHeight>
	            <a:RealLength>17</a:RealLength>
	            <a:RealWeightKg>10</a:RealWeightKg>
	            <a:RealWidth>17</a:RealWidth>
	            <a:RefId>40123457</a:RefId>
	            <a:RewardValue>0</a:RewardValue>
	            <a:StockKeepingUnitEans>
	               <a:StockKeepingUnitEanDTO>
	                  <a:Ean>1223456789123</a:Ean>
	               </a:StockKeepingUnitEanDTO>
	            </a:StockKeepingUnitEans>
	            <a:UnitMultiplier>1</a:UnitMultiplier>
	            <a:WeightKg>1</a:WeightKg>
	            <a:Width>15</a:Width>
	         </StockKeepingUnitInsertUpdateResult>
	      </StockKeepingUnitInsertUpdateResponse>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}

### Cadastrar as Imagens das SKUs do KIT
{: #Cadastrar as Imagens das SKUs do KIT .slug-text}

### Parâmetros

| Nome                    | Tipo                          |
| -----------------------:| :-----------------------------|
| **urlImage**         		| **String** <br> Url da imagem.|
| **imageName**        		| **String** <br> Nome da imagem.|
| **stockKeepingUnitId**  | **Number** <br> Id da SKU.|
| **fileId**  						| **Number** <br> fileId.|
{: .doc-api-table }

#### Request

{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:ImageServiceInsertUpdate>	   <tem:urlImage>http://flicbrinquedos.com.br/media/catalog/product/cache/1/image/9df78eab33525d08d6e5fb8d27136e95/A/c/Acess_rio_Para_Jardim_-_P_-_Melissa_Doug_7_13.jpg</tem:urlImage>
	         <tem:imageName>Pa</tem:imageName>
	         <tem:stockKeepingUnitId>31018374</tem:stockKeepingUnitId>
	          <tem:fileId>31018374</tem:fileId>
	      </tem:ImageServiceInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}


#### Response

{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <ImageServiceInsertUpdateResponse xmlns="http://tempuri.org/"/>
	   </s:Body>
	</s:Envelope
{% endhighlight %}


### Associar SKUs ao KIT
{: #Associar SKUs ao KIT.slug-text}

### Request

{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
	 xmlns:tem="http://tempuri.org/"
	 xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:StockKeepingUnitKitInsertUpdate>
	         <tem:stockKeepingUnitKit>
	            <vtex:Amount>1</vtex:Amount> //quantidade de itens no KIT
	            <vtex:StockKeepingUnitId>31018374</vtex:StockKeepingUnitId>
	            <vtex:StockKeepingUnitParent>31018373</vtex:StockKeepingUnitParent> //id da SKU KIT
	            <vtex:UnitPrice>20.00</vtex:UnitPrice> //preço da unidade dentro do KIT
	         </tem:stockKeepingUnitKit>
	      </tem:StockKeepingUnitKitInsertUpdate>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}

### Response

{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <StockKeepingUnitKitInsertUpdateResponse xmlns="http://tempuri.org/">
	         <StockKeepingUnitKitInsertUpdateResult
							xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts"
							xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	            <a:Amount>1</a:Amount>
	            <a:Id>35</a:Id>
	            <a:StockKeepingUnitId>31018374</a:StockKeepingUnitId>
	            <a:StockKeepingUnitParent>31018373</a:StockKeepingUnitParent>
	            <a:UnitPrice>20.00</a:UnitPrice>
	         </StockKeepingUnitKitInsertUpdateResult>
	         <stockKeepingUnitKitId>35</stockKeepingUnitKitId>
	      </StockKeepingUnitKitInsertUpdateResponse>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}

### Ativar SKUs do KIT

### Request

{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:StockKeepingUnitActive>
	         <tem:idStockKeepingUnit>31018374</tem:idStockKeepingUnit>
	      </tem:StockKeepingUnitActive>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}


### Response

{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <StockKeepingUnitActiveResponse xmlns="http://tempuri.org/"/>
	   </s:Body>
	</s:Envelope
{% endhighlight %}


### Ativar KIT

### Request

{% highlight json %}
	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <tem:StockKeepingUnitActive>
	         <tem:idStockKeepingUnit>31018373</tem:idStockKeepingUnit>
	      </tem:StockKeepingUnitActive>
	   </soapenv:Body>
	</soapenv:Envelope>
{% endhighlight %}

### Response

{% highlight json %}
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	   <s:Body>
	      <StockKeepingUnitActiveResponse xmlns="http://tempuri.org/"/>
	   </s:Body>
	</s:Envelope>
{% endhighlight %}


### Preço KIT e SKUS do KIT
{: #Preço KIT e SKUS do KIT .slug-text}

O preço do KIT pode ser formado de 2 maneiras:

1. caso o SKU KIT contenha preço na tabela de preço, o preço considerado na loja será o preço definido

2. caso o SKU KIT não possua preço, o preço do KIT será feito pelo intens que o compõem.

<div class="api-description">
POST  /api/pricing/pvt/price-sheet
{: .api-route }
</div>

{% highlight json %}

		[
			{
				"Id": null,
				"itemId": 31018373, //id do KIT
				"salesChannel": 1,
				"price": 60.0,
				"listPrice": 50.0,
				"validFrom": "2012-12-05T17:00:03.103",
				"validTo": "2016-12-05T17:00:03.103"
			}
		]
{% endhighlight %}

### Estoque SKUs KIT
{: #Estoque SKUs KIT.slug-text}

O kit só estará disponível para a venda quando todos os seus itens estiverem disponíveis.

	POST /api/logistics/pvt/inventory/warehouseitems/setbalance

### Request

{% highlight json %}
		[
			{
				"wareHouseId": "1_1",
				"itemId": "31018374",
				"quantity": 100
			}
		]
{% endhighlight %}

### Response
{% highlight json %}
{
	true
}
{% endhighlight %}
