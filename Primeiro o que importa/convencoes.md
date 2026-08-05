# Convenções

O Autor usa convenções para que entenda alguns termos que será necessário ao longo do livro

## Comando

*Comando* é uma instrução dada ao OS para ser executada.

Exemplo:

```sh
ls
```

> [!TIP] 
> 'ls' é um comando para listar conteúdos do diretório, mostrando arquivos e pastas.

## Argumento

*Argumento* é o alvo do comando

```sh
wc documentos.txt
```

> [!TIP] 
> 'wc' é um comando para contar linhas, palavras e bytes, caracteres

Ou seja, *documentos.txt* é o alvo do comando

## Opção

*Opção* é quando há mudança de comportamento no comando

```sh
ls -a
```

> [!TIP]
>  -a é uma opção para listar os diretório ocultos
>
> Ou seja, 'ls -a' signfica: ***liste os diretórios e os ocultos***

## Stdin (Entrada Padrão)

É usado quando o comando recebe dados. Pense como o "teclado".

## Stdout (Saída Padrão)

É para onde o comando envia o resultado. No começo pense apenas como a "tela"

## O hífen (-)

Alguns comandos aceitam hífen (-) como "Leia da Entrada Padrão" - Stdin ou "Escreva na Saída Padrão" - Stdout, ao invés de um nome de arquivo. Porém, se vai ser Stdin ou Stdout, depende do comando.

Exemplo

```sh
cat -
```

> [!TIP] 
> 'cat' serve para concatenar, ler/exibir arquivos de texto.

Quando cat é usado com hífen (-), ele espera que você digite um texto

> [!IMPORTANT] 
> Quando um arquivo começa com "-" o comando pode entender que ele é uma opção. Para usar o comando use:
> ```sh
> wc -- -text.txt ou wc ./ -text.txt
>```
> Isso indica "Fim das Opções", o que significa que aquela string não será tratada como uma opção.

## Suportado e Não Suportado

Isso indica, no livro se um suporta uma das características, como stdin, stdou, -.

✅ Suportado = o comando sabe fazer isso

❌ Não suportado = o comando não possui essa funcionalidade

## Coisas que aprendi além do livro nesse tópico

### sudo

O 'sudo' tem três características, e é usado para ter acesso ao modo root de maneira segura.

1° - Ele tem um 'root mode' por demanda. Ou seja, quando é acessado funções específicas, (como instalar um programa), é preciso permissões de administrador, o que exige uma senha, aumentando a segurança.

Com o Sudo, qualquer coisa que é acessado, pede uma senha. Essa senha, é uma forma de segurança para ter certeza que uma ação X é realmente aquilo que deseja.

Para acessar o modo administrador, como curiosidade, use o comando:

```sh
su -
```

> [!TIP] 
> 'su' significa super user, ou seja, é comando para ter acesso ao modo administrador.

Um exemplo para acessar o sudo.

```sh
sudo apt curl
```

Nesse caso está usando o modo administrador para instalar o pacote curl.

Resumindo: 

Com o modo root é possível ter acesso de maneira integral as funções de administrador, porém se fazer algum comando errado pode acabar quebrando o OS.

Já o Sudo é um modo root sobre demanda, onde é acessado apenas para aquela tarefa específica aumentando a segurança contra erros críticos, por exemplo, apagar o própio OS.

2° - O Sudo, não pede senha do root, e sim do usuário ao ser usado.

3° - O Sudo é configuravel, ou seja, é possível configurar para que comandos específicos não peçam senha.
