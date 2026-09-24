# Capítulo 1

Aqui o autor apresenta os comandos essenciais para começar a usar o Linux.

## Terminal vs Shell

*Terminal* é onde os comandos são digitados.

*Shell* é aquilo que interpreta os comandos para serem executados. É um código que manda seu OS executar o comando.

## Opções combinadas

Alguns comandos aceitam opções combinadas como

```sh
wc -lw myfile1 myfile2
```

Nesse caso, o wc vai contar: o -l conta linhas e o -w conta palavras.

> [!TIP]
> Alguns comandos aceitam mais de um argumento, como no caso de wc.

## Pipe e Pipelines

Pipelines são sequências de comandos conectados por um pipe (|), permitindo que a saída de um comando seja usada como entrada do próximo. Dessa forma, é possível criar fluxos de processamento de dados.

```sh
echo "Oi tudo bem?" | wc -w
```

Nesse exemplo, o caractere | conecta os comandos echo e wc. O echo envia o texto para sua saída padrão (stdout), e o wc -w recebe esse texto pela entrada padrão (stdin), contando a quantidade de palavras.

### Por que isso acontece?

Porque o 'wc' pode ler dados de duas formas:

* Recebendo um ou mais arquivos como argumento.
* Recebendo dados pela entrada padrão stdin, como acontece em um pipeline.

Como o echo envia sua saída para a stdin do wc, este consegue processar o texto sem precisar de um arquivo.

Essa é a grande vantagem dos pipes: eles permitem conectar a saída de um comando à entrada de outro, criando um fluxo contínuo de processamento de dados.

stdout -> pipe -> stdin

## Argumentos vs Stdin

Os programas podem receber informações de duas formas principais: por **argumentos** ou pela **entrada padrão (stdin)**.

**Argumentos** são valores passados na linha de comando. Em muitos comandos, eles indicam onde os dados estão, ou seja, o caminho.

Já a **stdin** recebe os próprios dados. Esses dados podem vir do teclado, de um pipe (`|`) ou de um redirecionamento (`<`).

> [!IMPORTANT]
> Existem dois tipos de argumentos: as *opções* e o *alvo*. Nesse contexto, trata-se dos argumentos *alvo*.

Exemplo:

```sh
wc -w arquivo.txt      # recebe o caminho do arquivo como argumento

echo "Oi tudo bem?" | wc -w   # recebe os dados pela stdin

wc -w < arquivo.txt    # recebe os dados do arquivo pela stdin
```

## Diretórios do Linux

* **/boot** é o diretório onde ficam todos os arquivos de boot do kernel do Linux.

* **usr/bin** é o diretório onde se guardam os arquivos binários dos comandos do Linux. Por exemplo, os comandos echo e wc são arquivos C compilados para binários.

* **usr/lib** é o diretório que guarda as bibliotecas do C, ou seja, são arquivos próprios do C ou de terceiros (usados pelo próprio sistema) para que o código funcione.

* **usr/local/bin** é o diretório onde é possível o usuário criar seus próprios comandos, basta compilar um arquivo binário e colocar na pasta. Assim, terá seus próprios comandos exclusivos.

* **usr/sbin** é usado para administração do sistema — isso não tem nada a ver com o root. São apenas comandos para o sistema Linux. Alguns precisam de root (ou sudo), outros podem ser acessados pelo usuário normal, da mesma forma que os comandos encontrados em usr/bin.

* **/proc** é um diretório onde ficam mostrados todos os processos que estão rodando no seu OS. Desde drivers, softwares de interface, até processos do kernel.

Como curiosidade, o comando 'top' lista o diretório proc/ em tempo real, como um gerenciador de tarefas do Windows.

```sh
top
```

* **/sys** é um diretório que mostra o próprio kernel.

* **etc** é chamado "et cetera", que vem do latim e significa "entre outras coisas". Pense como o 'etc' do nosso português. É um diretório de configuração do OS, o que significa que afeta todos os usuários.

* **~/.config** é uma pasta de configuração pessoal do próprio usuário e não afeta de maneira global como o etc.

## Caminho absoluto vs relativo

Os caminhos absolutos são caminhos que começam da sua pasta raiz e começam com /

