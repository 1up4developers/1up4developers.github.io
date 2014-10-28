# 1up4developers Blog


## Montando Ambiente

Para baixar o Octopress (e os fontes):

`$ git clone git@github.com:1up4developers/octopress.git`

Para configurar o octopress com o repositório do Github Pages:

`$ bundle exec rake setup_github_pages`

Vai pedir o repositório, cole o endereço:

`$ git@github.com:1up4developers/1up4developers.github.io.git`

Feito isso vai aparecer a mensage:
`## Now you can deploy to git@github.com:1up4developers/1up4developers.github.io.git with `rake deploy` ##`

Para atualizar tudo com os fontes do nosso repositório, execute:

```
$ git checkout .
$ git fetch origin && git reset --hard origin/source
```

Pronto, está tudo configurado! O banch atual será "source".
Os fontes, imagens e markdown ficam neste branch.
Qualquer alteração nos fontes, será necessário dar um "git push" para subir os fontes.

## Novo post

Com tudo configurado, para criar um novo post execute:

`$ rake new_post["o titulo do seu post"]`

Isso vai criar um arquivo em "source/_posts/2013-09-17-o-titulo-do-seu-post.markdown".
Já entendeu né? Edite seu post em markdown normalmente.
Pra ajudar, pode usar um editor online: http://markup.herokuapp.com/

Se precisar subir alguma imagem, copie para "source/images" e faça o link no seu post (use caminho relativo).

Para subir um server local de preview, rode:

`$ rake preview`

e acesse a porta http://localhost:4000.

Quando terminar de editar, commit e push.

Mais informações: http://octopress.org/docs/blogging/

## Rascunhos

Por padrão, tudo que está no diretório `source/_posts` será publicado.
Então, para manter um rascunho no repositório, você pode salvar o arquivo em source/_drafts.
Quando decidir publicar, basta mover para source/_posts na mão mesmo.

## Publicar

Para publicar o site (com seu novo post), rode:
```
$ rake generate
$ rake deploy
```

Mais informações: http://octopress.org/docs/deploying/github/
1up4developers group: https://groups.google.com/forum/#!forum/1up4dev
