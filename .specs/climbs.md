# Subidas de referência (terreno real de treino)

> Escopo para registrar as serras/subidas usadas no treino do atleta, na região de Santa Maria/RS.
> Dados exatos (distância do trecho de subida, desnível, inclinação média/máxima) ainda **não
> estão confirmados** — o atleta não tem os números precisos. Preencher assim que houver uma
> fonte confiável (GPX/Strava segment do próprio atleta ou do pelotão).

## Subidas usadas

### São Martinho da Serra
- Localização: distrito rural de Santa Maria/RS, região da Quarta Colônia.
- Uso no treino: subida de referência do longão de sábado com o pelotão (ver `domain.md` — rotina semanal, "sugerir altimetria").
- Perfil conhecido (relatado pelo atleta, sem número exato de km/desnível ainda): **inclinação de até ~19%**, estrada de sobe e desce (não é uma subida única e constante) — é um treino puxado, de carga alta.
- Dados de distância/desnível total: **a confirmar**. Possível fonte: segmento no Strava — mas a API do Strava agora é paga (ver `.specs/data-model.md`), então captar isso via API não é prioridade; exportar manualmente (GPX) uma atividade que inclua a subida é a alternativa sem custo.

### Subida dos Porcos
- Sentido Silveira (rumo a Silveira Martins).
- Uso no treino: repetições curtas — ex. "3x subida dos porcos, indo até a primeira ponte e retorna para subir novamente".
- Dados de distância/desnível/inclinação: **a confirmar**.

### São Marcos
- "Subida do ginásio", em direção aos trilhos do Kipper.
- Uso no treino: intervalado em marcha fixa (força/torque) — ex. coroa grande + pinhão fixo, sem trocar marcha, buscando desenvolver velocidade mantendo postura firme no selim.
- Dados de distância/desnível/inclinação: **a confirmar**.

### Itaara (a partir do castelinho)
- Uso no treino: trecho de intensidade Z3 encaixado no final de um treino, quando dá tempo.
- Dados de distância/desnível/inclinação: **a confirmar**.

### Silveira Martins
- Localização: município vizinho a Santa Maria/RS, também na região da Quarta Colônia.
- Uso no treino: subida de referência do longão de sábado com o pelotão.
- Dados de distância/desnível/inclinação: **a confirmar**.
- Referência aproximada encontrada (rota completa Santa Maria → Silveira Martins, não apenas o trecho de subida): ~39,5 km com ~480 m de ganho de elevação. Não confundir com o trecho isolado de subida — precisa validação.

## O que falta para fechar o escopo

- [ ] Distância exata de cada subida (ponto de início ao topo)
- [ ] Desnível (ganho de elevação) de cada subida
- [ ] Inclinação média e inclinação máxima
- [ ] Tempo/FC média de referência do atleta nessas subidas (histórico, se disponível)
- [ ] Se as duas subidas são feitas na mesma saída (combinadas) ou em sábados alternados

Fonte sugerida para preencher: exportar o GPX/arquivo do Garmin de uma saída que inclua a subida,
ou o segmento do Strava (se o pelotão usa Strava para essas subidas).

## Como isso entra na regra de treino

**Não é para prescrever rota** (ex.: "faça X km até tal lugar") — a skill não deve indicar
distância/destino específico. O uso real é como **sinal de carga para ajustar os dias seguintes**:

- Se o atleta relatar que o longão de sábado foi em São Martinho (ou outra subida puxada como
  Silveira Martins), tratar esse sábado como um dia de **carga alta confirmada** (pela inclinação
  de ~19% e o perfil de sobe-e-desce), mesmo sem os números exatos de km/desnível.
- Nesse caso, o intervalado de terça seguinte deve ser ajustado para baixo — ex.: um intervalado
  mais tranquilo/curto, priorizando recuperação, em vez do intervalado padrão — para não empilhar
  carga alta em cima de carga alta (reforça a regra de prevenção de lesão em `domain.md`).
- Se o sábado foi um pedal mais leve/plano, o intervalado de terça pode seguir o padrão normal.

Ou seja: São Martinho e Silveira Martins funcionam como **classificadores qualitativos de carga**
do longão de sábado ("foi puxado" vs. "foi tranquilo"), que alimentam a decisão de intensidade da
terça seguinte — não como destino a ser prescrito.
