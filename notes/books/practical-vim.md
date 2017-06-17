# Practical VIM

## Visual Mode

### Usando seleção visual com comandos Ex

Sempre que for realizada uma seleção visual você pode executar um comando Ex por simplesmente iniciar o comando com *:*,
como você faria se não existisse seleção alguma. O comando será executado apenas na seleção.


### Selecionando text objects

* Selecionar todo o texto entre duas chaves: *<i{>*
* Selecionar todo o texto entre dois ": *<i">*
* Selecionar todo o texto entre parênteses : *<i(>*


## Normal Mode

* Apagar tudo da posição do cursor até o final da linha: *<D>*
* Juntar linha atual a próxima linha: *<J>*
* Alterar palavra *<cw>*
* Iniciar edição no início da linha: *<I>*
* Iniciar edição no final da linha: *<A>*
* Centralizar tela na posição cursor: *<zz>*
* Adiantar para inicio da palavra: *<w>*
* Adiantar para fim da palavra: *<e>*
* Retroceder para inicio da palavra: *<b>*
* Retroceder para fim da palavra: *<ge>*
* Marcar linha para voltar depois : *<m{register}>*
* Voltar para a linha marcada anteriormente : *<'{register}>*
* Ir para o arquivo : *<gf>*


### Operar em Text objects

Praticamente todo o comando do vim pode operar em um text object, seguindo o padrão *<{comando}i{delimitador}>*.

Você simplesmente compõe esse comando com outros comandos que precisam de motions, como o *<d>*, *<y>* e *<c>* e o delimitador que você deseja utilizar. 

Também é interessante que além de delimitadores, text objects podem ser também textos como parágrafos (*<ip>*) e sentenças (*<is>*).

Exemplos:

Edição:
* Apagar todo o texto entre duas chaves e iniciar edição: *<ci{>*
* Apagar todo o texto entre dois " e iniciar edição: *<ci">*
* Apagar todo o texto entre parênteses e iniciar edição : *<ci(>*

Deleção:
* Apagar todo o texto entre duas chaves: *<di{>*
* Apagar todo o texto entre dois ": *<di">*
* Apagar todo o texto entre parênteses: *<di(>*

Copia:
* Copiar todo o texto entre duas chaves: *<yi{>*
* Copiar todo o texto entre dois ": *<yi">*
* Copiar todo o texto entre parênteses: *<yi(>*


### Edição do cursor até um carácter

*<{operador}t{caracter}>*

Exemplos de como realizar operações em um texto até que ele encontre um "(":

* *<dt(>*
* *<yt(>*
* *<ct(>*


### Substituindo texto em todos arquivos de um projeto

* vim -o <arquivos ou regex, como *.h>
* set hidden
* argdo s,<texto-antigo>,<novo-texto>,ge
* argdo update

Mais sobre "set hidden":http://usevim.com/2012/10/19/vim101-set-hidden. Já coloquei no meu vimrc ;-).

O regex pode ser algo como "**/*.h" para indicar que inclui sub-diretórios.


## Insert Mode

* Apagar palavra anterior: *<C-w>*
* Apagar toda a linha: *<C-u>*
* Fazer uma conta e inserir resultado no texto: *<C-r=>*
* Colar o conteúdo de um registrador: *<C-r{register}>*


## Macros

### Gravando macro

* Iniciar gravação de macro : *<q{register}>*
* Encerrar gravação de macro : *<q>*
* Executar gravação de macro : *<@{register}>*

### Reproduzindo macro

* Em paralelo: *<:{range}normal @{register}>*, onde o *register* eh onde estarão os comandos a serem executados.


## Comandos Ex

Todos os comandos Ex começam com o *:", seguidos pelo comando.

* Colar o conteúdo de um registrador no comando (util ao substituir): *<C-r{register}>*
* Listar registradores: *<reg>*


### Executando com filtro

Executar um comando apenas nas linhas que batem com um padrao: 

[range]g/<pattern>/cmd

Deletando todas as linhas com "abacate" em qualquer posicao:

%g/abacate/d


## Patterns

* Utilizar *\v* para habilitar o *Very magic search*, assim não é necessário fazer milhões de escapes (fica parecido com pearl e ruby).
* Utilizar *\V* para habilitar o *Very no magic search*, útil quando é necessário procurar por uma palavra cheia de caracteres bizarros.


## Registradores

### Uso

Qualquer comando que utiliza registrador (seja leitura ou escrita) pode ser pré-fixado com um registrador, seguindo o padrão:

* *<"{reg}{comando}>*

Exemplos:

* Colando do registrador de yank (0): *"0P*
* Copiando para o registrador de clipboard do sistema (+): *"+y*


### Registradores interessantes

* Yank register: *<0>*
* System clipboard: *<+>*


## Mapeamentos

* Utilizar *<Leader>* nos mapeamentos personalizados, evitando estragar mapeamentos naturais do vim.


## Autocomplete

* Iniciar e navegar na lista: *<C-n>*
* Aceitar opção: *<C-y>*
* Sair do autocomplete: *<C-e>*
* Completar linha inteira: *<C-x><C-l>*
* Omni Complete: *<C-x><C-o>*
* Completar com correção ortográfica: *<C-x><C-s>*


## Spell Checker

* Habilitar: *set spell*
* Desabilitar: *set nospell*
* Ir para o próximo erro: *<]s>*
* Ir para o erro anterior: *<[s>*
* Corrigir palavra (cursor deve estar encima dela): *<z=>*
* Adicionar palavra ao dicionário (cursor deve estar encima dela): *<zg>*
* Remover palavra ao dicionário (cursor deve estar encima dela): *<zw>*


## Plugins Interessantes 

* Comentando código: https://github.com/tpope/vim-commentary
* Class outline viewer: http://majutsushi.github.io/tagbar/
* Busca em texto selecionado no visual mode: https://github.com/bronson/vim-visual-star-search
* Supercharged substituition :-): https://github.com/tpope/vim-abolish
* Javascript: https://github.com/mozilla/doctorjs
* CTags + GIT: http://tbaggery.com/2011/08/08/effortless-ctags-with-git.html


## Configurações especificas por linguagem

É possível carregar um *vimrc* adicional além do que se encontra em *~/.vimrc*. Essa funcionalidade é provida pelo plugin:

http://vim.wikia.com/wiki/Keep_your_vimrc_file_clean

Basta criar o arquivo seguindo o padrão: *~/.vim/ftplugin/{filetype}.vim*

Exemplos de como fica o arquivo para algumas linguagens:

* *~/.vim/ftplugin/javascript.vim*
* *~/.vim/ftplugin/ruby.vim*
* *~/.vim/ftplugin/python.vim*

O vimrc deve estar configurado com o plugin filetype: 

* filetype plugin on
