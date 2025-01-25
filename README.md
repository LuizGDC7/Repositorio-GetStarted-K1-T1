# Repositorio-GetStarted-K1-T1
## Comandos no Git
[![N|Solid](https://git-scm.com/images/logos/2color-lightbg@2x.png)](https://git-scm.com/doc)
## Configurações globais

```bash
    git config --global user.name # configura o nome do usuário global do git
    git config --global user.email # configura o email do usuário global do git
```

## Funcionamento
O git possui basicamente 5 estados: untracked, unmodified, modified, staged, commit.
### Untracked
São arquivos ignorados / não reconhecidos pelo git, um exemplo seriam arquivos que acabaram de ser gerados. A partir do momento em que usamos o comando git add no arquivo ou ele sofre um commit, ele estará sendo rastreado pelo git.
### Unmodified
São arquivos rastrados pelo git, mas que não sofreram alterações.
### Modified
São arquivos rastrados pelo git e que sofreram alterações passíveis de serem commitadas.
### Staged
Ao usarmos o comando git add, o arquivo vai para stage, ou seja, uma cópia do arquivo é preparada para o commit, que é como um checkpoint no desenvolvimento do código.

Se você quiser remover um arquivo do staged (por exemplo, quando quer reverter alterações feitas antes de um commit), você pode usar:

```bash
git rm --cached nome_arquivo # tira o arquivo do staged do git

git rm --cached . -r # remove todos os arquivos do estágio

git restore --staged nome_arquivo # tira o arquivo do stage, só funciona se houver algum commit feito antes

# git rm funciona com e sem primeiro commit, git restore precisa de pelo menos um commit feito.
```
### Commit
O checkpoint do desenvolvimento.

## Iniciando um repositório local

```bash
echo "# nome_repositório" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin url_repositorio_remoto
git push -u origin main
```
## Iniciando um repositório remoto

Tendo um repositório local em mãos e uma url de repositório limpo remoto, use:

```bash
git remote add origin url_repositorio_remoto
git branch -M main
git push -u origin main
```

## Verificando diferenças entre commits e arquivos.
Podemos verificar diferenças entre arquivos, commits e até mesmo branches usando:

```bash
git diff item1 item2 # Sendo item1 e item2 o objeto de comparação
```


## Log
O log é o histórico do nosso desenvolvimento. Nele podemos encontrar commits, merge's, tags e etc. Alguns comandos são:

```bash
git log # Mostra o histórico do desenvolvimento na branch atual
```

```bash
git log --oneline # Mostra o histórico da branch atual de forma resumida
```

```bash
git log -p # Mostra alterações feitas no commit
```

## Commits
Como já dito, são os checkpoints do nosso desenvolvimento. Ao fazermos alterações, podemos salvá-las no nosso repositório local usando:

```bash
git add .
git commit -m "mensagem"
```

Ou de forma resumida:

```bash
git commit -a -m "mensagem"
```

### Manipulando commits
#### Adicionar arquivos que ficaram para trás
```bash
git add nome_arquivo
git commit -amend --no-edit
```
#### Alterar mensagem de commit
```bash
git commit --amend -m "nova mensagem"
```
#### Desfazendo alterações antes do commit
```bash
git clean -f # Desfaz alterações e exclui arquivos untracked, não altera arquivos staged
git checkout . # Desfaz alterações, não altera arquivos untracked, não altera arquivos staged.
git checkout nome_arquivo # Desfaz alterações no arquivo até a versão do commit anterior.
```
#### Revertendo commits
Para reverter commits específicos (não os apaga do histórico), use:
```bash
git reverse hash_do_commit
git reverse HEAD # Esse aqui reverte o commit mais atual
```
#### Deletando commits
Isso reverte as alterações feitas pelos commits e os exclui do histórico de desenvolvimento.
```bash
git reset --hard # Excluir absolutamente tudo feito pelo commit, incluindo arquivos no stage e arquivos untracked.

git reset --mixed # Deleta o commit, mas não exclui os arquivos. Eles não estão no stage.

git reset --soft # Exclui o commit, mas as alterações anteriores que estiveram no staged do último commit permanecem no staged.

#Se for adicionado o parâmetro HEAD~X (sendo X um número inteiro) a qualquer um dos comandos anteriores, X commits sofrerão a deleção.
```
### Tags
Uma forma a mais de informação sobre os commits.
```bash
git tag # Cria uma tag no commit mais atual
git tag -a -m "mensagem" hash_do_commit # Tag com mensagem e autor será colocada no commit especificado
git tag -d nome_tag # Deleta a tag informada localmente
git push --delete origin nome_tag # Deleta a tag no servidor
```
### Compressão de commits
```bash
git rebase --interactive HEAD~X # (sendo x um número inteiro) Te permite comprimir vários commits em um só
```
## Gitgnore
O arquivo .gitgnore impede o track de arquivos específicos, ou seja, não são enviados ao repositório remoto ao final da etapa de desenvolvimento. Caso um arquivo que deveria ser ignorado pelo Git não tenha sido ignorado por engano, use:
```bash
git update-index --skip-worktree nome_arquivo
git update-index --no-skip-worktree nome_arquivo # Caso deseje reverter o comando anterior 
```
## Branches
Uma branch é uma etapa de desenvolvimento individual feita a partir de uma branch. Alterações feitas nela não afetam outras branches. Alguns comandos:
```bash
git branch # Lista as branches
git branch -m novo_nome # Renomeia a branch atual
git branch -d nome_branch # Deleta a branch atual, use -D no lugar de -d para forçar a deleção
```
### Criação
```bash
git checkout -b nome_branch # Ou também:
git switch -c nome_branch
```
### Troca entre branches
```bash
git checkout nome_branch # Ou também:
git switch nome_branch
```
### Enviando branch para servidos remoto
```bash
git --set-upstream origin nome_Branch # A url do repositório remoto receberá a nova branch.
```
### Deletando branchs
```bash
git push --delete origin nome_branch # Isso apaga a branch do servidor remoto, mas não do servidor local, é necesśario apagar no servidor local também.
```
### Recriando branch a partir do servidor
Você pode precisar disso caso alguma alteração forçada ocorra no servidor, caso aconteça, o histórico pode ser alterado. Com isso, podemos bagunçar o histórico com nossa branch atual. Se você quiser descartar a branch atual, pode usar:
```bash
git branch -D nome_branch_atual
git fetch origin nome_branch_atual # Vai trazer as informações mais recentes da branch do servidor, mas não vai aplicar elas ainda
git checkout nome_branch_Atual # Agora sim, vai aplicar as informações do servidor
```
O processo todo infelizmente descarta suas alterações, mas preserva o histórico e consistência.
### Renomeação
```bash
git branch -m novo_nome # Se você quiser renomear uma branch no repositório remoto, você precisa excluir essa branch do servidor, renomear localmente e enviar a branch para o servidor novamente.
```
### Merge
Junta duas branchs, caso alguma colisão ocorra é necessário resolver o conflito manualmente para maior segurança.
```bash
git merge branch_a_ser_mergida_na_atual
```

### Alguns adendos
Se você estiver alterando arquivos e trocar de branch e fizer o stage dos arquivos, os arquivos que foram alterados na outra branch estarão no stage da branch atual, o que pode ser indesejado e até perigoso. Para mudar isso, use:
```bash
git checkout -f nome_branch # Deleta as alterações feitas e faz a troca de branch.
```
Se você não quiser perder suas alterações, veja sobre git stash.

## HEAD
A head aponta para uma branch, que aponta para o último commit feito. Se você ir para um commit antigo e de lá criar uma nova branch, a HEAD vai apontar para essa nova branch

## Stash
Git stash é um comando que permite fazer um checkpoint sem fazer commit de nossas alterações. Em seguida, ele limpa as alterações do commit atual. Isso é ótimo caso precisemos trocar de branch urgentemente mas não queiramos perder nossas alterações feitas.
```bash
git stash # Executa o comando e cria o checkpoint
git stash apply nome_stash # Aplica o stash na branch atual
# git stash pop nome_stash # Aplica o stash e o remove da lista
# git stash drop nome_stash # remove o stash da lista de stashs
```
## Push
Envio de nossas alterações ao servidor.
```bash
git push # Envia nossas alterações se não houver conflito
git push --force # Sobrescreve o servidor remoto com o nosso
git push --force_with_lease # Sobrescreve o servidor se não houverem conflitos
```
## Rebase
Imagine que você começou a trabalhar numa alteração e usou como base uma branch. E em algum momento essa branch recebeu alterações no servidor remoto e você não quer conflitos ou simplesmente quer trabalhar na branch mais atual. Você pode fazer um rebase, e receber as alterações dessa branch. Será como se você tivesse trabalhado com essa branch nova em folha desde o começo.
```bash
git rebase nome_branch # Aplica as alterações da nome_branch na branch atual
git pull rebase # Evita a necessidade de merge commit ao fazer o git pull
```
## Cherry-Pick
```bash
git cherry-pick hash_do_commit # na branch atual, vai trazer um commit específico de outra branch para a atual e aplicar as alterações. Pode ser útil se for necessário trazer funcionalidades com velocidade sem precisar esperar elas serem concluidas.
```
## Bisect
Basicamente, te permite navegar entre commits usando busca binária até encontrar algo que você procura (feature quebrada ou funcionando, bugs, etc). Funcionamento:
```bash
git bisect start # começa o algoritmo
git bisect good hash_do_commit # commit inicial ou que ainda funciona
git bisect bad hash_do_commit # commit final que não funciona
```
Algoritmo:
```bash
git bisect good # se o funcionamento está de acordo
git bisect bad # se o funcionamento não estiver de acordo
# O processo dura até você encontrar o commit específico.
git bisect reset # Para parar o algoritmo
```
## Fetch
```bash

git fetch # traz as mudanças mais recentes do repositório remoto para o local, mas não aplica. Isso permite investigar o histórico, checar por mudanças feitas e ver como isso vai impactar no nosso repositório local. Se você quiser aplicar alguma coisa, é só dar merge na branch desejada.
```
## Grep
```bash
git comando | grep item_busca # é um filtro simples
```