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

> [!TIP] -a é uma opção para listar os diretório ocultos
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
> Quando um arquivo tem começa com "-" o comando pode entender que ele é uma opção, ou seja, uma variável de ambiente. Para usar o comando use:
> ```sh
> wc -- -text.txt ou wc ./ -text.txt
>```
> Isso indica "Fim das Opções", o que significa que aquela string não será tratada como uma opção.

## Suportado e Não Suportado

Isso indica, no livro se um suporta uma das características, como stdin, stdou, -.

✅ Suportado = o comando sabe fazer isso

❌ Não suportado = o comando não possui essa funcionalidade
