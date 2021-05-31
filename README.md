# Formatar o README.md
O arquivo Readme.md é utilizado para apresentar as soluções do projeto mantido no repositório no formato de uma página de internet. O README é um arquivo com a extensão MD (MarkDown).

## Carregar imagem

![Text alt (alternativo) para a imagem](https://github.com/arleysouza/exemplo-github-2/blob/main/banner.png)

Para adicionar uma imagem no README.md é necessário antes carregar ela na lista de arquivos do projeto. Essa imagem foi obtida [aqui](https://digitalinnovation.one/artigos/como-fazer-login-corretamente-no-github-pelo-terminal).

## Formatações

### Sintaxe Markdown
Markdown (MD) é uma forma de estilizar texto na web. Com ele é possível controlar a exibição do documento; formando palavras como negrito ou itálico, adicionar imagens e criar listas são apenas algumas das possibilidades com o Markdown. Majoritariamente, Markdown é apenas um texto normal com alguns caracteres não alfabéticos inseridos, como `#` ou `*` (https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf).

### Headers
Os títulos utilizam a mesma notação HTML (h1 a h6), onde h1 é o título maior e h6 é o menor. O nível é representado pela quantidade de hashtags:
- `#Título 1`
- `##Título 2`
- `###Título 3`
- `####Título 4`
- `#####Título 5`
- `######Título 6`

### Código e instruções de programação
Códigos e instruções são representados envolvendo eles por \`crase\`, por exemplo, `console.log('oi')`. 
As instruções que ocupam mais de uma linha podem ser representadas usando três \`\`\`, por exemplo,
```
function somar(a,b){
 return a + b;
}
```
Podemos especificar a linguagem de programação para realçar a sintaxe:
```javascript
// JavaScript
function somar(a,b){
 return a + b;
}
```
```python
# Python
def somar(a,b):
 return a + b;
```
```sql
select * from tbcliente where idade > 21;
```




### Listas
A listas podem ser sem ordem ou ordenadas:
* Cada item da lista sem ordem é criado com um asterístico `*` no início da linha;
* Para criar uma sublista é necessário adicionar uma tabulação `<Tab>`:
  * No início dessa linha tem um `<Tab>` seguido por um `*`.

Lista ordenada:
1. Cada item é iniciado por um número seguido por `.`;
2. O próximo item da lista já é completado pelo editor do GitHub.

### Negrito e itálico
Use o `**` ou `__` para formatar negrito e `*` ou `_`para itálico.

__Exemplo de negrito__ usou a marcação `__Exemplo de negrito__`.

**Outra opção de negrito** usou a marcação `**Outra opção de negrito**`.

_Exemplo de itálico_ usou a marcação `_Exemplo de itálico_`.

*Outra opção de itálico* usou a marcação `*Outra opção de itálico*`.


