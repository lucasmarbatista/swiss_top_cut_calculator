# Swiss Top Cut Calculator

> **Web-based calculator to determine Top Cut chances, bubble scenarios and ID (Intentional Draw) math for Swiss tournaments.**

Uma ferramenta web leve e rápida desenvolvida para Juízes e Organizadores de Torneio (TOs). Ela calcula instantaneamente as chances matemáticas dos jogadores alcançarem o Top Cut (Top 4, Top 8 ou Top 8 Assimétrico) durante as rodadas finais do sistema Suíço.

Acesse a calculadora online aqui: [Link](https://lucasmarbatista.github.io/swiss_top_cut_calculator/top_cut_calculator.html) <!-- Lembre-se de colocar o link aqui depois -->

---

## O que o código faz?

Durante as últimas rodadas de um torneio, calcular quem ainda tem chance de classificar e quem já está eliminado pode ser confuso, especialmente com os "Intentional Draws" (IDs) nas primeiras mesas.

Este sistema automatiza esse processo:
1. Recebe a quantidade de jogadores em cada **bracket de pontuação**.
2. Calcula o **Corte Atual** (a pontuação de quem está na 4ª ou 8ª posição neste momento).
3. Estima o **Corte Projetado**, prevendo que a linha de corte suba à medida que os jogadores do topo pontuam ou empatam.
4. Analisa matematicamente o cenário de cada grupo, retornando um Status claro e a chance de dependerem dos critérios de desempate (Bolha).

---

## Entendendo a Matemática (Regras do suíco utilizado)

No formato suíco utilizado, a pontuação funciona assim:
*   **Vitória:** $3$ pontos
*   **Empate / ID:** $1$ ponto
*   **Derrota:** $0$ pontos

Para as fórmulas abaixo, consideramos:
*   $P_{atual}$ = Pontuação atual do jogador
*   $R$ = Rodadas restantes
*   $P_{max}$ = Pontuação máxima possível ($P_{atual} + R \times 3$)
*   $P_{empate}$ = Pontuação se apenas empatar/ID ($P_{atual} + R \times 1$)
*   $C_{atual}$ = Corte Atual (pontuação atual do 8º ou 4º colocado)
*   $C_{proj}$ = Corte Projetado ($C_{atual} + \lfloor R \times 1.5 \rfloor$)

### Como o Status é calculado?

O algoritmo avalia o potencial máximo de cada jogador e o compara com as linhas de corte para definir 4 status possíveis:

1.  **Sem Chance (Eliminado):**
    Ocorre quando a pontuação máxima possível do jogador não alcança sequer a pontuação que o 8º lugar *já tem hoje*.
    **Fórmula:** Se $P_{max} < C_{atual}$

2.  **Já Classificado:**
    Ocorre quando a pontuação atual do jogador já é igual ou superior ao que o corte exigirá no final do torneio. Ele não pode mais ser alcançado por quem está fora do Top.
    **Fórmula:** Se $P_{atual} \geq C_{proj}$

3.  **Pode Empatar (Zona de ID):**
    Ocorre quando o jogador só precisa de empates nas rodadas restantes para bater o corte projetado.
    **Fórmula:** Se $P_{empate} \geq C_{proj}$

4.  **Necessita Vencer:**
    Ocorre com os jogadores no meio da tabela. Eles não podem empatar (ou ficarão abaixo do corte), sendo obrigados a jogar e vencer para ter chances.
    **Fórmula:** Quando o jogador não se encaixa em nenhum dos casos acima, mas seu $P_{max} \geq C_{atual}$

---

## O que é a Chance de Bolha e como é calculada?

Em alguns torneio de formato suíco, a **"Bolha"** acontece quando vários jogadores terminam o Suíço com a mesma pontuação, disputando as últimas vagas do Top Cut. A vaga é então decidida pelos critérios de desempate, principalmente o *Opponent's Win Percentage (OWP)*.

O sistema identifica quem tem chance de cair na bolha cruzando a pontuação limite com o **Corte Projetado**.

*   **Pode Empatar + Bolha:** Se o jogador der um ID e sua pontuação bater *exatamente* no número do corte projetado ($P_{empate} = C_{proj}$), ele tem chance de bolha. Se ele tiver um ponto a mais, passa direto.
*   **Necessita Vencer + Bolha:** Se, ao vencer todas as rodadas restantes, a pontuação máxima do jogador cair na zona de incerteza (exatamente no Corte Projetado ou 1 ponto para cima/baixo), ele é considerado Bolha.
    **Fórmula:** Se $C_{proj} - 1 \leq P_{max} \leq C_{proj} + 1$
*   *(Exceção do Assimétrico):* No Top 8 Assimétrico, quem empata em pontos sempre passa. Logo, qualquer jogador que consiga apenas se igualar ao $C_{atual}$ passa a ter "chance de bolha/classificação".

---

## Como usar (Localmente)

Como este projeto é construído apenas com HTML, CSS (Tailwind) e JavaScript "Vanilla", não há necessidade de instalação ou dependências (Node, npm, etc).

1. Clone o repositório:
   ```bash
   git clone https://github.com/lucasmarbatista/swiss_top_cut_calculator.git
   ```
2. Abra o arquivo `top_cut_calculator.html` em qualquer navegador web.
3. Insira as configurações do seu torneio e adicione os jogadores.

---
*Desenvolvido para facilitar a vida da comunidade competitiva.*
