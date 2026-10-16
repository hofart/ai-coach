# Perfil do atleta (dados atuais)

> Instância do schema em `.specs/athlete-profile-schema.md`, preenchida via onboarding pela skill
> `treinador-bike`. Única fonte dos dados pessoais do atleta — `.specs/domain.md` não contém mais
> valor específico de pessoa, só regra genérica.

## Identificação e contexto
- Região: Santa Maria/RS (Quarta Colônia) — ver `.specs/climbs.md` para subidas de referência.
- Nível/experiência: iniciante em treino estruturado de estrada (speed) — pedala speed há ~8 meses
  (desde ~jan/2026; ver `docs/evolucao-mensal.md`, que só registra volume real a partir de
  março/2026). Andava de MTB desde 2019, recreativo, sem intensidade/estrutura.
- Objetivo: andar bem, com ênfase em construir base muscular sólida (processo de longo prazo, sem
  prova específica marcada).

## Medidas e medição
- Peso: 71–72 kg — Altura: 1,80 m
- Método de medição: FC (`dispositivo_medicao`: relógio Garmin, sem medidor de potência ainda —
  plano de compra futura; quando chegar, adicionar zonas de potência como camada extra, sem
  descartar FC).
- Dispositivo de entrega do treino (`dispositivo_entrega`): ciclocomputador **iGPSPORT 200S**
  (diferente do aparelho de medição — ver "Equipamento" abaixo e `.specs/output-format.md`).
- Zonas atuais (vigentes desde: a confirmar — ver `docs/teste-fthr.md` para o teste mais recente,
  22/08/2026, que sugeriu uma discrepância ainda não aplicada a esta tabela):

| Zona | Faixa (bpm) | Nome |
|------|-------------|------|
| Z1   | 95 – 124    | Recuperação |
| Z2   | 125 – 136   | Endurance (base) |
| Z3   | 137 – 147   | Tempo |
| Z4   | 148 – 159   | Limiar (Threshold) |
| Z5   | 160 – 177   | VO2max |

> Modelo de 5 zonas por FC (mesma lógica Zwift/TrainerRoad) — ver regra genérica de cálculo em
> `.specs/domain.md`. Atualizar esta tabela a cada reteste de LTHR (skill `teste-fthr`), e registrar
> a mudança em `docs/decisions.md`.

## Equipamento
- Bike de estrada (treinos estruturados de terça/quinta/sábado)
- MTB (pedal leve de domingo)
- Ciclocomputador **iGPSPORT 200S** (GPS, tela no guidão) — adquirido em 16/10/2026. Passa a ser o
  dispositivo usado para **exibir/seguir** o treino estruturado durante a pedalada (zonas, blocos,
  cadência), ver "Entrega do treino" em `docs/decisions.md`.
- **Sensor de cadência**, pareado ao iGPSPORT 200S — permite prescrever e verificar **cadência-alvo**
  nos blocos (já era vocabulário usado em `.specs/training-base.md`, agora com dado real medido).
- **A fonte de FC continua sendo o relógio Garmin** (pulso) — o iGPSPORT 200S não substitui o
  Garmin para medição de frequência cardíaca, só adiciona cadência/navegação/tela no guidão.
- **Rolo de treino indoor (smart trainer): não possui** (em 16/10/2026). Alternativa indoor hoje é
  a **bike estacionária da academia** (tem acesso) — ver contingência climática em
  `.specs/weather.md`/`.specs/domain.md`. Se adquirir um rolo no futuro, atualizar este campo — a
  regra de contingência passa a oferecer a opção "rolo em casa" automaticamente.

## Disponibilidade semanal

| Dia    | Tipo de sessão                     | Estruturado? | Observação |
|--------|--------------------------------------|---------------|------------|
| Terça  | Intervalado (estrada)                | Sim | Fixo toda semana |
| Quinta | Intervalado **ou** Z2 (estrada)       | Sim | Alterna conforme "Alternância de carga" abaixo |
| Sábado | Longão (estrada)                     | Sim (sugestão) | Recebe sugestão de treino estruturado (zona-alvo) por padrão, duração conforme alternância (~2h30 ou ~3–4h); **ir com o pelotão é opção do atleta no dia** — se for com o grupo, vale só a duração/carga, sem seguir a estrutura sugerida. Subidas de referência (sinal de carga, não rota a prescrever): São Martinho da Serra e Silveira Martins (`.specs/climbs.md`) |
| Domingo| MTB com a esposa                     | Não | Pedal social ~2h; só registrar carga, não prescrever |

