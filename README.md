<!-- Para usar o logo: coloque o arquivo em prints/logo.png e remova as marcas de comentario abaixo
<p align="center">
  <img src="prints/logo.png" alt="Logo" width="120">
</p>
-->

<h1 align="center">Painel Porsche</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Porsche-vendas-D5001C?style=for-the-badge" alt="Porsche">
  <img src="https://img.shields.io/badge/Excel-base%20de%20dados-217346?style=for-the-badge" alt="Excel">
  <img src="https://img.shields.io/badge/Claude-constru%C3%A7%C3%A3o%20da%20dashboard-D97757?style=for-the-badge" alt="Claude">
  <img src="https://img.shields.io/badge/HTML-arquivo%20%C3%BAnico-E34F26?style=for-the-badge" alt="HTML">
</p>

Dashboard interativa de vendas da Porsche, feita em **um único arquivo HTML**, a partir de uma planilha do **Excel** com 100 vendas. Construída com o **Claude**, como estudo de automação e tratamento de dados por prompt.

**Dashboard publicada:** https://ozeaguiar.github.io/dashboard-porsche-sales/

> Adicione aqui os prints depois de gerá-los:
>
> `![Visão geral](prints/dashboard.png)`
> `![Filtro aplicado: cidade Atlanta](prints/filtro-atlanta.png)`

---

## O que a dashboard faz

- **Filtros:** Modelo, Model Year, Cidade, Forma de pagamento e Período da venda. Todos atualizam a página inteira.
- **Indicadores de topo:** total de vendas, receita, ticket médio, modelo líder e ano-modelo líder.
- **Visual:** inspirado na identidade da Porsche, com fundo escuro, tipografia espaçada e vermelho como único acento. Funciona nos temas claro e escuro.

---

## Perguntas de negócio

### 1. Quais os principais modelos vendidos por cidade?

**Por que escolhi:** saber o que cada praça compra ajuda a decidir estoque e foco comercial por região.

**Como a dashboard responde:** a tabela "Modelos mais vendidos por cidade" lista cada cidade com suas vendas, receita e o modelo líder. Em caso de empate, todos os modelos aparecem. Clicar numa cidade filtra o painel inteiro.

**O que os dados mostram:**
- A base tem 79 cidades, quase todas com 1 ou 2 vendas, então os empates são comuns.
- Atlanta lidera em volume, com 3 vendas (Taycan Turbo, Taycan GTS e 911 Carrera Cabriolet, uma de cada).
- Las Vegas tem o 911 GT3 vendido 2 vezes, e Dallas tem o Taycan Turbo vendido 2 vezes. São os casos em que um mesmo modelo se repete na cidade.
- Por família, o 911 lidera com 23 vendas, seguido de Cayenne (18), Macan (17), Taycan (16), Panamera (14) e 718 (12).

### 2. Qual o ano-modelo que mais saiu em um período?

**Por que escolhi:** mostra se o mercado procura carros novos ou seminovos, e muda conforme o período analisado.

**Como a dashboard responde:** o gráfico "Vendas por ano-modelo" destaca o líder em vermelho, e o filtro "Período da venda" recorta por ano. O gráfico "Vendas por mês" mostra a evolução no tempo, com rótulos de mês e ano.

**O que os dados mostram:**
- No total, o ano-modelo **2024** lidera, com 29 vendas.
- Atenção: 13 dessas 29 vendas estão sem data válida. Ao olhar por ano de venda, a liderança muda. Em 2026, por exemplo, os anos-modelo 2023 e 2025 empatam em 7 vendas.
- Por isso o filtro de período importa: a resposta depende do recorte.

### 3. Um site de carros populares com base nos dados de cada cidade

**Por que escolhi:** transforma os dados em algo parecido com a vitrine de uma concessionária, mostrando o que é mais procurado.

**Como a dashboard responde:** a seção "Vitrine" mostra os 6 modelos mais populares em cards, com família, número de vendas, preço médio, anos-modelo e cidades. Ao escolher uma cidade, a vitrine passa a mostrar só o que é popular nela. A ordem é por unidades vendidas e, em empate, por receita.

**O que os dados mostram:** seis modelos empatam no topo, com 4 vendas cada: Taycan 4S, Cayenne Coupe, Cayenne E-Hybrid, Macan Electric, Panamera e Macan T.

---

## Tratamento da base (Excel)

A planilha original tem os campos em duas versões: o dado cru e o sanitizado. Antes de levar para a IA:

- Usei **somente as colunas sanitizadas**: modelo, ano-modelo, preço, quilometragem, forma de pagamento, cidade, estado, status de entrega e data da venda.
- Converti preço e ano para **número**.
- **Deixei de fora dados pessoais** (nome de cliente e de vendedor), porque a base vai embutida no HTML e o repositório é público.
- Criei a **família do modelo** (911, 718, Cayenne, Macan, Panamera, Taycan) a partir do nome, para agrupar os gráficos.
- **24 datas vieram como "INVALID".** Mantive essas vendas nos totais e criei a opção "Sem data válida" no filtro de período. Elas não aparecem no gráfico mensal, e a dashboard avisa isso.
- Algumas datas são **posteriores a hoje** (até 2027). Mantive como estão na base e deixei um aviso no rodapé.
- Os valores estão em **dólares** (as cidades são dos EUA), como na planilha.

---

## Ferramenta usada

- **Claude**, com geração do HTML e ajustes por conversa.
- Não usei ChatGPT com Canvas nem agente com skill.

---

## Prompt e evolução

**Prompt inicial** (resumo do que pedi):

> Usando o recurso de canvas, renderize uma dashboard em HTML ao lado, com os filtros Modelo da Porsche, Model Year, City e Payment method. Responda: principais modelos por cidade, ano-modelo que mais saiu em um período e um site de carros populares com base nos dados de cada cidade. Base visual: site da Porsche Brasil, com ar elegante e refinado.

**O que mudou até a versão final:**

1. Primeira versão gerada com filtros, KPIs, tabela de cidades, gráfico de ano-modelo e vitrine.
2. Os KPIs de receita e ticket médio quebravam em duas linhas. Reduzi a fonte e ajustei a largura.
3. A vitrine deixava cards soltos na última linha. Ajustei a grade para encaixar melhor.
4. O gráfico "Vendas por mês" mostrava só anos no eixo, com um rótulo "03/24" ambíguo. Troquei por rótulos mês/ano (mar/24, set/24 e assim por diante).
5. Pedi o símbolo da Porsche ao lado do título, mas o Claude não reproduz logotipos. Fica o cabeçalho tipográfico.

---

## Como publicar (GitHub Pages)

1. Salve o arquivo como `index.html` na raiz do repositório.
2. Em **Settings → Pages**, escolha *Deploy from a branch*, branch `main`, pasta `/ (root)`.
3. O link sai em `https://ozeaguiar.github.io/dashboard-porsche-sales/`.

## Como rodar localmente

Abra o `index.html` no navegador. Não precisa instalar nada.

## Estrutura

```
.
├── index.html     # dashboard completa (HTML, CSS e JS, com a base embutida)
├── README.md
└── prints/        # capturas de tela da dashboard
```

---

*Projeto de estudo. Porsche é marca registrada de seus respectivos titulares. Este painel não é afiliado à Porsche.*
