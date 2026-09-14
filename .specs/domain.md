# Domínio: regras de treino de bike

## Perfil do atleta

- Medição atual: **relógio Garmin com monitor de frequência cardíaca** — sem medidor de potência ainda (plano de compra futura). Todo o treino, por ora, deve ser prescrito e avaliado por FC (zonas da tabela abaixo), não por watts. Quando o medidor de potência chegar, adicionar zonas de potência (% FTP) como camada extra, sem descartar as zonas de FC.
- Peso: **71–72 kg** — Altura: **1,80 m**
- Nível/experiência: **iniciante em treino estruturado de estrada (speed)** — pedala speed há ~8 meses (desde ~jan/2026; o histórico em `docs/evolucao-mensal.md` só começa a registrar volume real a partir de março/2026, os primeiros ~2 meses provavelmente foram adaptação/sem plano formal ainda). Andava de **MTB desde 2019**, mas de forma recreativa, sem intensidade/estrutura — a base de ciclismo "sério" é recente.
- Objetivo: **andar bem**, com ênfase específica em **construir uma base muscular sólida** — um processo que o atleta reconhece que leva tempo, não é sobre resultado rápido. Sem prova específica marcada. Isso reforça a decisão de carga estável sem mesociclos formais (ver "Periodização") — o processo é de construção de base de longo prazo, não de pico para evento.
- Disponibilidade semanal: ver "Rotina semanal atual" abaixo
- Equipamento: ver "Rotina semanal atual" abaixo

## Rotina semanal atual (bike)

4x de bike por semana:

| Dia    | Treino                              | Observação |
|--------|--------------------------------------|------------|
| Terça  | Intervalado (estrada)                | Fixo toda semana |
| Quinta | Intervalado **ou** Z2 (estrada)       | Alterna semana a semana — ver "Alternância semanal" abaixo |
| Sábado | Longão com pelotão (estrada)         | **Não prescrever estrutura/zonas** — o atleta sai acompanhado, não segue um plano estruturado nesse dia. Sugerir apenas a **duração** (~2h30 ou ~3–4h, conforme "Alternância semanal" abaixo). Subidas de referência (contexto, não prescrição): São Martinho da Serra e Silveira Martins (ver `.specs/climbs.md`) |
| Domingo| MTB com a esposa                     | **Não prescrever nada.** Pedal social, ~2h — só registrar como carga da semana (duração/FC se disponível), não sugerir treino |

### Alternância semanal (equilíbrio de carga)

Padrão observado nos treinos reais da assessoria (ver `.specs/training-base.md`): a carga da semana
é equilibrada alternando entre duas estruturas, para não empilhar carga alta nos 3 dias
estruturados (terça/quinta/sábado) toda semana:

- **Semana tipo A — 2 intervalados**: terça intervalado + quinta intervalado → longão de sábado
  **reduzido** (~2h30, em vez de 3–4h), pois a semana já teve duas sessões de alta intensidade.
- **Semana tipo B — 1 intervalado + Z2**: terça intervalado + quinta **Z2 (~2h, endurance)** →
  longão de sábado pode ser o **padrão mais longo** (~3–4h), já que a quinta foi mais leve.

Alternar A/B semana a semana (ex.: A na semana 1, B na semana 2, A na semana 3...). Isso é uma
regra de equilíbrio aproximada, não uma fórmula rígida — ajustar conforme sensação/fadiga
relatada e a restrição de saúde (síndrome do trato iliotibial) acima.

Equipamento: bike de estrada é a usada nos treinos estruturados (terça/quinta/sábado); MTB é só para o pedal leve de domingo.

Além da bike, o atleta faz **natação** às quartas e sextas (2x/semana), com plano de reduzir para 1x/semana.

## Restrições de saúde

- Histórico de lesão: **síndrome do trato iliotibial** (dor lateral no joelho), ocorrida em **29/08/2026**, associada a carga de treino total muito alta (bike + natação acumulados). Registrado como histórico/referência para dosar carga futura — não é um fato para reanalisar a cada decisão, só o limite prático de volume a respeitar.
- Evidência real: ver `docs/evolucao-mensal.md` — de março a agosto/2026 o atleta fez consistentemente **127% a 267% do volume proposto** pela assessoria (nunca abaixo de 100%), com pico em abril/maio (267%/229%). Agosto foi o mês de menor volume da série, coincidindo com o período da lesão. Ao gerar/ajustar treinos, tratar excesso recorrente sobre o proposto como sinal de risco, não como "capacidade extra" do atleta.
- Regra: ao calcular a carga semanal, considerar o volume de natação como parte da carga total do atleta, não apenas o volume de bike — evitar recomendar aumento de intensidade/volume na bike em semanas de carga cruzada alta.
- Priorizar dias de folga/recuperação real entre o treino intervalado de quinta e o longão de sábado.

## Zonas de treino

Baseadas em frequência cardíaca (bpm), a partir da FC limiar (LTHR) atual do atleta.

| Zona | Faixa (bpm) | Nome |
|------|-------------|------|
| Z1   | 95 – 124    | Recuperação |
| Z2   | 125 – 136   | Endurance (base) |
| Z3   | 137 – 147   | Tempo |
| Z4   | 148 – 159   | Limiar (Threshold) |
| Z5   | 160 – 177   | VO2max |

> Modelo simplificado de 5 zonas por FC (mesma lógica usada por Zwift/TrainerRoad), baseado na LTHR atual do atleta.
> Atualizar esta tabela sempre que a LTHR mudar (reteste). Registrar a data da última atualização em `docs/decisions.md`, para saber quando as zonas ficaram desatualizadas.

## Tipos de treino

- Endurance / base
- Intervalado (VO2max, limiar, sprints)
- Força (baixa cadência)
- Recuperação ativa
- Longão
- Teste de FC limiar (LTHR) — substitui o teste de FTP enquanto não há medidor de potência

## Periodização

**Decisão**: sem fases de mesociclo formais (base/build/peak/taper) — não há prova/evento-alvo
(objetivo é "andar bem", ver Perfil do atleta). Carga mantida relativamente estável ao longo do
ano, variando apenas pela alternância semanal A/B (ver "Alternância semanal"). Reavaliar essa
decisão se um objetivo/prova específica surgir no futuro.

- Progressão de volume/intensidade: ajustada pelo reteste de LTHR a cada 6–8 semanas (ver skill `teste-fthr`), não por blocos de periodização formais.
- Semanas de recuperação (deload): não há regra fixa definida ainda — se a carga/fadiga pedir, tratar como exceção pontual (reduzir a semana), não como calendário fixo.
- **Cadência de geração: 1x por mês** (ver `.specs/output-format.md`), não semanal — gerar o mês
  inteiro de uma vez (terça/quinta alternando semana tipo A/B, duração de sábado por semana) para
  evitar reupload semanal no Garmin.

## Regras de geração do treino

Consolidado — já coberto em detalhe nas seções acima, listado aqui como índice:

- **Distribuição do volume semanal**: ver "Rotina semanal atual" e "Alternância semanal" (terça fixa intervalado; quinta e duração de sábado alternam semana A/B).
- **Ajuste por fadiga/histórico**: ver "Restrições de saúde" (não tratar excesso sobre o proposto como capacidade segura) e a regra 7 da skill `treinador-bike` (subida puxada no sábado → reduzir intervalado de terça seguinte).
- **Restrição de dias consecutivos**: não empilhar dois dias de alta carga seguidos (ver "Restrições de saúde").