Agora suponha que você está no diretório /home. Para procurar algo dentro do diretório, é preciso tirar a barra inicial, o que significa que nesse momento o usuário está usando um caminho relativo.

Exemplo:

Suponha que dentro do diretório home exista o diretório 'arquivos' e o subdiretório 'word'. Ao fazer isso, é possível acessá-los.

```sh
cd arquivos/word
```

Ainda dentro do diretório home:

```sh
cd /arquivos/word
```

Nesse caso dará erro, porque vai buscar a partir do diretório raiz, e não existe esse caminho a partir do raiz — somente dentro de /home.

Por convenção, ./ é o mesmo que sem a barra:

```sh
cd ./arquivos/word 
```

Existem situações em que, para acessar certos programas — como todo arquivo executável —, é obrigatório o uso do ./

### Por que arquivo executável precisa de ./

Como foi citado anteriormente, os comandos do Linux são programas executáveis. Os motivos são os seguintes:

1 - Para evitar ambiguidade 

Suponha que dentro do seu diretório '/home/nome-do-usuario' tenha um arquivo binário chamado ls. ls é o nome do comando padrão do OS, o que significa que, ao chamar o comando ls, ele sempre vai dar preferência ao comando global. Para executar o seu ls, é necessário usar ./ls.

Isso acontece porque existe uma variável de ambiente chamada $PATH, que mostra qual o caminho e a prioridade que o comando deve seguir. Para visualizar:

```sh
echo $PATH
```

É a localização usada para buscar onde está o comando.

Então, por convenção, sempre que for executar um executável, é pedido o ./, porque pode acontecer de algum dia o usuário criar um arquivo com o mesmo nome de um comando do Linux. 

2 - Segurança

Imagine que você, sem querer, baixa um arquivo malicioso na sua pasta de fotos. As suas fotos estão em '/home/seu-usuario/fotos', e existe um arquivo malicioso chamado ls lá.

Ao executar o comando ls, ele não vai executar o ls dentro da sua pasta, e sim o ls global (que está no $PATH, dito anteriormente), evitando erros de segurança acidentais.

Para executar o código interno, é preciso usar ./, que diz ao sistema: "***Ei Linux, eu quero executar não o ls padrão do sistema, e sim o ls que eu criei, que está dentro da pasta onde eu estou localizado***"

> [!TIP]
> Para ver em qual diretório você está localizado no momento, use o comando pwd, que significa 'print working directory'.
> ```sh
> pwd
> ```

## Comando ~/

O comando serve para redirecionar para o /home. Se no seu Linux há apenas um único usuário, então vai ir para o único usuário padrão.

```sh
cd ~/
```

Irá para sua pasta '/home/seu-usuário'.

É útil também para quando você está em outras pastas fora de home e dá um cd '~/pagina/html'. Ou seja, vai direto para o diretório home procurar a pasta desejada.

## Entendendo o comando ls -l

Como sabemos, o comando ls lista tudo aquilo que está no diretório. Já a opção -l adiciona mais detalhes sobre esse conteúdo. E é isso que vamos explorar.

Imagine que você está no diretório 'cd ~/' e executou o comando ls -ld no diretório .config. Como no exemplo abaixo:

![img-comando-ls-ld](assets/debia-comando-ls.png)

### drwx------

#### Entendendo cada símbolo

d -> A letra inicial mostra qual é o tipo daquele conteúdo. No caso, d é um diretório. Exemplo: se começar com - é um arquivo comum, se for l é um link simbólico. Ou seja, cada caractere inicial mostra o tipo desse conteúdo.

Após o caractere inicial, cada letra tem um significado:

r -> read significa ler <br>
w -> write significa (escrever/modificar) <br>
x -> execute significa (executar ou "entrar", em caso de diretório)<br>
'-' -> permissão nula

#### Entendendo as divisões

Essa string é dividida em grupos de três, precedidos por apenas um caractere. Então fica:

```sh
d rwx --- ---
```

* d -> é o tipo

* rwx -> representa o conjunto do dono, ou seja, quais as permissões o (user), o dono do diretório/arquivo, pode ter.

* --- -> Esse segundo conjunto representa os grupos.

    Os grupos servem para quando há vários usuários e o dono quer dar permissões coletivas.

    Imagine que, em uma empresa, os usuários João e Maria fazem parte do grupo Financeiro. É possível criar grupos e dar permissões para o grupo específico.

