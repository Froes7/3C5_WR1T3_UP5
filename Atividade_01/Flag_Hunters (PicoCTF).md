# Flag Hunters (picoCTF)
> **Categoria:** Engenharia reversa
>
> **Autor:** Syreal

### Introdução
Este é um desafio de dificuldade fácil presente no [picoCTF](https://play.picoctf.org/practice?originalEvent=74&page=1), ele é o primeiro desafio de [engenharia reversa](https://en.wikipedia.org/wiki/Reverse_engineering) do evento de 2025. A resolução dessa questão consiste em analisar um código com um sistema de letra de música e aproveitar de uma falha existente nele, assim, adquirimos nossa flag.

- [Página do Desafio](https://play.picoctf.org/practice/challenge/472?originalEvent=74&page=1)

### Análise inicial
O enunciado é o seguinte:
> "Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? There might be something in it for you. The program's source code can be downloaded **here.**"
(Letras de música pulam de versos para o refrão como uma chamada de sub-rotina. Existe um refrão escondido que esse programa não imprime por padrão. Você consegue fazer ele imprimir? Pode ser que tenha algo nele para você. O código fonte pode ser baixado **aqui**)

Ao clicar em "*here*" iniciamos o download de um programa em [python](https://aws.amazon.com/pt/what-is/python/) com o título de "*lyric-reader*" (leitor de letras de música).
Logo após o enunciado é dito que precisamos lançar uma instância para conseguirmos detalhes adicionais.

> Additional details will be available after launching your challenge instance. 
(Detalhes adicionais estarão disponíveis após lançar sua instância de desafio)

Para lançar a instância apertamos no botão "Launch Instance" e aguardamos alguns segundos. Com a instância lançada temos o seguinte texto:

> "Connect to the program with netcat: $ nc verbal-sleep.picoctf.net 56290"
(Conecte-se ao programa usando *netcat*: $ nc verbal-sleep.picoctf.net 56290)

```sh
nc verbal-sleep.picoctf.net 56290
```
[Netcat](https://en.wikipedia.org/wiki/Netcat) (nc) é uma ferramenta de linha de comando que lê e escreve dados de uma conexão de rede, é uma ferramenta extremamente útil e amplamente usada para testar e depurar conexões, fazer varreduras, tranferir arquivos e diversas outras funcionalidades.
Antes de aplicarmos o comando fornecido, daremos uma olhada nas três dicas do desafio:

> **1** This program can easily get into undefined states. Don't be shy about Ctrl-C.
(Este programa pode entrar facilmente em um estado indefinido. Não seja tímido sobre Ctrl-C.)

> **2** Unsanitized user input is always good, right?
(Entrada de úsuario não validada é sempre boa, certo?)

> **3** Is there any syntax that is ripe for subversion?
(Existe alguma sintaxe que esteja pronta para subversão?)

### Interpretação

O enunciado e as dicas que foram fornecidas nos induzem a fazer uma entrada no programa aproveitando de um erro de sintaxe para nos exibir a *flag* oculta.

### Pontos Chave

O código do programa é o seguinte:
```sh
import re
import time


# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'


song_flag_hunters = secret_intro +\
'''

[REFRAIN]
We’re flag hunters in the ether, lighting up the grid,
No puzzle too dark, no challenge too hid.
With every exploit we trigger, every byte we decrypt,
We’re chasing that victory, and we’ll never quit.
CROWD (Singalong here!);
RETURN

[VERSE1]
Command line wizards, we’re starting it right,
Spawning shells in the terminal, hacking all night.
Scripts and searches, grep through the void,
Every keystroke, we're a cypher's envoy.
Brute force the lock or craft that regex,
Flag on the horizon, what challenge is next?

REFRAIN;

Echoes in memory, packets in trace,
Digging through the remnants to uncover with haste.
Hex and headers, carving out clues,
Resurrect the hidden, it's forensics we choose.
Disk dumps and packet dumps, follow the trail,
Buried deep in the noise, but we will prevail.

REFRAIN;

Binary sorcerers, let’s tear it apart,
Disassemble the code to reveal the dark heart.
From opcode to logic, tracing each line,
Emulate and break it, this key will be mine.
Debugging the maze, and I see through the deceit,
Patch it up right, and watch the lock release.

REFRAIN;

Ciphertext tumbling, breaking the spin,
Feistel or AES, we’re destined to win.
Frequency, padding, primes on the run,
Vigenère, RSA, cracking them for fun.
Shift the letters, matrices fall,
Decrypt that flag and hear the ether call.

REFRAIN;

SQL injection, XSS flow,
Map the backend out, let the database show.
Inspecting each cookie, fiddler in the fight,
Capturing requests, push the payload just right.
HTML's secrets, backdoors unlocked,
In the world wide labyrinth, we’re never lost.

REFRAIN;

Stack's overflowing, breaking the chain,
ROP gadget wizardry, ride it to fame.
Heap spray in silence, memory's plight,
Race the condition, crash it just right.
Shellcode ready, smashing the frame,
Control the instruction, flags call my name.

REFRAIN;

END;
'''

MAX_LINES = 100

def reader(song, startLabel):
  lip = 0
  start = 0
  refrain = 0
  refrain_return = 0
  finished = False

  # Get list of lyric lines
  song_lines = song.splitlines()
  
  # Find startLabel, refrain and refrain return
  for i in range(0, len(song_lines)):
    if song_lines[i] == startLabel:
      start = i + 1
    elif song_lines[i] == '[REFRAIN]':
      refrain = i + 1
    elif song_lines[i] == 'RETURN':
      refrain_return = i

  # Print lyrics
  line_count = 0
  lip = start
  while not finished and line_count < MAX_LINES:
    line_count += 1
    for line in song_lines[lip].split(';'):
      if line == '' and song_lines[lip] != '':
        continue
      if line == 'REFRAIN':
        song_lines[refrain_return] = 'RETURN ' + str(lip + 1)
        lip = refrain
      elif re.match(r"CROWD.*", line):
        crowd = input('Crowd: ')
        song_lines[lip] = 'Crowd: ' + crowd
        lip += 1
      elif re.match(r"RETURN [0-9]+", line):
        lip = int(line.split()[1])
      elif line == 'END':
        finished = True
      else:
        print(line, flush=True)
        time.sleep(0.5)
        lip += 1



reader(song_flag_hunters, '[VERSE1]')
```

Se trata de uma letra de música onde depois do refrão conseguimos fazer uma entrada de uma frase qualquer que vai se repetir ao final de todo verso da música. Analisando o programa conseguimos perceber que nossa *flag* está inserida no final de um verso secreto dentro do programa:

```sh
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'
```

Continuando, ao analisar a função que lê a entrada, mais especificamente nesse local:

```sh
 for line in song_lines[lip].split(';'):
      if line == '' and song_lines[lip] != '':
```

Percebemos que ele utiliza como um delimitador o caractere de ponto e vírgula (";"), isso é um erro de sintaxe que nos permite escrever qualquer frase e aplicar um ponto e vírgula depois, assim, conseguimos executar um comando dentro do programa.
Entre as intruções identificadas temos a seguinte função:

```sh
elif re.match(r"RETURN [0-9]+", line):
        lip = int(line.split()[1])
```

Essa função lê a entrada e procura pelo comando "*RETURN*" seguido de um número, isso faz com que o programa imprima um verso referente ao número que escrevermos.

### Resolução

Agora que encontramos o erro de sintaxe dentro do programa, podemos explorá-lo para acionar a funcionalidade presente dentro do programa e fazê-lo imprimir o verso que contém a nossa *flag*. Primeiramente, vamos seguir as instruções do programa e rodar o comando netcat fornecido dentro de uma linha de comando:

[![netcat_flag_hunters.jpg](https://i.postimg.cc/vHMq6dDd/netcat-flag-hunters.jpg)](https://postimg.cc/CdPHWXjv)

Como esperado, o programa começa a imprimir as estrofes e fica esperando uma entrada após "*Crowd:*" (Plateia). Utilizando as informações que nós temos vamos digitar:

```sh
writeup;RETURN 0
```
[![flag_hunter.jpg](https://i.postimg.cc/qqY2wy0Y/flag-hunter.jpg)](https://postimg.cc/Mf7cKcnb)

> **flag:** picoCTF{70637h3r_f0r3v3r_0099cf61}

### Conclusão
Neste desafio podemos perceber a importância de um código bem estruturado, pois qualquer erro de sintaxe ou entrada não validada pode ser explorada por atacantes e levar a uma vulnerabilidade de segurança grave.

Também conseguimos colocar em prática nossa habilidade de análise de código e engenharia reversa, duas ferramentas muito importantes e úteis dentro do cenário de cybersegurança.
