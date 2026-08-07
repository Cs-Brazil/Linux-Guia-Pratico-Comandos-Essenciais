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

## Argumentos vs stdin

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

* **/boot** é um diretório onde fica o kernal do Linux, é onde fica o nucleo do sistema.

* **usr/bin** é o diretório onde se guarda os arquivos binários dos comandos do Linux. Por exmplos os comandos, echo, wc são arquivos C compilados para binários.

* **usr/lib** guarda os diretório as bibliotecas do C, ou seja, são arquivos própios do C ou de terceiros (usados pelo própio sistema) para que o código funcione.

* **usr/local/bin** é o diretório onde é possível o usuário criar seus própios comandos, basta compilar um arquivo binário e colocar na pasta. Assim, você terá seus própios comandos exclusivos.

* **user/sbin** usado para administração do sistema, isso não tem nada haver com o root. São apenas comandos para o sistema Linux. Alguns precisam de root (ou sudo) ou podem ser acessados pelo usuário normal, da mesma forma os comandos encontrados no usr/bin.

## Caminho absoluto vs relativo

Os caminhos absolutos, são caminhos que começam da sua pasta raiz, que começa /.

Agora suponha que está no diretório /home. Para procurar algo dentro do diretório, é preciso tirar a barra inicial, o que significa que está procurando no diretório relativo.

Exemplo:

No primeiro exemplos, suponha que dentro do diretório home, exista os diretório arquivos e word. Ao fazer isso, é possível acessa-los.

```sh
cd arquivos/word
```

Ainda dentro do diretório home:

```sh
cd /arquivos/word
```

Nesse caso dará erro, porque vai puxar no diretório raiz e não existe esse tipo de caminho apartir do raiz

Por convenção, ./ é o mesmo que sem a barra:

```sh
cd ./arquivos/word
```