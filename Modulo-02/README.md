# Capítulo 2

Neste capítulo, serão aprendidos comandos essenciais para o uso diário do Linux via terminal.

## Operações Básicas de Arquivos

- [ls](#ls)
- [cp](#cp)
- [mv](#mv)
- [rm e rmdir](#rm-e-rmdir)
- [ln](#ln)
- [mkdir](#mkdir)

---

## `ls`

O comando `ls` tem a flag (opção) `-h`, porém ela só faz sentido junto com o `-l`. O `-h` mostra o tamanho dos arquivos em um formato legível (KB, MB, GB).

```sh
ls -lh
```

---

## `cp`

O comando `cp` é o de copiar e colar. Quando há:

```sh
cp file1.txt file2.txt
```

Ele copia o conteúdo do `file1.txt` para o `file2.txt`.

Agora, quando há um ou mais arquivos sendo copiados para um diretório, usa-se:

```sh
cp file1.txt file2.txt file3.txt directory/
```

Esse `/` serve para indicar que o destino é um diretório. O `cd` não precisa disso porque só aceita diretórios, já o `cp` precisa saber se o destino é um arquivo ou um diretório.

### Flags interessantes

Existem algumas flags interessantes no `cp` que valem a pena ser exploradas:

* `-p`: não copia apenas o conteúdo, mas também o timestamp, o grupo e o proprietário (se houver permissão). Quando copia com `-p`, é criado um novo inode, porém os dados são os mesmos do original.

* `-i`: pergunta antes de sobrescrever, é como um aviso.

* `-f`: força a sobrescrita.

* `-a`: usado para fazer uma cópia fiel de arquivos e diretórios. Ele é a união de três opções:
  * `-r`: copia diretórios e subdiretórios, bem como seus arquivos.
  * `-d`: copia o link simbólico em si, e não o arquivo para o qual ele aponta.
  * `--preserve=all`: mantém intactos os timestamps, permissões de acesso, dono, grupo, atributos estendidos e etc.

---

## `mv`

O `mv` pode ser usado para **renomear** ou **mover** arquivos.

Para renomear:

```sh
mv file1.txt file2.txt # Agora file1 virou file2
```

Para mover arquivos para um diretório:

```sh
mv file1.txt file2.txt directory/ # Agora file1 e file2 estão dentro de directory
```

> [!TIP]
> O `mv` também tem as flags `-i` e `-f`.

---

## `rm` e `rmdir`

Os dois apagam coisas, mas têm propósitos diferentes:

| Comando | Apaga | Observação |
|---------|-------|------------|
| `rm` | Arquivos | Com `-r`, também apaga diretórios e tudo que há dentro deles |
| `rmdir` | Apenas diretórios **vazios** | Se o diretório tiver algo dentro, dá erro |

### `rm`

O `rm` é usado para apagar arquivos:

```sh
rm arquivo1.txt
```

Já a flag `-r` (recursiva) serve para apagar diretórios e seus subdiretórios:

```sh
rm -r directory_2
```

A flag `-f` (force) não pede confirmação e ignora arquivos que não existem. Ela é comumente combinada com o `-r`:

```sh
rm -rf directory_2
```

> [!IMPORTANT]
> Tome cuidado com `sudo rm -r`, isso pode destruir o seu OS.

### `rmdir`

O `rmdir` serve para apagar diretórios vazios (apenas vazios). Ele não tem a flag `-r`, pois não apaga conteúdo:

```sh
rmdir directory_2/directory3
```

> [!IMPORTANT]
> O `rmdir` tem a flag `-p`: ele apaga toda a cadeia de pastas, do final para o início (todas precisam estar vazias). Se quer apagar apenas a pasta final, não use a flag.
>
> ```sh
> rmdir -p directory_2/directory3 # Apaga directory3 e depois directory_2
> ```

---

## `ln`

Há também os links.

Para criar um link simbólico (atalho):

```sh
ln -s /caminho/do/original nome_do_atalho
```

Já para criar um hardlink:

```sh
ln file1.txt file2.txt
```

---

## `mkdir`

O `mkdir` cria diretórios:

```sh
mkdir directory1 directory2
```

### Flag `-p`

Com a flag `-p`, é possível criar diretórios aninhados:

```sh
mkdir -p fotos/construção/laje
```

Nesse caso, é criado um diretório dentro do outro.

Também é possível criar vários diretórios de uma só vez dentro de um mesmo diretório. Veja:

```sh
mkdir -p fotos/construção/{laje,brita,areia}
```

### Flag `-m`

A flag `-m` é a abreviação de `--mode`, parecido com o `chmod`. O comando `chmod` serve para trocar as permissões de pastas e arquivos. Com o `-m`, é possível já criar diretórios definindo suas permissões de maneira direta:

```sh
mkdir -m 700 pasta_privada
```

Quando não é definido, o `mkdir` aplica o *umask*, ou seja, as permissões padrão.

> [!IMPORTANT]
> Se você combinar `-m` com `-p` (para criar pastas aninhadas, como `mkdir -pm 700 a/b/c`), a permissão definida no `-m` será aplicada apenas ao último diretório da cadeia (`c`). Os diretórios intermediários (`a` e `b`) serão criados com as permissões padrão do sistema.