* --- -> Esse terceiro conjunto representa outros.

    Outros são os demais usuários que não pertencem ao grupo nem são donos.

    Vamos supor que temos cadastrado no computador o usuário Joaquim. Como Joaquim não é dono e nem faz parte do grupo Financeiro, então para ele — ou seja, o resto do mundo — não haverá permissão nenhuma. Nem de r, w e x.

> [!TIP]
>
> É possível modificar essas permissões: mudar o dono, colocar mais de um grupo, alterar as permissões de cada grupo, modificar as permissões de others.

### O número '2'

Esse número representa os hard links. Hard link é um sistema interno que aponta para o arquivo. Isso significa que o nome do arquivo/diretório não guarda o conteúdo em si, apenas a referência — ambos apontam para o mesmo lugar no disco.

Exemplo: 

```sh
ln arquivo1.txt arquivo2.txt
```

Tanto o arquivo1.txt como o arquivo2.txt estão apontando para o mesmo inode.

O inode é um conjunto de metadados que mostra as informações daquele arquivo/diretório e, de maneira resumida, guarda o ponteiro para o conteúdo real no disco.

Ou seja, se eu mudar o arquivo1.txt, o arquivo2.txt também muda, pois ambos apontam para o mesmo conteúdo. E vice-versa.

Aqui está uma imagem como exemplo:

![Hardlinks](assets/Inode-Arquivos.png)

Existem 2 hard links, pois cada arquivo aponta para o mesmo inode.

Se usar um ls -l, em ambos vai aparecer o número 2 na contagem de hard links — porque tanto arquivo-1 quanto arquivo-2 apontam para o mesmo inode.

Ou seja, se mudar o arquivo A, o arquivo B também muda, porque ambos apontam para o mesmo lugar.

> [!IMPORTANT]
> Em diretórios, o . e o .. apontam para inodes. Enquanto o . aponta para o próprio diretório, o .. aponta para o diretório pai — o que conta como hard link extra sempre que há subdiretórios.

### Dois nomes iguais

O primeiro nome 'ismael' representa quem é o dono daquele arquivo. Ou seja, é o nome do meu usuário, configurado no momento da instalação do OS.

Como estou na pasta home, no meu usuário, tudo aquilo que eu criar pertence a mim, e cada usuário criado posteriormente terá seu próprio diretório dentro de home.

O segundo nome representa o grupo. Como eu sou o único usuário no meu OS, no momento da instalação é criado um grupo com o mesmo nome do usuário, de maneira padrão, no caso do Debian. Outras distros podem mudar esse comportamento.

Como foi dito na dica acima, é possível mudar o nome do grupo e criar outros.

### bytes

O número 4096 representa o tamanho daquele arquivo/diretório. Sua unidade de medida é em bytes.

### Modificação

'ago 4 16:22' é a data e hora da última modificação.

### .config

É o nome do arquivo/diretório.

## Entendendo `.` e `..`

Quando você usa `ls -a` num diretório, aparecem duas entradas que normalmente ficam escondidas: `.` e `..`. Usando `ls -al`, dá pra ver que as duas são do tipo `d` — ou seja, são **diretórios** de verdade, não atalhos nem nada especial.

- `.` aponta para o **próprio** diretório onde você está.
- `..` aponta para o diretório **pai** (um nível acima).

O interessante é que essas duas entradas não são "à parte" — elas contam como **hard links de verdade** pro inode do diretório. E é aí que mora a explicação do número que aparece na coluna de links do `ls -l`.

### Por que o diretório `/home/ismael` tem 4 hard links?

```sh
$ ls -ld /home/ismael
drwxr-xr-x 4 ismael ismael 4096 ago 27 10:00 /home/ismael
```

Repara no `4` logo depois das permissões — essa coluna mostra quantos nomes (hard links) apontam pro inode daquele diretório. Pra chegar em 4, dá pra contar assim:

**1 e 2 - os dois links "de fábrica" que todo diretório já nasce com:**
1. O **nome do próprio diretório**, visto de fora — `ismael`, dentro de `/home`, apontando pro inode do diretório.
2. A entrada **`.`** dentro dele mesmo, que aponta pra si próprio (mesmo inode).

