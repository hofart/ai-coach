# Domínio: regras de treino de bike

> Regras genéricas de geração/ajuste de treino — aplicáveis a qualquer atleta usando o ai-coach.
> O dado específico do atleta atual (medidas, rotina semanal, zonas, restrições de saúde, região)
> vive em `docs/atleta.md`, estruturado por `.specs/athlete-profile-schema.md`. Este arquivo nunca
> deve voltar a ganhar um valor específico de pessoa — se uma regra precisar de um número/decisão
> do atleta, ele vem de `docs/atleta.md`, não daqui.

## Perfil do atleta

Ver `docs/atleta.md` (instância atual) e `.specs/athlete-profile-schema.md` (schema e
questionário de onboarding). Se `docs/atleta.md` não existir, ou estiver claramente desatualizado
(o atleta menciona mudança de rotina, peso, objetivo, lesão nova), a skill `treinador-bike` deve
rodar o onboarding descrito no schema antes de gerar ou ajustar qualquer treino.

## Rotina semanal e alternância de carga

A rotina semanal (quais dias, que tipo de sessão, se é estruturado ou não) e qualquer padrão de
alternância de carga entre semanas (ex.: semana A/B) são **dado do atleta**, registrados em
`docs/atleta.md` — não regra fixa deste arquivo. A regra genérica é:

- Dias marcados como **não estruturados** no perfil (`estruturado: false`) recebem apenas
  sugestão de duração/carga a partir do histórico recente — nunca blocos/zonas prescritos.
- Um dia pode ser marcado como **estruturado por sugestão, com opção social** (ex.: o sábado do
  atleta atual) — a skill prescreve um treino completo (zonas/blocos) como padrão, mas o atleta
  pode optar no dia por uma atividade social/em grupo em vez disso; nesse caso, só a duração/carga
  da atividade social importa, a estrutura sugerida não precisa ser seguida.
- Se o perfil tiver um padrão de alternância (`alternancia_carga`), aplicá-lo consistentemente
  semana a semana, ajustando conforme fadiga relatada e as restrições de saúde do perfil.
- Não empilhar dois dias de alta carga seguidos entre os dias estruturados do perfil (ver
  "Restrições de saúde" abaixo).

## Restrições de saúde

Regra genérica: toda `restricao_saude` registrada no perfil do atleta (`docs/atleta.md`) é
histórico/referência para dosar carga futura, não um fato a reanalisar a cada decisão — é o limite
prático de volume/intensidade a respeitar. Ao calcular a carga semanal:

- Somar a carga de **todas** as atividades do perfil (`outras_atividades` + sessões de bike), não
  só a bike isolada.
- Tratar excesso recorrente do atleta sobre o volume proposto (ver histórico em
  `docs/evolucao-mensal.md` ou equivalente) como **sinal de risco**, nunca como "capacidade extra".
- Priorizar folga/recuperação real entre dias de alta carga consecutivos.

## Zonas de treino

Modelo: zonas calculadas a partir da FC limiar (LTHR) do atleta — 5 zonas (Z1 Recuperação, Z2
Endurance/base, Z3 Tempo, Z4 Limiar, Z5 VO2max), mesma lógica usada por Zwift/TrainerRoad. Quando
o atleta tiver medidor de potência, adicionar zonas de %FTP como camada extra, sem descartar FC
(ver RNF05 em `requirements.md`).

As faixas de bpm/%FTP **vigentes** ficam em `docs/atleta.md`, não aqui. Atualizar essa tabela a
cada reteste de LTHR/FTP (skill `teste-fthr`), registrando a mudança com data em
`docs/decisions.md`.

## Contingência climática (dias estruturados de estrada)

Dias estruturados ao ar livre (terça/quinta, e o longão de sábado quando aplicável) dependem do
tempo. Regra genérica — detalhes de fonte/coordenadas/limiar ficam em `.specs/weather.md`
(arquivo regional, como `.specs/climbs.md`):

- Só consultar previsão dentro da **janela de ~7 dias** (fora disso, não é confiável — prescrever
  o treino padrão e sinalizar que será refinado perto da data).
- Se houver risco relevante de chuva na janela provável do treino, apresentar ao atleta as
  alternativas que façam sentido pelo equipamento registrado em `docs/atleta.md`
  (`rolo_indoor`, `acesso_academia`):
  - Nenhuma mudança na estrutura/zonas se a alternativa for **rolo em casa**.
  - Estrutura **adaptada para duração menor (40–60min)** se a alternativa for **academia** (bike
    estacionária) — não é o mesmo estímulo da rua, prioriza o bloco principal.
  - **Treino normal na rua** se houver janela de trégua dentro da disponibilidade do atleta.
- Se o atleta não tiver nenhuma alternativa indoor registrada, não inventar uma opção — sinalizar
  o risco de chuva e perguntar como o atleta prefere lidar com aquele dia.

## Tipos de treino

- Endurance / base
- Intervalado (VO2max, limiar, sprints)
- Força (baixa cadência)
- Recuperação ativa
- Longão
- Teste de FC limiar (LTHR) ou FTP, conforme `metodo_medicao` do perfil — via skill `teste-fthr`.

## Periodização

A decisão de usar (ou não) fases de mesociclo formais (base/build/peak/taper) depende do
`objetivo`/`prova_alvo` do perfil do atleta (`docs/atleta.md`):

- Sem prova-alvo definida: carga estável, variando só pela alternância semanal do perfil (se
  houver) — ver decisão atual registrada em `docs/decisions.md`.
- Com prova-alvo definida: reavaliar a necessidade de periodização formal.

Progressão de volume/intensidade: ajustada pelo reteste de LTHR/FTP a cada 6–8 semanas (skill
`teste-fthr`), não por blocos de periodização formais, a menos que uma prova-alvo justifique.

Semanas de recuperação (deload): sem regra fixa — tratar como exceção pontual conforme
fadiga/histórico, não calendário fixo, a menos que o perfil registre uma preferência diferente.

Cadência de geração: ver `.specs/output-format.md` (hoje mensal para o atleta atual — ajustar se o
perfil mudar essa preferência).

## Regras de geração do treino

Consolidado — já coberto em detalhe nas seções acima, listado aqui como índice:

- **Distribuição do volume semanal**: ver "Rotina semanal e alternância de carga" + `docs/atleta.md`.
- **Ajuste por fadiga/histórico**: ver "Restrições de saúde" e a regra 7 da skill `treinador-bike`
  (ex.: subida puxada no sábado → reduzir intervalado de terça seguinte, quando essa rotina se
  aplicar ao perfil atual).
- **Restrição de dias consecutivos**: não empilhar dois dias de alta carga seguidos entre os dias
  estruturados do perfil.
