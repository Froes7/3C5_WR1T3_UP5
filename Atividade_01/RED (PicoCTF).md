# RED (PicoCTF)
> **Categoria:** Forense digital
>
> **Autor:** Shuailin Pan (LeConjuror)

### Introdução
Este é um desafio de dificuldade fácil sobre forense digital presente no [PicoCTF](https://play.picoctf.org/practice). A resolução consiste em analisar um arquivo de imagem em busca da *flag*, o título do desafio se dá pelo conteúdo da imagem que se trata apenas de um [PNG](https://pt.wikipedia.org/wiki/PNG) completamente vermelho, ou pelo menos, é o que parece ser.

- [Página do desafio](https://play.picoctf.org/practice/challenge/460)

### Análise Inicial
O enunciado da questão é direto e objetivo:
> "RED, RED, RED, RED 
Download the image: **red.png**"
(VERMELHO, VERMELHO, VERMELHO, VERMELHO 
baixe a imagem: **red.png**)

Baixando a imagem com o nome de red.png e abrindo-a vemos apenas a cor vermelha, como não há nenhuma informação visível, seguiremos com a análise, vamos ver as três dicas que o desafio nos fornece:

> **1** The picture seems pure, but is it though?
(A imagem parece pura, mas será que é mesmo?)

> **2** Red?Ged?Bed?Aed?

> **3** Check whatever Facebook is called now.
(Verifique aí o que o facebook se chama agora.)

Print do enunciado da questão:
[![printred.jpg](https://i.postimg.cc/v8JDBPHM/printred.jpg)](https://postimg.cc/rz9qZNrY)

Red.png:
[![red.png](https://i.postimg.cc/yYx3QCf3/red.png)](https://postimg.cc/mtxrkJWT)
### Interpretação

Agora com as informações que coletamos, podemos assumir que devemos procurar por informações em algum lugar da imagem. A terceira dica nos aponta para os [metadados](https://pt.wikipedia.org/wiki/Metadados) da imagem, visto que a empresa Facebook, em 2021, mudou o seu nome para [Meta](https://www.meta.com/pt-br/about/?srsltid=AfmBOoqTnDhT58d4_cV0nivM678rtOG_3BigDPMdLy5Pfsb17az0EhVU). Para isso, iremos usar uma ferramenta chamada de [Exiftool](https://en.wikipedia.org/wiki/ExifTool), essa ferramenta é usada tanto para leitura
quanto para modificação dos metadados de um arquivo.

```sh
exiftool red.png
```

[![redexiftool.jpg](https://i.postimg.cc/1XZkTBH8/redexiftool.jpg)](https://postimg.cc/bD3VSQ4P)

Ao analisar a saída do comando vemos um trecho em destaque, entre os dados da imagem temos uma mensagem em forma de poema:

> "Poem: Crimson heart, vibrant and bold,.Hearts flutter at your sight..Evenings glow softly red,.Cherries burst with sweet life..Kisses linger with your warmth..Love deep as merlot..Scarlet leaves falling softly,.Bold in every stroke."

Essa mensagem não precisa de tradução, visto que o conteúdo em si não nos direciona para a flag. Usando como base a segunda dica que  varia as letras iniciais em maiúsculo das palavras e as usa para formar "[RGBA](https://pt.wikipedia.org/wiki/RGBA)", podemos juntar as letras maiúsculas dessa mensagem e formar a seguinte frase:

> CHECK-LSB
(Verifique o LSB)

O termo "LSB" nessa pista se refere ao [*Least significant bit*](https://en.wikipedia.org/wiki/Bit_numbering), basicamente, o LSB é o bit de um arquivo que pode ser manipulado para esconder mensagens sem fazer uma modificação notável no conteúdo, isso é chamado de [*LSB stenography*](https://medium.com/@renantkn/lsb-steganography-hiding-a-message-in-the-pixels-of-an-image-4722a8567046), ou, estenografia de LSB. Em imagens, é possível manipulá-lo sem gerar uma mudança visível, como mudar levemente uma cor de um pixel ou de uma parte do conteúdo.
Para verificar o LSB, usaremos outra ferramenta chamada de [Zsteg](https://cybersectools.com/tools/zsteg), essa ferramenta realiza exatamente o que precisamos: ela detecta mensagens escondidas no LSB e nos imprime o resultado da varredura.

[![zstegred.jpg](https://i.postimg.cc/htNbntJd/zstegred.jpg)](https://postimg.cc/rDCtCcNV)

Logo abaixo do poema que nós já encontramos podemos identificar uma mensagem codificada:
```sh
cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjB=cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURnty
```
Para identificar a codificação dessa frase iremos usar o site [Dcode](https://www.dcode.fr/) e sua ferramenta de [identificação de cifra](https://www.dcode.fr/cipher-identifier).

[![identifierred.jpg](https://i.postimg.cc/RZb9Vj9f/identifierred.jpg)](https://postimg.cc/yD92LpDY)

O site nos apontou para a codificação [Base64](https://en.wikipedia.org/wiki/Base64), logo, usaremos o site [Cyberchef](https://gchq.github.io/CyberChef/) para decodificar essa mensagem de Base64 para texto legível.

[![cyberchefred.jpg](https://i.postimg.cc/HWZCtqcN/Cyberchefred.jpg)](https://postimg.cc/K4MVGs57)

> **Flag:** picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}

### Conclusão
Neste desafio utilizamos vários conceitos básicos de forense digital, nessa categoria de questões, é importante o poder de dedução de quem estiver resolvendo, visto que é normal pistas possuírem mais de uma interpretação ou informações estarem escondidas em plena vista.
Também é importante ressaltar o conhecimento sobre tipos de arquivos e suas propriedades, isso é crucial para a área de cybersegurança pois há diversas maneiras de esconder mensagens dentro de um arquivo que pode parecer puro à primeira vista.