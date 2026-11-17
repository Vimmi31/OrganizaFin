# 💰 Controle Financeiro Pessoal

Aplicação web **100% client-side, responsiva e offline-first** desenvolvida em arquivo único (`HTML + CSS + JavaScript puro`), sem dependências externas obrigatórias, projetada para controle de fluxo de caixa mensal, acompanhamento de metas, projeção de parcelas e análise visual de gastos.

---

## ✨ Funcionalidades Principais

### 1. 📅 Mês Atual (`vMes`)
Visão operacional do dia a dia para gerenciar receitas e despesas do mês selecionado:
* **Indicadores de Topo:**
  * **Entradas:** soma da renda do mês com a sobra acumulada do mês anterior.
  * **Saídas:** total planejado, quanto já foi pago e quanto ainda falta pagar.
  * **Sobra Prevista vs. Em Caixa Hoje:** diferencia o saldo projetado para o fim do mês do dinheiro real disponível no momento (considerando apenas o que já foi marcado como recebido/pago).
* **Alertas Inteligentes:**
  * Aviso automático quando as despesas ultrapassam o dinheiro disponível.
  * Alerta de prazos para contas **vencidas** ou **vencendo nos próximos 5 dias**, com filtro rápido em um clique (*"Ver agora"*).
* **Distribuição e Metas por Categoria:**
  * Barra proporcional de gastos por categoria e acompanhamento de **teto mensal** (verde abaixo de 80%, amarelo próximo ao limite e vermelho quando estourado).
* **Lançamento Rápido e Calculadora Embutida:**
  * Botão `+` expansível com suporte a expressões matemáticas no campo de valor (ex.: `50+25,90` ou `120/3`), definição de vencimento, comentários e repetição automática.
* **Gestão de Contas na Lista:**
  * Marcar como paga/recebida individualmente ou em lote (*"Marcar listadas como pagas"*).
  * Edição direta de valor e vencimento na própria linha ou edição completa via modal (com opção de propagar novo nome/valor para os meses futuros).
  * Duplicação rápida de uma conta para o mês seguinte e exclusão com confirmação.
  * Ordenação (pendentes primeiro, vencimento, categoria, maior/menor valor, A–Z) e filtros (pendentes, pagas, urgentes, recorrentes e por categoria).

### 2. 📊 Gráficos e Saúde Financeira (`vGraf`)
Painel visual interativo construído em **SVG dinâmico** (adaptado automaticamente às cores personalizadas e aos temas claro/escuro):
* **Indicadores de Saúde Financeira:**
  * **Comprometimento da Renda:** percentual da renda mensal comprometido com despesas fixas e dívidas.
  * **Taxa de Reserva:** percentual da renda destinado à categoria de reserva/poupança no mês.
* **Fluxo de Caixa Mensal (6 ou 12 meses):**
  * Gráfico divergente onde as **Entradas** crescem para cima da linha `R$ 0`, as **Saídas** crescem espelhadas para baixo e uma **curva suave** mostra a evolução da sobra mensal. Ao passar o mouse ou tocar em cada coluna, um painel exibe os valores detalhados do mês.
* **Saídas por Categoria (Gráfico de Rosca / Donut):**
  * Alterna entre o **mês atual** e o **ano inteiro**, exibindo o total de despesas e a participação percentual e em reais de cada categoria.
* **Evolução da Reserva Guardada:**
  * Gráfico de área mostrando o crescimento acumulado de tudo o que já foi pago na categoria de reserva ao longo dos meses.
* **Parcelamentos em Andamento:**
  * Lista todas as compras/dívidas parceladas ativas no mês com barra de progresso, valor restante a pagar e previsão exata do mês de quitação.

### 3. 📆 Visão Anual (`vAno`)
Consolidação estratégica de janeiro a dezembro de qualquer ano:
* **Resumo Anual:** total de renda no ano, total de despesas planejadas e total de dívidas efetivamente quitadas.
* **Evolução Mensal Empilhada:** gráfico de colunas de 12 meses (`jan` a `dez`) dividido pelas cores de cada categoria, com linha tracejada indicando a **média mensal de gastos** e seleção interativa por mês.
* **Ranking de Categorias no Ano:** cards ordenados do maior para o menor gasto anual, exibindo o percentual sobre o total e a média mensal por categoria.

### 4. 🗂️ Histórico (`vHis`)
* Exibe o **patrimônio acumulado em reservas já pagas** em todo o histórico registrado.
* Lista todos os meses cadastrados em ordem decrescente com resumo de entradas, saídas e sobra; clicar em qualquer mês abre seus lançamentos diretamente.

### 5. 🔍 Busca Global e Atalhos
* Pesquisa em tempo real por **descrição, categoria ou comentário** em todos os meses cadastrados, exibindo a soma total dos itens encontrados e filtro opcional por intervalo de datas (`De` / `Até`).
* **Atalho `/`:** pressionar `/` fora de campos de texto foca imediatamente a barra de busca.

### 6. 🎨 Personalização e Tema
* **Modal Personalizar:** permite alterar o título exibido no topo, renomear categorias, escolher cores personalizadas, definir tetos mensais de gasto e alternar o tamanho da fonte (`Normal` ou `Grande`).
* **Tema:** alternância entre modo **Automático** (segue o sistema operacional), **Claro** e **Escuro**.

