# Capitulo 1

Aqui o autor apresenta os comandos essenciais para começar a usar o Linux.

## Terminal vs Shell

*Terminal* é onde os comandos são digitados.

*Shell* é aquilo que interpreta os comandos para serem executados. É um código que manda seu OS executar o comando.

## Opções combinadas

Alguns comandos aceitam opções combinadas como

```sh
wc -lw myfile1 myfile2
```

Nesse caso o wc vai contar as -l conta linhas e o -w as palavras.

> [!TIP]
> Alguns comandos aceitam mais de um argumento, como no caso de wc.

## Pipe e Pipelines

Pipelines são sequências de comandos conectados por um pipe (|), permitindo que a saída de um comando seja usada como entrada do próximo. Dessa forma, é possível criar fluxos de processamento de dados.
```sh
echo "Oi tudo bem?" | wc -w
```

Nesse exemplo, o caractere | conecta os comandos echo e wc. O echo envia o texto para sua saída padrão (stdout), e o wc -w recebe esse texto pela entrada padrão (stdin), contando a quantidade de palavras.

### Porque isso acontece?

Porque o 'wc' pode ler dados de duas formas:

* Recebendo um ou mais arquivos como argumento.
Recebendo dados pela entrada padrão stdin, como acontece em um pipeline.

* Como o echo envia sua saída para o wc através do pipe, o wc consegue processar esse texto mesmo sem um arquivo.

Como o echo envia sua saída para a stdin do wc, este consegue processar o texto sem precisar de um arquivo.

Essa é a grande vantagem dos pipes: eles permitem conectar a saída de um comando à entrada de outro, criando um fluxo contínuo de processamento de dados.

stdout -> pipes -> stdin

## Argumentos vs Stdin

Os programas podem receber informações de duas formas principais: por **argumentos** ou pela **entrada padrão (stdin)**.

**Argumentos** são valores passados na linha de comando. Em muitos comandos, eles indicam onde os dados estão, ou seja, o caminho.


Já a **stdin** fornece os próprios dados ao programa. Esses dados podem vir do teclado, de um pipe (`|`) ou de um redirecionamento (`<`).

> [!IMPORTANT]
> Existem dois tipos de argumentos as *opções* e o *alvo*. Nesse contexto, trata-se dos argumentos *alvo*.

Exemplo:

```sh
wc -w arquivo.txt      # recebe o caminho do arquivo como argumento

echo "Oi tudo bem?" | wc -w   # recebe os dados pela stdin

wc -w < arquivo.txt    # recebe os dados do arquivo pela stdin
```

## Diretorio do Linux

* **/boot** é o diretório onde mostra todos os arquivos do boot do kernal do Linux.

* **usr/bin** é o diretório onde se guarda os arquivos binários dos comandos do Linux. Por exmplos os comandos, echo, wc são arquivos C compilados para binários.

* **usr/lib** é o diretório guarda  as bibliotecas do C, ou seja, são arquivos própios do C ou de terceiros (usados pelo própio sistema) para que o código funcione.

* **usr/local/bin** é o diretório onde é possível o usuário criar seus própios comandos, basta compilar um arquivo binário e colocar na pasta. Assim, terá seus própios comandos exclusivos.

* **user/sbin** usado para administração do sistema, isso não tem nada haver com o root. São apenas comandos para o sistema Linux. Alguns precisam de root (ou sudo) ou podem ser acessados pelo usuário normal, da mesma forma os comandos encontrados no usr/bin.

* **/proc** é um diretório onde mostra todos os processos que estão rodando no seu OS. Desde de drivers, sofwares de interface, o processos do kernal.

Como curiosidade o comando 'top' lista o diretório proc/ em tempo real, como um  gerenciador de tarefas do windows.

```sh
top
```

* **/sys** é um diretório que mostra o própio kernal.

## Caminho absoluto vs relativo

Os caminhos absolutos, são caminhos que começam da sua pasta raiz e começam /

Agora suponha que está no diretório /home. Para procurar algo dentro do diretório, é preciso tirar a barra inicial, o que significa que nesse momento o usuário está no diretório relativo.

Exemplo:

No primeiro exemplo, suponha que dentro do diretório home, exista o diretório 'arquivos' e o subdiretório 'word'. Ao fazer isso, é possível acessa-los.

```sh
cd arquivos/word
```

Ainda dentro do diretório home:

```sh
cd /arquivos/word
```

Nesse caso dará erro, porque vai puxar no diretório raiz e não existe esse tipo de caminho apartir do raiz, somente dentro do /home

Por convenção, ./ é o mesmo que sem a barra:

```sh
cd ./arquivos/word 
```

Existem situações para que que para acessar os certos programas, como todo arquivos executaveis, é obrigatório o uso do ./

### Porque arquivo executável precisa de ./

Como foi citado anteriormente, os comandos do Linux são programas executaveis. Então os seguites motivos são:

1 - Para evitar ambiguidade 

Suponha que dentro do seu diretório '/home/nome-do-usuario' tenha um arquivo binário chamado ls. ls é o  nome do comando padrão do OS, o que significa que ao chamar o comando ls, ele sempre vai dar preferência ao comando global. Para executar o comando ls seu, é necessário usar o ./ls.

Porque existe um variável de ambiente chamado $PATH que mostra qual o caminho e prioridade o comando deve seguir. Para vizualizar:

```sh
echo $PATH
```

É a localização para buscar onde está o comando.

Então por convenção, sempre os executaveis é pedido ./, porque pode acontecer de algum dia o usuário criar um arquivo que tenha o nome igual dos comandos do Linux. 

2 - Segurança

Imagina que você sem querer baixa um arquivo malicioso na suas pastas de fotos. As suas fotos estão no '/home/seu-usuario/fotos', e existe um arquivo malicioso chamado ls lá.

Quando executa o comando ls, ele não vai executar o ls dentro da sua pasta e sim o ls global (que está em $PATH, dito anteriormente) evitando erros de segurança acidentais.

Para executar o código interno, é preciso usar ./ que diz ao "***Ei Linux, eu quero executar não o ls padrão do sistema, e sim o ls que eu criei, que está dentro da pasta onde eu estou localizado***"

>[!TIP]
> Para ver qual repositório está localizado no momento, use o comando pwd que significa 'print working directory'.
> ```sh
> pwd
> ```

## Comando ~/

O Comando serve para se redirecionar para o para o /home. Se no seu linux tem apenas um único usuário, então vai ir para o único usuário padrão.

```sh
cd ~/
```

Irá para sua pasta '/home/seu-usuário'.

É útil também para quando está em outras pastas fora de home, e der um cd '~/pagina/html'. Ou seja, vai direto para diretório home procurar a data desejada

