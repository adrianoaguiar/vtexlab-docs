---
layout: docs
title: Considerações
application: erp
docType: guide
---

## Considerações
{: #Considerações .slug-text}

O webservice VTEX deve ser usado o mínimo possível para os processo de integração, hoje com excessão do **Catálogo**, que está com sua API REST em desenvolvimento, todos os outros módulos da VTEX possúem APIs REST bem definidas e de alta performance.

É altemante recomendado que se use as APIs REST nos módulos que não seja o **Catálogo**.

## Pooling
{: #Pooling REST .slug-text}

O envio ou consumo de dados num processo de integração deve ser executado somente quando necessário, ou seja, o dado só deve ser enviado do ERP para a plataforma VTEX quando ele realmente for alterado.

É aconselhados, **NAO** fazer uma integração que varre entidades inteiras do ERP e atualiza todos os dados na plataforma VTEX de tempos  em tempos.]

Além de consumir e processar dados desnecessáriamente, isso não funcionaria para lojas com mais de 5 mil Skus no catálogo.


## Ferramentas de apoio ao integrador
{: #Ferramentas REST .slug-text}

Recomendamos algumas ferramentas que são de extrema importância para qualquer integrador:

**soapUI >=2.5.1**

Esta ferramenta é muito importante no processo de integração, pois ela permite simular os metodos do webservice,
gerando automaticamente o request XML.

Nesta ferramenta pode fazer as chamdas para as APIs REST também.

## Postman
Nesta ferramente pode se testar, armazenar histórico, salvar coleções de requests do acesso de todas as APIs dos modulos VTEX  (OMS, Logistics, Pricing, GCS, etc).

É de suma importancia que o integrador tenha o conhecimento de ferramentas desse tipo, ou outras parecidas, antes de inciar um processo de integração usando webservice SOAP ou APIs REST VTEX.
