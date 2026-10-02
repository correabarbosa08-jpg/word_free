# Word Frequency

**Tier:** 1-Beginner

Calculating the frequency of words in a block of text is a technique which has
various uses in algorithms such as searching, sorting, and semantic analysis.
The objective of the Word Frequency app is count the frequency of words in a
block of text and create a tabular display of each unique word in the text
along with its frequency, in descending order by frequency.

## User Stories

-   [ ] User can see a text input box, a 'Translate' button, and a word
        frequency table.
-   [ ] User can enter text (or cut and paste) into the input box. This input
        box must allow the entry of large blocks of text (maximum of 2048 characters).
-   [ ] User can click the 'Translate' button to analyze the word frequency in
        the text that has been input.
-   [ ] User can see an error message if the text input box is empty.
-   [ ] User can see the word frequency table populated when the 'Translate'
        button is clicked. Each row in the table contains a word and the number of times
        it occurs in the input text.
-   [ ] User can see the word frequency table ordered in descending sequence
        by word frequency.

## Bonus features

-   [ ] User can see a graphical representation of the word frequency in a
        bubble chart, column chart, or any other form of graphical representation the
        developer chooses.
-   [ ] User may choose to enter the URL of a web page whose content is to be
        analyzed instead of manually entering text. (Hint: See the
        [Podcast Directory](../2-Intermediate/Podcast-Directory-App.md) application for ideas).

## Useful links and resources

-   [Bag of Words Model (Wikipedia)](https://en.wikipedia.org/wiki/Bag-of-words_model)
-   [Semantic Analysis (Wikipedia)](https://en.wikipedia.org/wiki/Sentiment_analysis)

## Example projects

- [Word Frequency Counter](https://codepen.io/maxotar/pen/aLrwJM)
- [Bubble Chart](https://codepen.io/Quendoline/pen/pjELpM)
- [Svelte Word Frequency by Gabriele Corti](https://codepen.io/borntofrappe/pen/QWWWqQM)
## Funcionalidades

Stack: **HTML + CSS + JavaScript puro**, sem frameworks nem bibliotecas externas.

1. **Área de input de texto**
   - `<textarea>` com `maxlength="2048"` e contador de caracteres (`0/2048`).
   - Aceita digitar ou colar blocos grandes de texto.

2. **Botão 'Translate'**
   - Ao clicar, analisa o texto (ou a URL) e preenche a tabela de frequência.
   - Normalização: tudo em minúsculas, remove pontuação, mantém acentos
     (ex.: "Ação," e "ação" contam como a mesma palavra).
   - Tabela com as colunas **Palavra** e **Frequência**, em ordem decrescente
     de frequência (empate: ordem alfabética).

3. **Mensagem de erro se o input estiver vazio**
   - Se o texto (e a URL) estiverem vazios ou só com espaços, exibe
     "Digite um texto ou informe uma URL para analisar." abaixo do input.
   - A mensagem some assim que o usuário começa a digitar.
   - Também exibe erro quando a URL é inválida ou a página não pode ser carregada.

4. **Gráfico visual de frequência** (botões "Barras | Bolhas" para alternar)
   - **Barras:** barras horizontais feitas com HTML + CSS (`<div>` com largura
     proporcional à frequência), sem biblioteca de gráficos. Mostra as 10
     palavras mais frequentes, com o valor ao lado de cada barra. As barras
     crescem de 0 até o valor final com uma transição de CSS, uma após a outra.
   - **Bolhas:** gráfico em SVG com as 20 palavras mais frequentes. A área de
     cada círculo é proporcional à frequência; os círculos são posicionados
     ao longo de uma espiral, sem se sobrepor. Aparecem com uma animação de
     "pop"; o tooltip mostra palavra e frequência.
   - Com "reduzir movimento" ativado no sistema (`prefers-reduced-motion`),
     as animações são desligadas.

5. **Ignorar palavras comuns** (checkbox, desmarcado por padrão)
   - Remove da contagem artigos, preposições, pronomes etc. em português e
     inglês ("de", "a", "que", "the", "and"…), lista em `js/stopWords.js`.
   - Ao marcar/desmarcar depois de uma análise, o resultado é refeito na hora
     (a página da URL não é baixada de novo).

6. **Analisar uma página web pela URL**
   - Campo `<input type="url">` como alternativa ao texto manual; se preenchido,
     tem prioridade sobre a `<textarea>`.
   - O conteúdo é baixado com `fetch()`, o HTML é convertido em texto com
     `DOMParser` (removendo `<script>` e `<style>`) e então analisado.
   - Observação: navegadores bloqueiam requisições para outros domínios (CORS).
     Por isso a requisição passa por um proxy público, por exemplo
     `https://api.allorigins.win/raw?url=<URL codificada>`.

## Estrutura de pastas

```
wordfree/
├── app.md                    # Especificação e documentação do app (este arquivo)
├── package.json              # Scripts npm (start, test) — sem dependências
├── index.html                # Página: textarea, campo de URL, botão, erro, tabela e gráfico
├── css/
│   └── styles.css            # Estilos da página, tabela e barras do gráfico
├── js/
│   ├── wordFrequency.js      # Lógica pura: normalizar texto e contar palavras
│   ├── stopWords.js          # Lista de palavras comuns (português e inglês)
│   ├── fetchUrl.js           # Busca a página pela URL e extrai o texto
│   ├── chart.js              # Desenha os gráficos de barras e de bolhas
│   └── main.js               # Liga tudo: eventos, validação, erros e renderização
└── tests/
    ├── wordFrequency.test.js # Testes da lógica de contagem (node:test)
    └── chart.test.js         # Testes do posicionamento das bolhas
```

Os arquivos JS usam módulos ES (`<script type="module" src="js/main.js">`).
A lógica de contagem fica em `js/wordFrequency.js`, sem acesso ao DOM,
para poder ser testada isoladamente no Node.

## Comandos para rodar e testar

Pré-requisito para os testes: [Node.js](https://nodejs.org/) 20 ou superior.
Nenhuma dependência precisa ser instalada.

```bash
# Rodar o app com um servidor local (necessário por causa dos módulos ES)
npm start            # = npx serve .
# ou, com Python:
python -m http.server 8000

# Abrir no navegador
# http://localhost:3000  (serve)   ou   http://localhost:8000  (Python)

# Rodar os testes (test runner nativo do Node, encontra os arquivos *.test.js)
npm test
# equivalente a:
node --test
```

> Abrir o `index.html` direto pelo explorador de arquivos (`file://`) não
> funciona, porque o navegador bloqueia módulos ES carregados assim.

### Testes manuais

- [ ] Clicar em 'Translate' com tudo vazio → aparece a mensagem de erro.
- [ ] Digitar "a b a" → tabela mostra `a: 2`, `b: 1`, nessa ordem.
- [ ] Colar mais de 2048 caracteres → texto é cortado no limite.
- [ ] Gráfico mostra as mesmas palavras do topo da tabela.
- [ ] As barras crescem animadas ao clicar em 'Translate'.
- [ ] Botão "Bolhas" troca para o gráfico de bolhas, sem círculos sobrepostos.
- [ ] Marcar "Ignorar palavras comuns" com "o gato e o rato" → só `gato` e `rato`.
- [ ] Informar `https://example.com` → tabela e gráfico com o conteúdo da página.
- [ ] Informar uma URL inválida → aparece mensagem de erro.
