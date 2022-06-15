# Gerando certificado

Ela é feita para gerar um certificado de acesso remoto, esse certificado será um arquivo muito sensível, por isso, não é interessante que seja manipulado por ninguém a não ser o proprietário da conta.

Durante a documentação iremos realizar o processo utilizando o programa `openssl` disponível para Mac, Windows e Linux. 

1. Para iniciar o processo de geração de certificado você precisa primeiro criar uma chave privada privada codificada em formato PEM:

```
openssl genrsa -out <nome_arquivo> 2048
```

**nome_arquivo**: corresponde ao nome do arquivo final que irá conter a sua chave privada.

Armazene esse arquivo pos nós iremos utilizar ele mais para frente.

2. Após a criação da chave privada nós iremos agorar criar nossa chave pública:

```
openssl rsa -in <nome_arquivo_chave_privada> -out <nome_arquivo> -pubout -outform PEM
```

**nome_arquivo**: nome do arquivo onde será armazenado a chave pública.

**nome_arquivo_chave_privada**: nome do arquivo escolhido anteriormente para armazenar a chave privada.

3. Agora é necessário enviar os nossos dados para a Nubank para que ela nós envie os códigos necessários para a geração do certificado.

Esse rota pode ser encontrada no através da rota *discovery* do app

Padrão: Rest
Método: POST
Nome: gen_certificate
Rota: https://prod-global-webapp-proxy.nubank.com.br/api/proxy/AJxL5LApUVAX0b5R5DnjMw3-9ibnk8UnZg.aHR0cHM6Ly9wcm9kLWdsb2JhbC1hdXRoLm51YmFuay5jb20uYnIvYXBpL2dlbi1jZXJ0aWZpY2F0ZXM

Corpo: Json
```
{
	"login": 123123123123,
	"password": 123123123123,
	"public_key": "-----BEGIN PUBLIC KEY-----\nKEY\n-----END PUBLIC KEY-----",
	"public_key_crypto": "----BEGIN PUBLIC KEY-----\nKEY\n-----END PUBLIC KEY-----",
	"model": "Identificador do aparelho (<device_number>)",
	"device_id": "<device_number>"
}
```


**login**: CPF da sua conta NuBank

**password**: senha da sua conta NuBank

**public_key**: chave pública gerada no passo dois.

**public_key_crypto**: não há muitas informações sobre esse campo no momento, então não sabemos qual tipo de criptografia é utilizado, então podemo somente inserir a chave publica igual no campo anterior

**model**: modelo do aparelho formado por um string que identifique o aparelho mais o *device_id* que é uma string de 12 caracteres que pode conter letras minusculas e números

**device_id**: uma string de 12 caracteres que pode conter letras minusculas e números

*Quando você escreve a public_key e public_key_crypto é necessário inserir \\n no final da primeira linha e no ínicio da última*

Reposta: 

Code: 401
Body:
```
{
	"error": "Unauthorized"
}
```

4. É enviado um código de acesso para o dono da conta por e-mail e nos headers da requisição nós podemos encontrar o *header WWW-Authenticate* na chave *encrypted-code* podemos encontrar uma hash que irá ajudar a nos logar então armazene ele.

Depois desse pegar os códigos. Você deverá enviar outro request para o mesmo endpoint 
só que dessa vez com o seguinte body: 

Corpo: Json
```
{
	"login": 123123123123,
	"password": 123123123123,
	"public_key": "-----BEGIN PUBLIC KEY-----\nKEY\n-----END PUBLIC KEY-----",
	"public_key_crypto": "----BEGIN PUBLIC KEY-----\nKEY\n-----END PUBLIC KEY-----",
	"model": "Identificador do aparelho (<device_number>)",
	"device_id": "<device_number>",
	"code": "<codigo>",
	"encrypted_code": "<codigo_encryptado>"
}
```

**code**: código recebido por e-mail

**encrypted_code**: código recebido nos headers do request

Resposta: 
Body: 
```
{
"certificate": "-----BEGIN CERTIFICATE-----\nHASH\n-----END CERTIFICATE-----",
"certificate_crypto": "-----BEGIN CERTIFICATE-----\nHASH\n-----END CERTIFICATE-----"
}
```

5. Estamos quase lá agora que já possuimos o certificado basta adiciona-lo em um arquivo, remover os caracteres \\n no final e no ínicio do hash e utilizar mais uma vez o comando `openssl`

Agora nós iremos armazenar nossa chave privada e nosso ceritificado em um arquivo pkcs12 através do seguinte comando

```
openssl pkcs12 -export -in <nome_do_arquivo_com_certificado> -inkey <nome_do_arquivo_com_chave_privada> -out <nome_do_arquivo_de_saida>
```

**nome_do_arquivo_com_certificado**: arquivo com certificado que foi criado durante o passo 5.

**nome_do_arquivo_com_chave_privada**: O nome do arquivo que possui a chave privada, que foi gerada, durante o passo 1.

**nome_do_arquivo_com_certificado**: arquivo criptografado que será usado para fazer a utilização da api.

Será pedido uma senha é recomendável que você insira uma senha forte, e que você também consiga se lembrar dela.

[próximo passo](Autenticacao.md)





