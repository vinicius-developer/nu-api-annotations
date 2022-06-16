# Discovery

Para descobrir quais são as rotas que a nubank disponibiliza inicialmente, nós podemos utilzar dois endpoints um deles irá mostrar as rotas utilizadas pelo APP e o outro mostra os endpoints utilizados pelo site web.

[Rota do app](https://prod-s0-webapp-proxy.nubank.com.br/api/app/discovery)

[Rota do site web](https://prod-s0-webapp-proxy.nubank.com.br/api/discovery)

Para acessar algumas das várias rotas, vorê irá sofrer com erros de SSL, e para resolve-los irá precisar gerar um certificado através da rota *gen_certificate*. O processo para a geração de certificados está melhor descrito no seguinte documento: [gerando certificados](GerandoCertificado)