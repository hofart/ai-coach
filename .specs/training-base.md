# Base de treinos — biblioteca de padrões (assessoria)

> Exemplos reais de treinos estruturados prescritos pela assessoria do atleta. Base de referência
> da skill do treinador: pode ser **repetida diretamente** ou usada como **padrão/estrutura** para
> mesclar estímulos e propor treinos novos — as duas coisas são válidas (ver `treinador-bike/SKILL.md`).
>
> **Atenção às zonas nos exemplos abaixo**: os valores de bpm citados nos exemplos (ex. "Z2 131–142",
> "Z4 155–166") são de uma **calibração de FC antiga**, anterior às zonas atuais registradas em
> `.specs/domain.md` (Z1 95–124, Z2 125–136, Z3 137–147, Z4 148–159, Z5 160–177 — essas são as
> vigentes). Ao gerar/adaptar um treino com base nesses exemplos, usar sempre os nomes das
> zonas (Z1–Z5) e remapear para os bpm atuais de `domain.md` — nunca copiar os números de bpm
> literais dos exemplos.

## Padrão estrutural comum

- Aquecimento: em torno de **35min em Z2**, quase sempre presente.
- Desaquecimento: em torno de **20min em Z2**, quase sempre presente.
- Corpo principal: bloco(s) de repetição — `Repetir Nx` com um ou mais sub-passos (esforço + descanso,
  cada um com zona-alvo e/ou métrica auxiliar: cadência, distância-alvo, marcha fixa).
- Às vezes fecha com um bloco extra de zona única antes do desaquecimento (ex.: 10–15min em Z3).

## Tipos de estímulo observados (vocabulário para a skill usar/combinar)

1. **Intervalado clássico por zona**: `Nx (tempo em ZX / descanso em Z2)`.
   Ex.: 7x (4min Z3 + 1min Z4 + 3min descanso Z2), aquecimento 35min Z2, desaquecimento 20min Z2.

2. **Ativação/cadência + contra-relógio**: blocos curtos de cadência alta (spin-ups) seguidos de um
   tiro de "contra relógio" (esforço forte e constante) e blocos leves de transição.
   Ex.: 3x (1min cadência 100+ rpm / 1min descanso), 5min leve, 5min contra relógio forte, 10min leve.

3. **Intervalado com distância-alvo controlada**: o esforço tem zona E uma faixa de distância a
   cumprir no tempo (controla se o atleta está "empurrando" o suficiente), às vezes com cadência-alvo.
   Ex.: 6x (6min Z3, completar 3,71–4km, cadência 95rpm / descanso 3'30'' Z2).
   Ex.: 6x (5min Z4, completar 3,09–3,33km, cadência 95rpm / descanso 6min Z2).
   Ex.: 10x (4min Z4, completar 2,47–2,67km / descanso 4min Z2).

4. **Força/torque em marcha fixa (subida real)**: subir em marcha fixa (coroa grande + pinhão fixo,
   sem trocar), buscando desenvolver força e velocidade, postura firme no selim. Feito em subida
   nomeada (ex.: São Marcos).
   Ex.: aquecer 35min Z2 → 5x (2'30'' marcha fixa coroa grande/pinhão 13 / descanso 4min Z2) →
   descanso 10min Z2 → 10min Z3 → desaquecer 20min Z2.

5. **Sprints curtos**: esforços muito curtos e intensos, em pé + sentado, com descanso longo.
   Ex.: 10x (30seg Z5, completar 309–333m — 10" em pé sprintando + 20" sentado fazendo força /
   descanso 1'40'' Z2), seguido de 10min descanso Z2, 15min Z3 (encaixando subida de Itaara se der
   tempo), desaquecer 20min Z2.

6. **Escada de zonas**: sequência sem "descanso" propriamente dito, alternando zonas em blocos.
   Ex.: 8x (2'30'' Z3 + 2'30'' Z4 + 1'30'' Z1).

7. **Repetição de subida real nomeada** (sem estrutura de bpm — é a subida em si que dá o estímulo):
   Ex.: "3x subida dos porcos, indo até a primeira ponte e retorna para subir novamente" (sentido Silveira).

## Subidas nomeadas usadas como estímulo

Ver `.specs/climbs.md` para detalhes — São Martinho da Serra, Silveira Martins, Subida dos Porcos,
São Marcos (subida do ginásio, direção trilhos do Kipper), Itaara (a partir do castelinho).

## Como a skill do treinador deve usar isso

- Pode **usar um treino desta base diretamente (repetir tal como está)**, ou **mesclar estímulos**
  e **propor treinos novos** — ambos válidos, desde que façam sentido com a metodologia registrada
  em `.specs/methodology.md` (Coggan/Allen, Friel, periodização) e respeitem as regras de
  `.specs/domain.md` (rotina fixa, carga cruzada, prevenção de lesão).
- Sempre usar os bpm atuais de `.specs/domain.md` para as zonas, nunca os bpm citados nos exemplos.

## Sábado e domingo — fora do escopo de "treino estruturado"

Diferente de terça/quinta, sábado (longão com pelotão) e domingo (MTB com a esposa) **não seguem
estrutura/zonas prescritas** — ver `.specs/domain.md` (rotina semanal). A skill só sugere duração no
sábado e registra o domingo como carga, sem propor treino em nenhum dos dois. Por isso essas
lacunas foram removidas da lista — não fazem parte do escopo de "treino estruturado" desta base.