Só isso já dá 2. Todo diretório vazio (sem subpastas) tem exatamente esses 2 hard links.

**3 e 4 - um a mais pra cada subdiretório direto:**

Dentro de `/home/ismael` existem dois subdiretórios: `.config` e `linuxpocketguide`. Cada um deles tem, dentro de si, uma entrada `..` — e essa `..` aponta de volta pro pai (`/home/ismael`). Ou seja, cada subdiretório soma **mais um hard link** pro inode do pai.



Esse `..` dentro de `.config` é o 3º hard link de `/home/ismael`. O `..` dentro de `linuxpocketguide` é o 4º.

### Resumindo a fórmula

```text
hard links do diretório = 2 (nome próprio + '.') + quantidade de subdiretórios diretos (cada um soma um '..')
```

No exemplo: `2 + 2 subdiretórios (.config e linuxpocketguide) = 4`.

### Por que isso é útil saber

Esse número na coluna de links é, na prática, um jeito rápido de saber **quantos subdiretórios diretos** uma pasta tem, sem precisar listar o conteúdo — é só pegar o total e subtrair 2. 

**Importante:** isso só vale pra **diretórios**. Arquivos comuns não ganham hard links "de fábrica" eles começam com 1 link (o próprio nome), e só sobem se você criar hard links manualmente como será visto.

## Recursos do Bash

Os recursos do Bash são uma forma de deixar seus comandos mais poderosos, o que facilita certas automatizações. Ao invés de listar todos os itens de uma lista, é possível usar certos recursos do próprio shell para filtrar, por exemplo, todos os arquivos ou diretórios que começam com a letra 'a'.

Enquanto os comandos são programas escritos em C e estão fisicamente no diretório usr/bin, os recursos do shell são estruturas nativas que o próprio shell entende, pois ele foi feito para reconhecê-las.

O Bash executa comandos, mas tem recursos que, em vez de rodar programas isolados, permitem combinar comandos diferentes com funcionalidades do próprio Bash, para automatizar tarefas.

Os recursos do Bash podem ser: Globbing, Redirecionamento, Pipes, Variáveis de Ambiente e Variáveis do Shell. Ou seja, são muitos recursos para deixar seus comandos mais poderosos do que usá-los de forma isolada.

Neste momento, vamos explorar alguns recursos do Bash.

Por curiosidade, para saber onde o Bash está instalado, digite:

```sh
echo $SHELL
```

Existem vários shells, mas o Bash é um dos mais famosos.

## O que é Globbing

Globbing é um recurso do Bash (não é exclusivo dele, outros shells também têm) para criar **filtros de nomes de arquivo/diretório**, usando caracteres coringa. Por exemplo, com o globbing é possível, num comando `ls`, filtrar apenas o que começa com a letra `a`, ou o que termina com `.config`.

O globbing **não é um filtro no sentido de "programa que processa"** ele é feito pelo próprio shell **antes** do comando rodar. O shell olha o padrão, expande para os nomes que existem no disco e casam com aquele padrão, e só depois passa essa lista pronta como argumento pro comando (`ls`, `echo`, `rm`, etc).

**Importante:** se o padrão não casar com nada, por padrão o Bash não "some" com o argumento, ele devolve a própria string literal do padrão, sem expandir. Ou seja, o comando recebe o texto do jeito que você escreveu, com isso o comando interpreta e não encontra aquilo que digitou porque simplesmente não existe.

## Padrão `*`

O `*` é o coringa mais comum e serve para representar "qualquer sequência de caracteres" (inclusive nenhuma). Ele funciona como um **preenchimento**: onde você põe o `*`, o shell aceita qualquer coisa ali.

```sh
echo /home/config*
```

Isso lista tudo que **começa com** `config` (o `*` está depois, preenchendo o que vem à direita), por exemplo `config`, `config.bak`, `configuracoes/`.

```sh
echo /home/*.config
```

Já isso lista tudo que **termina com** `.config` (o `*` está antes, preenchendo o que vem à esquerda) por exemplo `app.config`, `meu-projeto.config`.

