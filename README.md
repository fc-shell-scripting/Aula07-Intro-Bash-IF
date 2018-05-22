# Introdução ao script

A medida que os problemas vão se tornando mais complexos, Começamos a sentir a limitação de apenas usar o direcionamento de arquivos e pipes para executar tarefas. Passamos a precisar armazenar dados, solicitar informações ao usuário e criar programas interativos.

Para resolver estes problemas, nós podemos usar um script. Script é uma rotina de comandos dentro de um arquivo para que um interpretador possa executar.

Algumas estruturas que ainda não foram vistas nas aulas anteriores agora passam a ser importantes, como if/else, while, for, etc..., que serão vistas nas próximas aulas, vão permitir a criação de interfaces e programas interativos com o usuário,incluido execução de comandos de acordo com alguma condição definida pelo programador.

### Alguns detalhes antes de começarmos
Quando colocamos nossos comandos dentro de um arquivo, para ser executado pelo interpretador, precisamos definir qual o interpretador queremos utilizar. Fazemos isso da seguinte forma:

```shell
bash script.sh
```
No comando acima estamos definindo que o interpretador bash irá executar os comandos do arquivo _script.sh_. A notação no final do arquivo não é obrigatória para o funcionamento, mas é uma bopa prática para identificarmos o que existe dentro do arquivo.

Mas, e se o script não for de nossa autoria? Como sabemos qual o shell que o autor original considerou para executar o script? Bom, para resolver este problema, podemos usar uma segunda forma de declarar qual o script que queremos usar, colocando na primeira linha do script o comando _bang_ (ou shebang), seguido do caminho do interpretador desejado:
```shell
#!/bin/bash
```
Os caracteres _#!_ no início do arquivo são conhecidos por _bang_.  A função deles é identificar qual o tipo de interpretador que será usado no script.
Se você chamar o script a partir de um outro shell, ele será substituído pelo shell presente na primiera linha de texto.

## ALERTA!
Dessa aula em diante, é interessante __não__ utilizar o terminal de seu sistem operacional primário, ou de algum servidor que está em utilização, porque vamos executar comandos de remoção de arquivos que podem, se digitados de forma errada, danficar o sistema operacional. Recomendo o uso de uma máquina virtual para a execução dos exercícios. __Se você não sabe como montar uma máquina virtual, [clique aqui para um artigo meu sobre o assunto](http://blog.czetta.com/Criando-uma-maquina-virtual/)__


## Hello World
Crie um arquivo chamado helloworld.sh. Dentro dele, insira o _shebang_ com o shell a ser usado:
```shell
#! /bin/bash
```
Na linha seguinte digite o comando echo, para mostrar uma mensagem na tela:
```shell
echo "hello world"
```

salve o arquivo, de permissão de execução no arquivo, com o comando:
```shell
chmod +x helloworld.sh
```

por último, execute o script, com a seguinte linha de comando:

```shell
./helloworld.sh
```
Na sua tela deve aparecer a frase "hello world".

## Executando comandos mais complexos
Você pode copiar e colar os comando dos exercícios das outras aulas dentro de um arquivo e executá-lo, da mesma forma que fizemos com o _helloworld.sh_. Para tarefas mais complexas, precisaremos armazenar variáveis dentro do script para uso posterior. Para isso, podemos criar uma _variável_, que é a forma que usamos para armazenar dados dentro do script. Vamos criar um script que mostre a mensagem "Olá usuario", onde usuário será substituído pelo nome do usuário.
```shell
#!/bin/bash
usuario="Joao"
echo "olá $usuario"
```
Quando este script for executado ele mostrará a mensagem "olá Joao". VOcê pode trocar o nome Joao por outro nome, e executar novamente para verificar o resultado.

Mas e se não soubermos o nome do usuário antes da criação do programa, ou se existirem múltiplos usuário? Podemos fazer a leitura das variáveia a partir da linha de comando, quando o programa for chamad para execução. Quando digitamos algum texto após o nome do script, estes dados que ficam na direita serão interpretados como variáveis, e nós podemos utilizar estas variáveis dentro do script. Para isso, precisamos entender como o shell interpreta o script e comandos:

```shell
.script.sh
```
ou
```shell
bash .script.sh
```
É interpretado da seguinte forma:

| comando (argumento 0)   
| :-------: 
| ./script.sh 
| bash ./script.sh

Então, quando passamos argumentos para o script:

```shell
./script.sh usuario usuario@empresa.com.br
```

será interpretado:

| Comando (argumento 0) | argumento 1 | argumento 2 |
| :-------------------: | :---------: | :---------: |
| ./script.sh           | Joao        | joao@empresa.com |

Os argumentos são separados por espaços, e por este motivo, se desejar passar uma frase, ou palavra composta, para o interpretador, coloque entre aspas.

Dentro do script, podemos acessar a variável de acordo com seu número na fila do comando. Se quisermos carregar o nome do usuário dessa forma, podemos alterar a variável dentro do script:

```shell
usuario="$1"
```
quando executarmos o comando, será necessário passar o nome do usuário, ou será mostrada a mensagem sem o nome do usuário.

```shell
./script.sh Joao
```

Agora completo, o arquivo ficará assim:

```shell {.line-numbers}
#!/bin/bash
usuario="$1"
echo "olá $usuario"
```

## IF
Este é o condicional mais simples que podemos usar dentro de scripts. A estrutura dele é simples:
```shell
if [[ condicao ]]; then
    #bloco de código
fi
```
Dentro dos condicionais, podemos incluir chamadas a subshell, usando o mesmo formato que usávamos diretamente na linha de comando
```shell
$(subcomando)
```

A estrutura _if_ possui mais uma parte, opcional, chamada _else_. O bloco else é executado quando o condicional do if é considerado como falso. A estrutura fica da seguinte forma:

```shell
if [[ condicao ]]; then
    #bloco de código, caso o condicional seja verdadeiro
else
    #bloco de códigos, caso o condicional seja falso
fi
```

Para exemplificar, vamos criar um programa que diga bom dia ou boa tarde, de acordo com o horário da máquina.

> Para este exemplo, você precisará desativar o ajuste automático de horário na sua máquina. 

```shell {.line-numbers}
#!/bin/bash

if [[ $(date +%H) > 12 ]]; then
    echo "Boa tarde"
else
    echo "Bom dia"
fi
``` 
Se quisermos complementar o comando acima, carregando o nome do usuário na mensagem de boas vindas, podemos complementar o código, para que fique da seguinte forma:

```shell {.line-numbers}
#!/bin/bash
usuario="$1"
if [[ $(date +%H) > 12 ]]; then
    echo "Boa tarde $usuario"
else
    echo "Bom dia $usuario"
fi
``` 

Altera a hora do seu ssitema e execute o comando novamente para perceber a diferença nas mensagens de acordo com cada horário.

## Exercícios
1. Crie um programa que receba duas palavras via linha de comando e mostre a mensagem "Iguais" quando as palavras forem iguais. caso contrário, mostre a mensagem "São diferentes".
2. Crie um programa que verifique se a pessoa possui idade para dirigir. Será recebido o nome e data de nascimento por parâmetros, no seguinte formato:
```shell
./script.sh nome 2018-05-21
```
Retorno uma mensagem para cada caso.

## Desafio

Crie o programa calc.sh que recebe 3 parâmetros, em ordem:

- primeiro numero
- operador (+ - / *)
- segundo numero

E retorna o resultado a operação entre eles:
EX:

```shell
./calc.sh 3 + 2
#mostra 5 na tela
```
