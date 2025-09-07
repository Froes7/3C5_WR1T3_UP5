# Interencdec (picoCTF)
> Categoria: Criptografia
>
> Autor: NGIRIMANA Schadrack

### Introdução
O [CTF](https://en.wikipedia.org/wiki/Capture_the_flag_(cybersecurity)) a seguir é um desafio de dificuldade fácil sobre [criptografia](https://www.ibm.com/br-pt/think/topics/cryptography) presente no [picoCTF](https://play.picoctf.org). Esse desafio consiste em descriptografar uma mensagem para conseguirmos resolver a questão, aliás, apesar de não ser confirmado, o título do desafio aparenta ser uma abreviação de "Inter" **->** Interpret, "enc" **->** encod, "dec" **->** decode, logo, interpretar, codificar e descodificar.
- [Página do desafio](https://play.picoctf.org/practice/challenge/418?category=2&difficulty=1&page=1)

### Análise Inicial
O enunciado do desafio é simples, contendo apenas:
> Can you get the real meaning from this file? Download the file **here**
(Você consegue adquirir o significado real deste arquivo? Faça o download **aqui**)

Ao apertar em *"here"* o arquivo é instalado, o arquivo também é simples, assim como o enunciado, consiste apenas em um documento de texto (.txt) com o título de "enc_flag" (*flag* codificada), ao abrir o arquivo nos deparamos com uma mensagem codificada:
```sh
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgyMHdNakV5TnpVNGZRPT0nCg==
```
Além do arquivo de texto, também temos uma dica:
> Engaging in various decoding processes is of utmost importance.
(Se envolver em vários processos de decodificação é de extrema importância)

Vale ressaltar também as categorias que o desafio foi classificado, principalmente as que citam algum tipo de criptografia/codificação, visto que essas podem ser dicas valiosas para a resolução.
Por exemplo, o Interencdec foi categorizado como:
- [Criptography](https://www.ibm.com/br-pt/think/topics/cryptography) (Criptografia); 
- [Base 64](https://en.wikipedia.org/wiki/Base64); 
- [Caesar](https://pt.wikipedia.org/wiki/Cifra_de_C%C3%A9sar) (César / Cifra de César);

Essas por si só podem nos guiar ou até agilizar o nosso processo de resolução, já que nós ja sabemos que tipo de codificação e cifra foram usados na nossa *flag*.

**Print do enunciado da questão:**

[![interencdec.jpg](https://i.postimg.cc/LsPW7pFC/interencdec.jpg)](https://postimg.cc/mcbVzKDC)

### Interpretação
Conforme as dicas e pistas encontradas anteriormente, podemos deduzir que nossa *flag* está criptografada dentro do arquivo que nos foi proporcionado, mais especificamente, ela foi codificado utilizando ***Base64*** e ***Cifra de César*** (ou alguma cifra de substituição). Isso faz sentido, pois tendo mais de um tipo de criptografia, também teríamos mais de um processo de decodificação, algo que foi citado anteriormente na dica da questão.

### Resolução
Para a resolução desse desafio, iremos usar o site [**Dcode**](https://www.dcode.fr/en) e suas ferramentas de [*identificação*](https://www.dcode.fr/cipher-identifier), [*Base64*](https://www.dcode.fr/cipher-identifier) e [*Cifra de César*]. Primeiro pegamos a *flag* codificada e identificamos qual ferramenta usar, ela por si só aparenta estar codificada usando Base64, porém, para evitar erros, colocamos ela em um identificador de cifra:

[![dcodeidentifier.jpg](https://i.postimg.cc/FsHZM4Qr/dcodeidentifier.jpg)](https://postimg.cc/fk1md1Lp)

Como esperado, a ferramenta de identificação apontou para ***Base64***, logo usamos a ferramenta de decodificação da mesma:

[![dcodebase64.jpg](https://i.postimg.cc/x8j9GPFP/dcodebase64.jpg)](https://postimg.cc/BLyW4238)
```sh
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX20wMjEyNzU4fQ=='
```
Agora que decodificamos uma *"camada"* de criptografia da nossa flag podemos notar algo diferente, nossa [string] agora está com um "**b'...'**" em volta. Ao pesquisar, descobrimos que esse "b" é uma representação de um objeto [***bytes***] em [***Python***], e não faz parte da nossa *flag*. Isso pode ter se originado de um programa em *python* que o autor usou durante a criação do desafio ou talvez foi colocado intencionalmente para confundir levemente quem tentar resolver a questão.
Enfim, após remover os caracteres indesejados da *string* ela fica assim:

```sh
d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX20wMjEyNzU4fQ==
```
Colando a *flag*

[![dcodeidentifier2.jpg](https://i.postimg.cc/7LKYm7Wk/dcocdeidentifier2.jpg)](https://postimg.cc/CZfgKZ0P)

Novamente a ferramenta apontou para ***Base64***, ao decodificar:

[![dcodebase642.jpg](https://i.postimg.cc/v8gWdG9W/dcodebase642.jpg)](https://postimg.cc/rzTd1Xfm)

```sh
wpjvJAM{jhlzhy_k3jy9wa3k_m0212758}
```

Percebe-se um padrão de *flag* ao passar a *string* pelo decodificador. "wpjvJAM" remete ao padrão "picoCTF" que está presente no começo das *flags* do picoCTF, o padrão de chaves ("{...}") confirma essa nossa suspeita, isso tudo implica na utilização de alguma cifra de substituição. Visto que a [Cifra de César] já foi citada anteriormente e também é uma das cifras de substuição, usamos o Dcode e sua ferramenta de Cifra de César:

[![dcodecesar.jpg](https://i.postimg.cc/C5Xszmkn/dcodecesar.jpg)](https://postimg.cc/rRGrHGhq)

> **Flag:** picoCTF{caesar_d3cr9pt3d_f0212758}

### Conclusão
Neste desafio vimos conceitos básicos de criptografia, servindo como uma ótima prática para afinar nosso processo de resolução de questões. 
Também aprendemos que o caminho para a resposta pode não ser direto, tendo em mente conhecimento prático é um divisor na área de segurança cibernética.