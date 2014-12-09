---
layout: docs
title: Considerações
application: erp
docType: guide
---

## Considerações
{: #1 .slug-text}

A integração de ERPs com lojas VTEX é realizada através de webservice (SOAP:XML), e API REST(JSON). O webservice VTEX deve ser usado o mínimo possível para os processos de integração. Hoje com excessão do **Catálogo**, que está com sua API REST em desenvolvimento, todos os outros módulos da VTEX possúem APIs REST bem definidas e de alta performance.

É altemante recomendado que se use as APIs REST nos módulos que não seja o **Catálogo**.

## Pooling
{: #2 .slug-text}

O envio ou consumo de dados num processo de integração deve ser executado somente quando necessário, ou seja, o dado só deve ser enviado do ERP para a plataforma VTEX quando ele realmente for alterado.

É aconselhado **NAO** fazer uma integração que varre entidades inteiras do ERP e atualiza todos os dados na plataforma VTEX de tempos  em tempos. Além de consumir e processar dados desnecessáriamente, isso não funcionaria para lojas com mais de 5 mil SKUs no catálogo.


## Ferramentas de Apoio ao Integrador
{: #3 .slug-text}

Recomendamos algumas ferramentas que são de extrema importância para qualquer integrador:

**soapUI >=2.5.1**

Esta ferramenta é muito importante no processo de integração, pois ela permite simular os metodos do webservice,
gerando automaticamente o request XML.

Nesta ferramenta pode se fazer as chamadas para as APIs REST também.

## Postman
Nesta ferramente pode se testar, armazenar histórico, salvar coleções de requests do acesso de todas as APIs dos modulos VTEX  (OMS, Logistics, Pricing, GCS, etc).

É de suma importancia que o integrador tenha o conhecimento de ferramentas desse tipo, ou outras parecidas, antes de inciar um processo de integração usando webservice SOAP ou APIs REST VTEX.
