# Cookie Monster Secret Recipe (PicoCTF)
> Categoria: [Web Exploitation](https://devopedia.org/web-exploitation)
> Autor: Brhane Giday e Prince Niyonshuti N.

## Desafio: Cookie Monster Secret Recipe
#### Introdução
Este [CTF](https://en.wikipedia.org/wiki/Capture_the_flag_(cybersecurity)) é um dos quatro desafios fáceis de [web exploitation](https://devopedia.org/web-exploitation) presentes no [picoCTF](https://play.picoctf.org). O desafio consiste em analisar e inspecionar um website em busca de uma informação escondida dentro dos [cookies](https://en.wikipedia.org/wiki/HTTP_cookie) do site, como está implícito no título do desafio.
- [Página do desafio](https://play.picoctf.org/practice/challenge/469?category=1&difficulty=1&originalEvent=74&page=1)
#### Análise Inicial

O enunciado do desafio é o seguinte:
> Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. As an aspiring cookie detective, your mission is to uncover this delectable secret. Can you outsmart Cookie Monster and find the hidden recipe?
(Come-Come escondeu sua receita de alto sigilo em algum lugar no seu website. Como um aspirante detetive de cookies, sua missão é desvendar este segredo delicioso. Você consegue ser mais esperto que o Come-Come e encontrar a receita escondida?)

Logo após é dito que precisamos lançar uma instância para conseguir mais informações e acessar o website onde conseguiremos a flag.
>Additional details will be available after launching your challenge instance.
(Detalhes adicionais estarão disponíveis após lançar sua instância de desafio)

Para lançar a instância apertamos no botão **"Launch Instance"** e aguardamos alguns segundos. Com a instância lançada temos o seguinte texto:
>You can access the Cookie Monster here and good luck
(Você pode acessar o Come-Come aqui e boa sorte)

Clicando na palavra **"here"** somos direcionados ao site do Come-Come, porém, antes de irmos direto ao site, daremos uma olhada nas três dicas que o desafio nos proporciona:

> **1** Sometimes, the most important information is hidden in plain sight. Have you checked all parts of the webpage?
(**1** Às vezes, a informação mais importante está escondida em plena vista. Você checou todas as partes da "webpage"?)

> **2** Cookies aren't just for eating - they're also used in web technologies!
(**2** Cookies não são apenas para comer - eles também são usados em tecnologias *web*!)

> **3** Web browsers often have tools that can help you inspect various aspects of a webpage, including things you can't see directly.
(**3** Navegadores muitas vezes tem ferramentas que podem ajudar você a inspecionar vários aspectos de uma página, incluindo coisas que você não consegue ver diretamente.)

Print do enunciado da questão:
[![enunciado.jpg](https://i.postimg.cc/bJDHhttT/enunciado.jpg)](https://postimg.cc/2bYBd3Tb)

#### Interpretação
As dicas e o contexto do desafio nos induzem a suspeitar que o que estamos atrás está escondido dentro das informações do site, mais especificamente, nos [cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) presentes na página, nos quais pode haver alguma informação importante, tendo em vista a dica de número dois e o nome do famoso personagem da série Vila Sésamo "Cookie Monster", ou [Come-Come](https://en-m-wikipedia-org.translate.goog/wiki/Cookie_Monster?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc) em português.

#### Resolução
Retornando ao website, nos deparamos com uma página de login e o título do site:

[![Página de login.jpg](https://i.postimg.cc/yxHVXTQD/P-gina-de-login.jpg)](https://postimg.cc/47WkXpTG)
> (Receita secreta do Come-Come)

Considerando o funcionamento dos cookies, primeiro temos que gerá-los ao interagir com a página, para isso, fazemos uma tentativa de login.
Ao tentar logar nos deparamos com essa página:
[![access denied.jpg](https://i.postimg.cc/B6NhMSN2/Access-denied.jpg)](https://postimg.cc/R66Q0zk0)
> (Acesso negado
Come-Come diz: 'Eu não precisar de nenhuma senha. Eu só precisar de biscoitos!'
Dica: Você verificou seus cookies recentemente?)
(Nota: a gramática errada imita a forma como o personagem Come-Come fala.)

Mais uma vez o desafio nos direciona a checar os cookies do site. 
Agora que nossos cookies foram gerados, pressionamos a tecla F12 para abrir a ferramenta de inspeção de página, iremos à aba de "Armazenamento" e depois para "Cookies" (ou, Application -> Cookies se estiver usando Chrome), nessa aba, encontramos apenas um cookie com o nome de "secret_recipe" (receita_secreta) e um valor codificado.

[![Cookies.jpg](https://i.postimg.cc/B615k2T5/Cookies.jpg)](https://postimg.cc/zb8RRbP3)
| Nome | Valor |
| ------ | ------ |
| secret_recipe| cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzQ3MzZGNkNCfQ%3D%3D|

Geralmente os valores de um site são codificados em [Base64](https://en.wikipedia.org/wiki/Base64) para armazenar informações de forma eficiente, pois os headers [HTTP](https://en-m-wikipedia-org.translate.goog/wiki/HTTP?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc) só aceitam caracteres [ASCII](https://en.wikipedia.org/wiki/ASCII) simples, para adquirir a [string](https://en.wikipedia.org/wiki/String_(computer_science)) mais facilmente vamos até a aba console e rodamos seguinte comando:

[![console.jpg](https://i.postimg.cc/P5PCSTzY/console.jpg)](https://postimg.cc/LJF4XdW8)
```sh
document.cookie
```
Assim podemos apenas copiar (CTRL+C) e colar (CTRL+V) em qualquer lugar que desejarmos.

Também podemos usar a ferramenta de identificação do site [Dcode](https://www.dcode.fr/cipher-identifier) para descobrirmos o tipo de codificação:
[![identifier.jpg](https://i.postimg.cc/9MT8T6nj/identifier.jpg)](https://postimg.cc/Q98JZyD6)

Agora que temos o valor do nosso cookie e seu tipo de codificação basta decodificar, para isso vamos usar a ferramenta de decodificação de Base64, novamente, do site [Dcode](https://www.dcode.fr/base-64-encoding). Assim, finalmente adquirindo a nossa flag.

[![base64.jpg](https://i.postimg.cc/kMvwmvrw/base64.jpg)](https://postimg.cc/5jj85v2F)
> `**Flag:** picoCTF{c00k1e_m0nster_l0ves_c00kies_4736F6CB}`

#### Conclusão
Este desafio abordou conceitos básicos de web exploitation, ele nos ensina a usar as ferramentas que estão ao nosso dispor nos navegadores para achar informações importantes de maneira leve e cômica. Essas ferramentas são fundamentais para a carreira de cybersegurança, onde é preciso usar todos os recursos disponíveis de maneira eficiente. Também é importante notar que testamos hipóteses simples, sem complicar demais a resolução — uma característica importante e que pode passar despercebida.
Durante a resolução do desafio somos treinados a entender que às vezes informações podem estar escondidas em lugares além do que conseguimos ver, melhorando nosso processo investigativo e curiosidade.