**Resumindo a lógica do `*`:** o coringa "puxa" pro lado oposto de onde ele está escrito. Se ele vem *depois* do texto fixo, o texto fixo é o começo do nome. Se ele vem *antes*, o texto fixo é o final do nome.

## Filtrando só diretórios: a barra `/` no final

Se você quer só pastas, "não quero arquivos, só diretórios", basta colocar uma `/` no final do padrão:

```sh
echo /home/config*/
```

Isso diz ao shell: "só me dê os resultados que, com essa barra no final, ainda formam um caminho de diretório válido". Isso inclui tanto **diretórios comuns** quanto **links simbólicos que apontam para diretórios** (lembra do `l` no começo da linha quando você faz `ls -l`?). Arquivos comuns e links que apontam pra arquivos ficam de fora.

## Resumindo tudo

| Padrão | O que faz |
|---|---|
| `config*` | Casa com o que **começa** com `config` |
| `*.config` | Casa com o que **termina** com `.config` |
| `config*/` | Casa com o que começa com `config` **e é diretório** (ou link pra diretório) |
| Sem padrão nenhum casando | Bash devolve a **string literal** do jeito que foi escrita |

 
## Pegadinha comum: `ls` "abrindo" as pastas sozinho
 
Se você rodar `ls /home/config*/` (ou `ls /home/*/`) e o resultado vier com o **conteúdo de dentro** das pastas em vez dos nomes das pastas, não é o glob que fez isso — é o comportamento padrão do `ls`.
 
Por padrão, quando você passa um **diretório** como argumento pro `ls`, ele não lista o nome do diretório: ele entra e lista o que tem **dentro**. O glob só entregou os nomes dos diretórios pro `ls`; quem decidiu "abrir" cada um foi o próprio `ls`.
 
Pra ver só os nomes dos diretórios, sem abrir o conteúdo, use a flag `-d`:
 
```sh
ls /home/config*/       # abre cada diretório e lista o conteúdo de dentro
ls -d /home/config*/    # lista só os nomes dos diretórios, sem entrar neles
```
 
Essa é a confusão mais comum: parece que o `*/` "não filtrou direito", mas na real o filtro funcionou certinho quem entrou nas pastas foi o `ls` sem o `-d`.
 
## Outros padrões

Existem outros padrões de filtro que é o `?`, que representa um unico caractere e o `[]` onde é possivel buscar por um intervalo.

## Variáveis de Ambiente

As variáveis de ambiente são valores do OS que podem ser consultadas em tempo de execução. É como se fosse váriaveis no desenvolvimento de software, onde é possivel acessar, ou modificar informações. As variáveis de ambiente tem configurações que mostram seu nome do usuário, o formato de data e hora para o sistema, qual será o editor de código padrão e etc.

### Variáveis e ambiente vs variáveis de shell

Como dito anteriormente, as variáveis de ambiente são valores que pertecem ao o OS. Isso significa que se desligar o computador ou abrir um novo shell, elas vão continuar existindo.

Já as variáveis de shell, são valores que pertecem apenas aquele shell específico onde foram criadas. Isso significa que se criar um shell pai, o shell filho não terá acesso aquela variável

As variáveis de ambiente pode ser acessadas por qualquer shell

### Criando uma variável de ambiente

```sh
# Veja todas as variáveis de ambiente no OS. printenv = (print envoriment)
printenv 

#Definir temporariamente só na seção
export MINHA_VAR="valor" # com o export, a variável não vai pertencer somente ao seu shell, mas se desligar o pc ela não persistirá. Por isso é necessário salvar

# Agora sua variável de ambiente está salva
echo 'export MINHA_VAR="valor"' >> ~/.bashrc

# Recarrega o .bashrc
source ~/.bashrc
```

> ![IMPORTANT]
> 
> O source é importante, porque está alterando o disco, pois quando você faz o source o linux pega as informações do disco e recarrega na RAM

> ![IMPORTANT]
>
> Se não colocar 'export' na criação da sua variável de ambiente, o 'printenv' não irá conseguir listar, porque sem 'export' sua variável será de shell e não de ambiente. No caso se for uma variável de shell, é necessário usar o comando `set` ou `echo $MYVAR`para lista-lo.

