# Swiss Top Cut Calculator (Calculadora de Top Cut para modo Suíco)

> **Web-based calculator to determine Top Cut chances, bubble scenarios and ID (Intentional Draw) math for Swiss tournaments.**

Uma ferramenta web leve e rápida desenvolvida para Juízes e Organizadores de Torneio (TOs) e Jogadores. Ela calcula instantaneamente as chances matemáticas de alcançar o Top Cut (Top 4, Top 8 ou Top 8 Assimétrico) durante as rodadas finais do sistema Suíço.

Acesse a calculadora online aqui: [Link](https://lucasmarbatista.github.io/swiss_top_cut_calculator/top_cut_calculator.html)

## 🚀 Novidades e recursos

- **Análise de risco:** A ferramenta agora vai além de dizer "Sim" ou "Não" para a bolha. Ela detalha o risco exato de uma decisão, avisando quando um jogador passa limpo se vencer, mas cai na bolha se aceitar um Empate (ID).
- **Corte projetado vs. Corte atual:** Separa visualmente quem tem chances reais acompanhando a curva do torneio (Corte Projetado) daqueles que dependem de resultados alheios (Apenas Corte Atual).
- **Suporte nativo a Top 8 assimétrico:** Regras exclusivas para este formato, onde não existe eliminação por desempate na linha de corte.
- **Dark mode automático:** Modo escuro integrado com suporte a preferência do sistema operacional e salvamento no cache local.
- **Balão explicativo:** Explicações interativas passando o mouse sobre os valores de corte.

---

## O que o código faz?

Durante as últimas rodadas de um torneio, calcular quem ainda tem chance de classificar e quem já está eliminado pode ser confuso, especialmente com os "Intentional Draws" (IDs) nas primeiras mesas.

Este sistema automatiza esse processo:
1. Recebe a quantidade de jogadores em cada **bracket de pontuação**.
2. Calcula o **Corte atual** (a pontuação do jogador que ocupa a última vaga neste exato momento).
3. Estima o **Corte projetado**, prevendo matematicamente para onde a nota de corte vai subir à medida que os jogadores do topo pontuam ou empatam.
4. Analisa o cenário de cada grupo, retornando um Status claro e uma análise de Risco (se dependerão de critérios de desempate/resistência ou se avançam limpos).

---

## Entendendo a matemática (Regras do suíço utilizado)

No formato suíço, a pontuação funciona assim:
-   **Vitória:** $3$ pontos
-   **Empate / ID:** $1$ ponto
-   **Derrota:** $0$ pontos

Para as fórmulas abaixo, consideramos:
-   $P_{atual}$ = Pontuação atual do jogador
-   $R$ = Rodadas restantes
-   $P_{max}$ = Pontuação máxima possível ($P_{atual} + R \times 3$)
-   $P_{empate}$ = Pontuação se apenas empatar/ID ($P_{atual} + R \times 1$)
-   $C_{atual}$ = Corte Atual (pontuação atual do 8º ou 4º colocado)
-   $C_{proj}$ = Corte Projetado ($C_{atual} + \lfloor R \times 1.5 \rfloor$)

### Como o Status principal é calculado?

O algoritmo avalia o potencial máximo de cada jogador e o compara com as linhas de corte para definir 4 status base:

1.  **Sem chance (Eliminado):** $P_{max} < C_{atual}$
    A pontuação máxima possível não alcança sequer a nota que o último classificado *já tem hoje*.
2.  **Já classificado:** $P_{atual} \geq C_{proj}$
    A pontuação do jogador já é igual ou superior ao que o corte exigirá no final do torneio.
3.  **Com chance ao empatar (Zona de ID):** $P_{empate} \geq C_{proj}$
    O jogador só precisa de empates para atingir o corte projetado.
4.  **Necessita vencer:** $P_{max} \geq C_{atual}$ (mas não se encaixa nas regras acima)
    O jogador não pode empatar (ou ficará abaixo do corte). É obrigado a jogar e vencer para ter chances.

---

## O que é a Chance de bolha e como é calculada? (Análise de Bolha e Risco)

Em torneios suíços, a **"Bolha"** (Bubble) acontece quando jogadores terminam com a mesma pontuação, disputando as últimas vagas pelos critérios de desempate (ex: *Opponent's Win Percentage - OWP*).

A calculadora traduz a matemática em dicas claras de tomada de decisão para a mesa:

* **Seguro (Vitória) / Bolha (Empate):** O cenário clássico da última rodada. Se o jogador vencer, passa 100% garantido. Se aceitar um ID, cai na nota de corte e dependerá dos seus tiebreakers.
* **Seguro apenas se vencer:** Empatar não serve e joga o participante para fora do corte. Apenas a vitória garante o avanço limpo sem depender de tiebreakers.
* **Bolha apenas se vencer:** O cenário de maior pressão. Empatar elimina o jogador matematicamente. Ele precisa vencer, e *mesmo vencendo*, cairá na bolha disputando os tiebreakers.

### Projetado vs. Atual (Com menor probabilidade)
A calculadora diferencia quem está acompanhando o fluxo do campeonato de quem ficou para trás:
- **(Corte Projetado):** O jogador atinge a pontuação estimada pela calculadora. Depende só dele (e dos desempates normais) para passar.
- **(Apenas Corte Atual):** Um cenário de raríssima probabilidade. O jogador alcança a pontuação do corte *de agora*, mas não atinge a meta final projetada. Ele só passa se ocorrerem empates generalizados nas outras mesas e a nota de corte do torneio "congelar" e parar de subir.

### Top 8 Assimétrico
No modo Assimétrico, o conceito tradicional de "Bolha" não existe. A regra dita que **todos os jogadores que igualarem a pontuação do último classificado (8º) avançam**, independentemente dos critérios de desempate.
A ferramenta adapta sua coluna final dinamicamente para mostrar apenas **"Com Vitória"**, **"Com Empate"** ou **"Depende da Mesa"** (quando alcança o atual, mas não o projetado).

---

## 💻 Como usar (Localmente)

Como este projeto é construído apenas com HTML, CSS (Tailwind via CDN) e JavaScript puro (Vanilla), não há necessidade de instalação, build ou dependências (Node, npm, etc).

1. Clone o repositório:
   ```bash
   git clone [https://github.com/lucasmarbatista/swiss_top_cut_calculator.git](https://github.com/lucasmarbatista/swiss_top_cut_calculator.git)
2. Abra o arquivo `top_cut_calculator.html` em qualquer navegador web.
3. Insira as configurações do seu torneio e adicione os jogadores.
