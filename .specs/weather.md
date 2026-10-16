# Previsão do tempo (contingência climática)

> Arquivo regional, como `.specs/climbs.md` — ligado ao campo `regiao` do perfil do atleta
> (`docs/atleta.md`). Outra região exigiria coordenadas/fonte equivalentes, não edição deste
> arquivo.

## Fonte

**Open-Meteo** (https://open-meteo.com) — API pública, gratuita, sem necessidade de chave/cadastro
(consistente com RNF01 — sem dependência de API paga).

Coordenadas de referência (Santa Maria/RS): latitude `-29.6868`, longitude `-53.8149`.

Endpoint (previsão horária, até 7 dias — suficiente pra identificar janelas de chuva/trégua no dia):

```
https://api.open-meteo.com/v1/forecast?latitude=-29.6868&longitude=-53.8149&hourly=precipitation_probability,precipitation&timezone=America%2FSao_Paulo&forecast_days=7
```

## Janela de confiabilidade

Previsão de chuva só é consultada para dias dentro de **~7 dias** da data atual — a API não
projeta mais que isso, e previsão além de poucos dias não é confiável para decisão de
contingência. Fora dessa janela, a skill `treinador-bike` prescreve o treino padrão (assumindo
saída normal na rua) e sinaliza que a decisão climática será refinada perto da data (ex.: quando o
atleta pedir "o treino de terça que vem" dentro da janela).

## Regra de contingência (dia estruturado de estrada — terça/quinta/sábado)

Ao gerar/revisar um treino estruturado de estrada dentro da janela de 7 dias:

1. Consultar `precipitation_probability` por hora, nas horas em que o atleta costuma treinar
   nesse dia.
2. Se houver probabilidade **alta** (≥60%) de chuva cobrindo a maior parte da janela provável do
   treino, apresentar **3 opções** pro atleta decidir no dia (ver `.specs/domain.md` —
   "Contingência climática"), de acordo com o equipamento registrado em `docs/atleta.md`:
   - **Rolo em casa** — só se o atleta tiver um (`docs/atleta.md` → equipamento). Mantém a
     estrutura original (zonas/blocos) tal como planejada, sem ajuste de duração.
   - **Treino normal na rua** — se houver uma janela de trégua (horas com probabilidade baixa)
     dentro da disponibilidade do atleta no dia. Descrever o horário da trégua pro atleta decidir
     em tempo real se vai sair.
   - **Bike na academia** (bike estacionária) — sempre que não houver rolo disponível. Adaptar a
     estrutura para a **duração máxima recomendada nesse ambiente (40–60min)**, não a duração
     padrão do treino de rua — não é o mesmo estímulo, então a sessão é mais curta, priorizando a
     parte principal do treino (zona-alvo) e cortando/encurtando aquecimento e desaquecimento se
     necessário pra caber no tempo.
3. Se a probabilidade for baixa na janela do treino, seguir a prescrição padrão sem apresentar as
   3 opções — só mencionar a previsão se for relevante.