No nosso exemplo, estámos criando variáves de ambiente local, ou seja, especifica para o seu usuário. Para variáveis globais, é necessário colocar no `/etc/environment`, o que precisa de permissão de adiministrador. É um pouco mais compicado de mexer, não vamos fazer isso agora.

### `printenv` vs `echo`

O `printenv` foi programado para ler a memória interna do sistema. Ele precisa saber o nome da gaveta (USER) para abrir e olhar o que tem dentro. Se você der o valor para ele usando $USER, ele vai tentar procurar uma gaveta chamada `joao`, que não existe.

```sh
printenv $HOME #O shell não vai encontrar pois o valor é seu-usuario
```

O certo é sem o `$` para esses comandos especiais, porque é são comandos específicos para gerenciar váriaveis. 

Como foi dito anteriormente, o `printenv` serve apenas para printar variáveis de ambiente, porém o `echo` consegue ler tanto variáveis de ambiente como de shell. O `echo` e outros comandos,  não conseguem acessar o variável nativamente, porque é necessario o `$`.

O `$` é uma função especifica do própio shell, capaz de entregar o valor da variável para o comando como argumento. Ou seja:

```sh
ls -ld $HOME
```

A maior parte dos comandos não acessam as variáveis por conta própia.

## Caminhos de Busca

Para localizar um comando no linux, use  which ou type.

```sh
which how

# /usr/bin/who
```

### Alises

Alises são abreviações de estrutura de comandos. Inves de a todo momento digitar:

```sh
ls -lG
```

É possível criar seus própios comandos para simplificar a escrita

```sh
alias td='ls -l'
```

No momento de digitar seu própio comando:

```sh
td # Executa o mesmo que ls -l
```

Esse novo comando, tem que ser salvo no ~/.bashrc para ser disponivel em outros shells futuros. 

> ![IMPORTANT]
> Alias, export e cd são comandos do shell e não scrips em c. O própio shell tem isso programado.

Para saber se um comando é um alias, ou se é um shell interno ou um um comando script c (comando de arquivos), use type

```sh
type cd l ll
```

## Entrada Saída e Redirecionamento

## Uso de `<`

O comando `<` permite ao shell, pegar o conteúdo do argumento e jogar ele ao comando, de maneira automática, acessando apenas os dados. Sem o `<`, é o comando que vai até o inode e procura o conteúdo, para que seja procesado. Então qual a diferença?

1. Linha de Montagem: Se um comando aceita `<`, então aceitará pipes `|`

```sh
grep "error" < file.txt
```

```sh
cat file.txt | grep "error"
```

2. Existem comandos que permitem entrada de dados, ou seja, é possivel coloar um script para colocar dados automaticos, como por exemplo um banco de dados que precis ser alimentado. Inves de colocar tudo manualmente, já entrega um arquivo com todas informações `comando < file`

3. Limpar saida do terminal: Há certos comandos como `wc` que no output, não mostram o dado puro como:

```sh
wc -l arquivo.txt #stdout: 10 arquivo.txt
```

```sh
wc -l < arquivo.txt #stdout: 10
```

Isso é interessante se precisar fazer calculos. Uma forma comum de limpar a saida.

## Criando arquivos

1. `comando > outifile` cria ou altera ou sobrepõe o outfile
2. `comando >> outfile` adiciona conteudo para o arquivo

> ![IMPORTANT]
>
> É usado para criar arquivos simples, como um arquivo de texto que o Linux aceita de maneira facil.
> Já outros arquivos como o pdf, é mais complexo porque depende de aplicações que o Linux não suporta nativamente, por isso é importante baixar.

É possível salvar o comando do log de error:

```sh
cat "oi" > teste.txt 2> errorfile.txt
```

O cat é um comando que abre arquivos, ao executar este código, ele cria `teste.txt` e `errorfile.txt`, se não existir é claro. Se o comando não der erro, vai escrever em `teste.txt` e se der erro vai escrever em `errorfile.txt`. Isso é usado em opções combinadas. 

Ou seja, se o arquivo"oi" existir escreve o conteúdo no `teste.txt` se não escreva no `errorfile.txt`.

```sh
cat "oi" &> errorfile.txt
```

Já esse comando, cria um arquivo unico para o erro, se der erro apenas escreve o log de erro que é chamados `stderr`, qué é abreviação de *standard error*. m
