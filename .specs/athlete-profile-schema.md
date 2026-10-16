# Schema do perfil do atleta

> Define os campos genéricos que compõem o perfil de qualquer atleta usando o ai-coach. Os
> valores reais do atleta atual ficam em `docs/atleta.md`, preenchidos a partir deste schema —
> este arquivo nunca deve conter dado específico de uma pessoa, só a estrutura.

## Campos

### Identificação e contexto
- `regiao` (texto livre) — região onde o atleta treina; referencia as subidas/terreno de `.specs/climbs.md` quando aplicável. **Este é o único campo que permanece ligado a um território fixo** — generalizar o perfil não significa generalizar o terreno; cada região tem seu próprio arquivo de subidas/referências se necessário.
- `nivel_experiencia` (texto livre) — tempo e tipo de prática (ex.: "iniciante em treino estruturado, ~8 meses; MTB recreativo desde 2019").
- `objetivo` (texto livre) — objetivo do atleta; `prova_alvo` (opcional: nome + data), se houver.

### Medidas e medição
- `peso_kg`, `altura_cm` (número)
- `metodo_medicao` (`fc` | `potencia` | `ambos`) — qual grandeza é usada para prescrever treino.
- `dispositivo_medicao` (texto livre) — aparelho que mede a grandeza acima (ex.: relógio Garmin,
  cinta de FC, medidor de potência de marca X).
- `dispositivo_entrega` (texto livre) — aparelho usado para **seguir/exibir** o treino na bike
  durante a pedalada (ex.: ciclocomputador iGPSPORT, relógio Garmin, celular). Pode ser o mesmo que
  `dispositivo_medicao` ou um aparelho diferente (ex.: hoje o atleta mede FC no relógio Garmin mas
  segue o treino no ciclocomputador iGPSPORT 200S — são aparelhos diferentes). Define qual
  documento de API/sincronização em `.specs/data-model.md`/`.specs/output-format.md` se aplica.
- `zonas_atuais` (tabela Z1–Z5, bpm e/ou %FTP) — vigentes a partir de `calibrado_em` (data) e `calibrado_por` (referência ao teste em `docs/teste-fthr.md` ou equivalente de potência).

### Equipamento
- `equipamentos` (lista) — ex.: `bike_estrada`, `mtb`, outros.
- `rolo_indoor` (booleano) — se o atleta tem rolo de treino (smart trainer) em casa. Usado na
  regra de contingência climática (`.specs/weather.md`/`.specs/domain.md`): se `true`, a opção
  "rolo em casa" mantém a estrutura original do treino; se `false`, a alternativa indoor depende
  de `acesso_academia`.
- `acesso_academia` (booleano) — se o atleta tem acesso a bike estacionária em academia. Usado na
  mesma regra de contingência: se `true` e `rolo_indoor` for `false`, a alternativa indoor é a
  academia, com duração adaptada (40–60min). Se ambos forem `false`, não há alternativa indoor —
  a skill deve sinalizar isso em dia de chuva em vez de sugerir uma opção que não existe.

### Disponibilidade semanal
- `rotina_semanal` (lista de `{dia, tipo_sessao, estruturado: bool, observacao}`) —
  `tipo_sessao` é texto livre (ex.: "intervalado", "longão", "social/MTB", "descanso");
  `estruturado: false` significa que a skill só sugere duração/carga, nunca blocos/zonas
  (equivalente à regra atual do sábado/domingo do atleta atual).
- `alternancia_carga` (texto livre, opcional) — descreve um padrão de rotação de carga entre
  semanas, se o atleta tiver um (ex.: semana A/B do atleta atual). Não é obrigatório — nem todo
  atleta precisa de alternância.

### Carga cruzada e saúde
- `outras_atividades` (lista de `{atividade, frequencia, observacao}`) — ex.: natação 2x/semana.
  Entra no cálculo de carga total, não só bike.
- `restricoes_saude` (lista de `{condicao, data_ocorrencia, observacao, impacto_na_carga}`) —
  histórico de lesão/condição relevante para dosar carga.

### Histórico vinculado
- `historico_mensal` → `docs/evolucao-mensal.md`
- `historico_fthr` → `docs/teste-fthr.md`
- `decisoes` → `docs/decisions.md`

## Onboarding — questionário de cadastro/atualização

A skill `treinador-bike` usa este roteiro quando `docs/atleta.md` não existe ainda, ou quando o
atleta pede para atualizar o perfil. Uma pergunta por vez, na ordem abaixo, pulando o que já está
registrado e correto:

1. Região onde treina (para aplicar `.specs/climbs.md` ou identificar que precisa de um arquivo de
   terreno equivalente para outra região).
2. Nível/experiência — há quanto tempo treina de forma estruturada, histórico de outras
   modalidades.
3. Objetivo — "andar bem"/base, prova específica, etc.
4. Peso e altura.
5. Método de medição disponível (FC, potência ou ambos) e **em qual aparelho** (`dispositivo_medicao`
   — ex.: relógio, cinta, medidor de potência) — se FC, quando foi a última calibração de LTHR (ou
   se precisa rodar a skill `teste-fthr` agora); se potência, FTP atual.
5b. **Qual aparelho o atleta usa para seguir/exibir o treino na bike** (`dispositivo_entrega` — ex.:
   ciclocomputador, relógio, celular, nenhum ainda). Pode ser diferente do aparelho de medição (ex.:
   mede FC no relógio, mas segue o treino num ciclocomputador). Essa resposta decide qual caminho de
   entrega/sincronização documentar (ver `.specs/data-model.md`/`.specs/output-format.md`) — não
   assumir um fabricante específico sem perguntar.
6. Equipamento adicional, **incluindo alternativa indoor pra dia de chuva**: tem rolo de treino
   (smart trainer) em casa? Tem acesso a bike estacionária em academia? (monta `rolo_indoor` e
   `acesso_academia` — necessário pra regra de contingência climática funcionar; se o atleta não
   tiver nenhuma das duas, registrar isso também, para a skill não sugerir uma opção inexistente).
7. Dias disponíveis na semana e o que cada dia costuma ser (estruturado vs. social/livre vs.
   descanso) — monta `rotina_semanal`.
8. Existe algum padrão de alternância de carga entre semanas, ou cada semana é tratada igual?
9. Outras atividades físicas regulares (natação, corrida, etc.) e frequência.
10. Restrições de saúde relevantes (lesões, condições) que afetem a carga.

Ao final, escrever/atualizar `docs/atleta.md` seguindo esta estrutura e confirmar com o atleta
antes de gerar o primeiro plano. Reabrir este roteiro (só os campos relevantes) sempre que o
atleta relatar uma mudança — peso, disponibilidade, lesão nova, troca de objetivo.
