# Autenticação
Certo para fazer a autenticação nós iremos precisar do certificado gerado durante o  [passo anterior](GerandoCertificado.md), ele será responsável por tornar acessível os endpoints da nubank e se não for utilizado será retornado error de SSL.

Caso você esteja acompanhando a documentação utilizando postman, insômnia ou curl pode verificar nos seguintes linhas como adicionar o certificado pkcs12

[Adicionando ceritificado no insômnia](https://docs.insomnia.rest/insomnia/client-certificates)

[Adicionando ceritificado no postman](https://learning.postman.com/docs/sending-requests/certificates/)

[Adicionando ceritificado no curl](https://stackoverflow.com/questions/32253909/curl-with-a-pkcs12-certificate-in-a-bash-script)

A autenticação na nubank é feita a partir de token um *JWT (JSON Web Token)*, então vou deixar alguns links aqui explicando como funciona esse padrão de geração de token para a internet.

[O que é JWT <- Artigo TreinaWeb](https://www.treinaweb.com.br/blog/o-que-e-jwt)

[O que é JWT <- Vídeo Código Fonte TV](https://www.youtube.com/watch?v=Gyq-yeot8qM&t=331s)

Para você adquirir seu token você vai precisar fazer um request para o seguinte endpoint:

Padrão: Rest
Método: Post
Nome: token
Rota: https://prod-global-auth.nubank.com.br/api/token


Corpo JSON
```
{
    "grant_type": "password",
    "client_id": "legacy_client_id",
    "client_secret": "legacy_client_secret",
    "login": "",
    "password": ""
}
```

**grant_type**: campo padrão

**client_id**: campo padrão

**client_secret**: campo padrão

**login**: somente os números do cpf que você utiliza para fazer login

**password**: senha da sua conta da NuBank

Reposta: 
```
	"access_token":
	"token_type":
	"_links":
	"refresh_token": 
	"refresh_before":
```

**access_token**: com onde vem o token jwt

**token_type**: é o tipo de Authentication que a api recebe, até o momento o  tipo de autenticação é o *bearer*

**_links**: é campo que eles utilizam para exebir o HATEOAS

**refresh_token**: após fazer login a primeira vez, nós podemos utilizar esse token para permanecer logado na aplicação, ao invés relizar o login novamente.

**refresh_berfore**: não é certeza mas aparenta ser o tempo onde o token perderá a validade

*Caso não saiba o que é HATEOAS vou deixar aqui um [link](https://docs.microsoft.com/pt-br/azure/architecture/best-practices/api-design) com uma documentação da microsoft explicando sobre padrões RESTful*

Após você conseguir ter acesso ao token, basta adiciona-lo nas requisições e já vai poder a explorar, API da nubank.