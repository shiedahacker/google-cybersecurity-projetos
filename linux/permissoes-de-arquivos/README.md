# Permissões de arquivo no Linux

> **Contexto:** Google Cybersecurity Professional Certificate (módulo de Linux)
> **Habilidades:** `ls -la`, `chmod` (notação simbólica), análise de permissões, princípio do menor privilégio

## Descrição do projeto

Neste projeto, trabalhei como analista de segurança dando suporte à equipe de pesquisa de uma grande organização. Minha função era revisar as permissões dos arquivos e diretórios da pasta `projects` e ver se elas estavam de acordo com o que cada usuário realmente precisava acessar. Quando encontrava algo fora do lugar, usava comandos do Linux para corrigir e remover os acessos que não deveriam existir.

## Verificar detalhes de arquivos e diretórios

Antes de mudar qualquer coisa, eu precisava ver como as permissões estavam. Entrei no diretório `/home/researcher2/projects` e rodei:

```bash
researcher2@fda4a8a3c07e:~/projects$ ls -la
```

Estas são as linhas da saída que usei na análise:

```text
drwx--x--- 2 researcher2 research_team 4096 Nov  2 13:12 drafts
-rw--w---- 1 researcher2 research_team   46 Nov  2 13:12 .project_x.txt
-rw-rw-rw- 1 researcher2 research_team   46 Nov  2 13:12 project_k.txt
-rw-r----- 1 researcher2 research_team   46 Nov  2 13:12 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 13:12 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 13:12 project_t.txt
```

O `ls` lista o conteúdo de um diretório. Com a opção `-l`, a lista vem detalhada, mostrando permissões, dono, grupo, tamanho e data de cada item. A opção `-a` faz aparecer também os arquivos e diretórios ocultos, que no Linux são os que têm o nome começando com ponto (`.`). Eles não aparecem num `ls` comum, e foi justamente por usar o `-a` que encontrei o `.project_x.txt`, um dos arquivos que precisavam de ajuste. Ao todo, a pasta tinha um subdiretório (`drafts`), um arquivo oculto e quatro arquivos de projeto.

## Descrever a cadeia de permissões

A primeira coluna da saída é uma sequência de 10 caracteres que resume o tipo do item e quem pode fazer o quê com ele:

| Posição | Significado |
|---|---|
| 1º caractere | Tipo do item: `d` = diretório, `-` = arquivo comum |
| 2º ao 4º | Permissões do **usuário** (dono) |
| 5º ao 7º | Permissões do **grupo** |
| 8º ao 10º | Permissões de **outros** (qualquer outro usuário do sistema) |

Dentro de cada bloco, a ordem é sempre a mesma: `r` para leitura, `w` para escrita e `x` para execução. Se no lugar da letra aparece um hífen, aquela permissão não foi concedida. Usando o `project_t.txt` como exemplo, a sequência `-rw-rw-r--` mostra que ele é um arquivo comum, que o dono e o grupo podem ler e escrever nele e que os outros usuários só podem ler. Ninguém tem permissão para executá-lo.

## Alterar permissões de arquivo

A organização não permite que outros usuários tenham permissão de escrita em nenhum arquivo. Na listagem, o `project_k.txt` aparecia com `-rw-rw-rw-`, o que significa que qualquer usuário do sistema poderia alterá-lo. Para corrigir, usei:

```bash
chmod o-w project_k.txt
```

O `chmod` é o comando que altera permissões. O primeiro argumento diz o que mudar e o segundo indica o arquivo. No `o-w`, o `o` se refere a outros, o sinal de menos remove uma permissão e o `w` é a escrita. Depois do comando, o `project_k.txt` ficou com `-rw-rw-r--`.

Também ajustei o `project_m.txt`. Ele é um arquivo restrito, que só o dono deveria poder acessar, mas o grupo ainda tinha permissão de leitura (`-rw-r-----`). Removi com:

```bash
chmod g-r project_m.txt
```

Aqui, o `g` representa o grupo e o `r` a leitura. O arquivo passou para `-rw-------`, e agora só o `researcher2` consegue ler e editar.

## Alterar permissões de arquivo em um arquivo oculto

O `.project_x.txt` foi arquivado pela equipe de pesquisa, e por isso é um arquivo oculto. Depois de arquivado, ninguém deveria conseguir alterá-lo, mas o dono e o grupo ainda precisavam ler o conteúdo. Quando conferi, ele estava com `-rw--w----`: o dono e o grupo tinham permissão de escrita, e o grupo não tinha leitura. Resolvi tudo com um único comando:

```bash
chmod u-w,g-w,g+r .project_x.txt
```

As vírgulas permitem fazer várias alterações de uma vez. O `u-w` tira a escrita do dono, o `g-w` tira a escrita do grupo e o `g+r` dá leitura ao grupo. O arquivo ficou com `-r--r-----`. Daria para chegar no mesmo resultado com `chmod 440 .project_x.txt`, usando a notação numérica, mas preferi a simbólica porque fica mais fácil de ler e entender o que está sendo alterado.

## Alterar permissões de diretório

O diretório `drafts` deveria ser acessado apenas pelo `researcher2`. Em diretórios, a permissão `x` tem um papel diferente: é ela que permite entrar na pasta e acessar o que está dentro. Como o `drafts` estava com `drwx--x---`, o grupo ainda conseguia entrar nele. Para remover esse acesso, usei:

```bash
chmod g-x drafts
```

Com isso, o diretório ficou com `drwx------`, e só o `researcher2` tem acesso a ele e ao seu conteúdo.

## Resumo das correções

| Item | Antes | Comando | Depois |
|---|---|---|---|
| `project_k.txt` | `-rw-rw-rw-` | `chmod o-w project_k.txt` | `-rw-rw-r--` |
| `project_m.txt` | `-rw-r-----` | `chmod g-r project_m.txt` | `-rw-------` |
| `.project_x.txt` | `-rw--w----` | `chmod u-w,g-w,g+r .project_x.txt` | `-r--r-----` |
| `drafts/` | `drwx--x---` | `chmod g-x drafts` | `drwx------` |

## Resumo

Com essas mudanças, as permissões da pasta `projects` passaram a refletir o nível de acesso que cada usuário realmente deveria ter. Usei o `ls -la` para ter a visão completa dos arquivos e diretórios, incluindo os ocultos, e depois o `chmod` para fazer quatro correções: tirei a escrita de outros no `project_k.txt`, deixei o `project_m.txt` acessível só para o dono, transformei o `.project_x.txt` em somente leitura para o dono e o grupo e restringi o diretório `drafts` ao `researcher2`. Esse trabalho segue o **princípio do menor privilégio**, em que cada pessoa tem apenas o acesso necessário para fazer seu trabalho, e reduz o risco de alguém acessar ou alterar o que não deveria.
