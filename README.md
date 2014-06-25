VTEXLab-Docs
============

Repositório da documentação publicada no site do [vtexlab](http://lab.vtex.com/docs).

Qualquer documentação que não seja de **API** pode ser publicada através desse repositório.

Para publicar a sua **API** consulte o [Scribe](https://github.com/vtex/scribe).

### Como criar um arquivo de documentação

1. Crie uma **branch** com o prefixo `doc/`

2. Certifique-se que a estrutura do diretório obedece o padrão:

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

3. O front-matter (arquivo que fica no cabeçalho) para cada `{resource}` deve ser:

```
    ---
	layout: docs
	title: título do recurso
	application: {application}
	docType: {doc-type}
	version: {version}
    ---

```

### Como publico um arquivo de documentação

#### Revisão — Recebendo feedback

Quando terminar de escrever é só fazer [pull request](https://help.github.com/articles/creating-a-pull-request) da branch que você criou para receber comentários via Github.

Geralmente enviamos o link do PR via email ou Hipchat pedindo aos colegas que ajudem a revisar juntamente com um pequeno prazo.

### Publicando

Depois de aceitar as revisões, editar seu post com base nos feedbacks e fazer commit das alterações, é só apertar o botão de merge na própria página do **pull request**.
Ele vai jogar seu arquivo *.md em `master` e a mágina do deploy (que às vezes demora uns 5 minutos) vai resolver o resto.


### Teste
1. `git clone https://github.com/vtex/vtexlab.git` e siga o tutorial de instalação
2.  Insira sua documentação (formato [markdown](http://daringfireball.net/projects/markdown/)) no `vtexlab/docs/`
3.  `grunt`
