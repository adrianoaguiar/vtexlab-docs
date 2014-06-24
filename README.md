VTEXLab-Docs
============

Repositório da documentação publicada no site do [vtexlab](http://lab.vtex.com/docs).

Qualquer documentação que não seja de **API** pode ser publicada através desse repositório.

Para publicar a sua **API** consulte o [Scribe](https://github.com/vtex/scribe).

###Como criar um arquivo de documentação

1. Certifique-se que a estrutura do diretório obedece o padrão:

	```
		{application}/{doc-type}[/{version}]/{resource}/index.md
	```

|nome|descrição|
|---|---------|
|{application}| nome da aplicação. Ex. portal,pricing e vtexjs|
|{doc-type}| tipo da documentação. Ex. api, lib, guide e etc|
|{version}| _opcional_. versão da API ou biblioteca|
|{resource}| recurso documentado|

**Observação: Consulte os arquivos do repositório para verificar exemplos.**

2. O front-matter (arquivo que fica no cabeçalho) para cada `{resource}` deve ser:

```
    ---
	layout: docs
	title: título do recurso
	application: {application}
	docType: {doc-type}
	version: {version}
    ---
```

###Teste
1. `git clone https://github.com/vtex/vtexlab.git` e siga o tutorial de instalação
2.  Insira sua documentação (formato [markdown](http://daringfireball.net/projects/markdown/)) no `vtexlab/docs/`
3.  `grunt`