### Alternância de carga

Padrão observado nos treinos reais da assessoria (ver `.specs/training-base.md`): a carga da
semana é equilibrada alternando entre duas estruturas, para não empilhar carga alta nos 3 dias
estruturados toda semana:

- **Semana tipo A — 2 intervalados**: terça + quinta intervalados → sábado **reduzido** (~2h30).
- **Semana tipo B — 1 intervalado + Z2**: terça intervalado + quinta **Z2** (~2h, endurance) →
  sábado pode ser o **padrão mais longo** (~3–4h).

Alternar A/B semana a semana (ex.: A na semana 1, B na semana 2, A na semana 3...). Regra de
equilíbrio aproximada, não fórmula rígida — ajustar conforme sensação/fadiga relatada e as
restrições de saúde abaixo.

#### Controle de alternância (log — pra skill saber qual é a próxima semana)

| Semana (seg–dom) | Tipo | Decidido em | Motivo |
|---|---|---|---|
| 19–25/10/2026 | B (intervalado + Z2) | 16/10/2026 | Carga mais conservadora — lesão em recidiva ativa, prioriza 1 dia forte só na semana. |

> Semana seguinte (26/10–01/11) seria tipo A por alternância — **reavaliar antes de assumir**,
> conforme resposta do joelho nessa semana (ver "Restrições de saúde").

## Outras atividades (carga cruzada)
- Natação: quarta e sexta (2x/semana), com plano de reduzir para 1x/semana. Contar como parte da
  carga total do atleta, não só a bike, ao dosar a semana.

## Restrições de saúde
- **Síndrome do trato iliotibial (dor lateral no joelho) — condição ATIVA/RECORRENTE, não
  histórico encerrado.** Primeiro episódio em 29/08/2026 (associado a carga total muito alta —
  bike + natação acumulados). Havia sido considerado resolvido, mas o atleta voltou a pedalar sem
  retomar estímulo de forma progressiva e a dor recidivou; fisioterapia em 15/10/2026 confirmou
  inflamação novamente. Avaliação: o corpo ainda está em processo de adaptação ao ciclismo
  estruturado — **é esperado que essa lesão fique recorrente por um tempo**, não é um evento
  isolado a tratar como passado.
  - **Enquanto estiver inflamado/em recidiva**: não rodar o teste de FTHR de esforço máximo (skill
    `teste-fthr`) — esforço de 20min no limiar pode agravar. Manter as zonas atuais (tabela acima)
    até o fisio liberar esforço máximo novamente.
  - Reduzir a tolerância a carga em relação ao período anterior: ao ficar em dúvida entre manter o
    padrão ou reduzir intervalado/longão, **priorizar redução** enquanto a recidiva for recente.
  - Priorizar fortalecimento complementar (glúteo médio, cadeia lateral do quadril —
    `.specs/methodology.md` seção 2) de forma mais consistente, não só como sugestão ocasional.
  - Reavaliar o status desta restrição (ativa vs. resolvida) a cada geração/ajuste de plano — não
    assumir "resolvida" sem confirmação do atleta/fisio, já que já recidivou uma vez.
  - **Rotina de fortalecimento prescrita** (ADR 16/10/2026, `docs/decisions.md`) — 2–3x/semana,
    fora da bike: ponte glútea unilateral (3x12/lado), caminhada lateral com elástico (3x15/lado),
    concha/clamshell com elástico (3x15/lado), prancha lateral (3x20–30s/lado), prancha frontal
    (3x30–45s), bird-dog (3x10/lado). Todo plano gerado deve mencionar essa rotina ou perguntar se
    está sendo feita, enquanto a lesão estiver em recidiva.
- Ver `docs/evolucao-mensal.md`: de março a agosto/2026 o atleta fez consistentemente 127%–267% do
  volume proposto pela assessoria (nunca abaixo de 100%), pico em abril/maio (267%/229%); agosto
  foi o mês de menor volume, coincidindo com o primeiro episódio da lesão. Tratar excesso
  recorrente sobre o proposto como sinal de risco, não como capacidade extra.
- Priorizar folga/recuperação real entre o intervalado de quinta e o longão de sábado.

## Histórico vinculado
- `docs/evolucao-mensal.md` — volume/FC/elevação mensal real.
- `docs/teste-fthr.md` — testes de LTHR.
- `docs/decisions.md` — mudanças de zona/periodização ao longo do tempo.