### 7. 💾 Armazenamento, Sincronização e Backup
* **Persistência Local:** salvamento automático no `localStorage` do navegador (`fin-vmb` e `fin-vmb-cfg`).
* **Arquivo Sincronizado (File System Access API):** em navegadores compatíveis (Chrome, Edge, Brave), permite vincular um arquivo `.json` local para salvamento automático em tempo real.
* **Backup e Planilhas:** exportação e restauração completa em `.json` e exportação formatada em `.csv` (separado por `;` com BOM UTF-8, pronto para abrir no Excel).

---

## 🛠️ Arquitetura e Referência de Funções (JavaScript)

O código é estruturado sem frameworks, dividido em módulos lógicos dentro da tag `<script>`:

### Utilitários, Datas e Matemática
| Função | Descrição |
| :--- | :--- |
| `BRL(n)` / `BRLk(n)` | Formata números no padrão monetário brasileiro (`R$ 1.250,00`) e em formato compacto para eixos de gráficos (`R$1.2k`). |
| `r2(x)` | Arredonda valores para 2 casas decimais evitando imprecisões de ponto flutuante. |
| `parseVal(raw)` | Converte textos digitados pelo usuário em número válido, suportando vírgula decimal e expressões matemáticas básicas (`+`, `-`, `*`, `/`). |
| `mkey(d)` / `shift(k, n)` | Gera chaves mensais no formato `AAAA-MM` e avança ou retrocede `n` meses no calendário. |
| `mdiff(a, b)` | Calcula a diferença exata em meses entre duas chaves `AAAA-MM`. |
| `mlabel(k)` / `mshort(k)` | Formata `AAAA-MM` para exibição por extenso (`Novembro de 2026`) ou abreviada (`nov/26`). |
| `dueIn(day, k)` / `addMonth(iso)` | Calcula a data de vencimento respeitando o último dia de cada mês (ex.: dia 31 vira dia 28/30 em meses mais curtos). |
| `daysBetween(a, b)` | Retorna a diferença em dias entre duas datas ISO (`AAAA-MM-DD`). |
| `smoothPath(pts)` | Gera curvas Bézier cúbicas suaves em SVG para a linha de tendência de sobra no gráfico de fluxo de caixa. |

### Motor Financeiro, Recorrência e Validação
| Função | Descrição |
| :--- | :--- |
| `ensure(k)` | Garante que o mês `k` exista na estrutura de dados, herdando automaticamente contas recorrentes e parcelas ativas de meses anteriores. |
| `sync(k, l, propFields)` | Propaga alterações de recorrência, número de parcela (`ix`), total (`n`) e, opcionalmente, nome/valor/categoria para os meses futuros. |
| `toggleRec(k, id)` | Ativa ou encerra a repetição mensal de um lançamento, alertando caso existam parcelas já pagas em meses futuros. |
| `calc()` | Percorre todos os meses em ordem cronológica calculando `carry` (saldo anterior), `inc` (entradas), `out` (saídas), `paid` (pagas), `sobra` (projetada) e `caixa` (saldo real pago). |
| `clean(o)` / `fixIx(m)` | Sanitiza e valida objetos importados de backups ou `localStorage`, reconstruindo índices de parcelas quando necessário. |
| `importData(o)` | Importa um backup `.json` validado e aplica configurações de tema/categorias se presentes. |

### Renderização e Interatividade
| Função | Descrição |
| :--- | :--- |
| `render()` | Controlador central da interface; decide qual aba ou resultado de busca desenhar no contêiner `#view`. |
| `vMes()` / `row(l, k)` | Renderiza a aba **Mês atual**, cards de resumo, alertas de vencimento, formulário de adição e cada linha de lançamento. |
| `vGraf()` / `renderAreaChart()` | Gera os SVGs do fluxo de caixa divergente, gráfico de rosca, área acumulada da reserva, KPIs e barra de parcelamentos. |
| `vAno()` | Gera o gráfico anual de colunas empilhadas por categoria, linha de média mensal e o ranking anual. |
| `vHis()` | Renderiza o acumulado histórico da reserva e a lista clicável de todos os meses. |
| `vBusca(q)` | Filtra lançamentos de todos os meses pelo termo buscado e intervalo de datas, exibindo a soma total. |
| `updateGrafTip(idx)` / `updateAnoTip(idx)` | Atualizam o destaque visual da coluna selecionada e o painel de detalhes nos gráficos sem precisar re-renderizar a página inteira. |

### Persistência e Arquivos
| Função | Descrição |
| :--- | :--- |
| `save()` / `commit()` | Salva o estado no `localStorage`, agenda a gravação no arquivo sincronizado (com debounce de 400 ms) e atualiza a tela. |
| `writeFile()` | Grava o JSON atualizado diretamente no arquivo local selecionado via File System Access API. |
| `download(b, name)` | Dispara o download de arquivos gerados em memória (`.json` e `.csv`). |

---

## 🚀 Como Usar

1. Baixe ou clone este repositório.
2. Abra o arquivo `index.html` diretamente em qualquer navegador moderno (desktop ou celular) — não requer servidor, instalação ou banco de dados.
3. Para maior segurança dos seus dados, utilize a opção **Criar arquivo sincronizado** (no rodapé) ou faça backups periódicos em `.json`.
