# Formato de saída do treino gerado

## Formato de exibição

**Decisão**: texto estruturado, com blocos/intervalos completos (não só resumo) — aquecimento,
bloco principal com zona-alvo, volta à calma, ver exemplo abaixo. Exibido na interface web (Nuxt)
e/ou app mobile (React Native/Expo) já decididos em `docs/decisions.md`. Formato de arquivo fica só
para a fase de exportação pro **iGPSPORT 200S** (ver "Onde o resultado é entregue" — o Garmin
permanece só como fonte de FC, não é o dispositivo de entrega), não para a exibição em si.

## Exemplo de treino (esboço)

```
Treino: Intervalado VO2max
Duração total: 1h00
Aquecimento: 15min Z1-Z2
Bloco principal: 5x 3min Z5 / 3min Z1 (recuperação)
Volta à calma: 10min Z1
```

## Exemplo com contingência climática (dia dentro de ~7 dias, risco de chuva)

Ver regra em `.specs/domain.md` ("Contingência climática") e `.specs/weather.md`. Formato: mesmo
treino-base, com as opções disponíveis conforme equipamento do atleta (`docs/atleta.md`):

```
Treino: Intervalado VO2max — terça, 20/10
Previsão: chuva provável das 17h às 19h (prob. 75%), trégua pela manhã (prob. <20% até 12h).

Opção A — Treino normal na rua (se sair até ~12h, antes da trégua acabar):
Duração total: 1h00
Aquecimento: 15min Z1-Z2
Bloco principal: 5x 3min Z5 / 3min Z1 (recuperação)
Volta à calma: 10min Z1

Opção B — Bike na academia (se a chuva não der trégua; sem rolo em casa — ver docs/atleta.md):
Duração total: 45min (adaptado — sem aquecimento/desaquecimento longos)
Aquecimento: 8min Z1-Z2
Bloco principal: 4x 3min Z5 / 2min Z1 (recuperação)
Volta à calma: 5min Z1
```

## Cadência de geração

**Geração em lote mensal**, não semanal — para não precisar subir arquivo no iGPSPORT toda semana.
A cada geração, produzir o mês inteiro de terça/quinta (alternando semana tipo A/B — ver
`.specs/domain.md`), já calculado de uma vez. Sábado (duração) também pode ser pré-sugerido por
semana, já que depende só do tipo de semana (A/B), não de contexto do dia. Domingo continua sem
sugestão (só registro posterior). Isso alinha bem com a ideia de mesociclo mensal em "Periodização"
(`domain.md`).

## Onde o resultado é entregue

**Decisão (atualizada em 16/10/2026 — ver `docs/decisions.md`)**: em duas fases. O dispositivo
usado para **seguir** o treino na bike passou a ser o **iGPSPORT 200S** (ciclocomputador no
guidão, ver `docs/atleta.md` — equipamento), não mais o relógio Garmin (que continua só como fonte
de FC). A lógica de entrega abaixo é a mesma já decidida pro Garmin, aplicada agora ao iGPSPORT.

1. **Curto prazo**: o treino é gerado no formato de texto estruturado (como os exemplos de `.specs/training-base.md`), exibido numa interface a construir futuramente.
2. **Depois**: gerar um **arquivo compatível com o iGPSPORT 200S** para o atleta **subir manualmente**.

**Por que não automático via API**: o iGPSPORT não oferece uma API pública/self-service para
publicar treinos estruturados no dispositivo — a única via documentada oficialmente é **conectar o
dispositivo via cabo USB-C e copiar o arquivo de treino para a pasta correta** (mesmo princípio do
"upload manual" já decidido pro Garmin: ver `.specs/data-model.md`). O iGPSPORT tem integração
oficial com TrainingPeaks, mas essa integração é documentada no sentido **dispositivo → nuvem**
(enviar atividade feita), não confirmada para o sentido inverso (empurrar treino planejado pro
dispositivo). Existem projetos de comunidade (ex.: relay que sincroniza treinos do Intervals.icu
para o iGPSPORT via engenharia reversa da API) — funcionam, mas não são oficiais/suportados e podem
quebrar a qualquer atualização do app ou da API do fabricante.

**Decisão**: por enquanto, **manual, via USB-C** (~8 treinos/mês, mesma cadência mensal). A
ferramenta gera o mês inteiro de uma vez (texto/lista); o atleta copia cada treino estruturado para
o iGPSPORT manualmente a partir disso. Automatizar via ferramenta de comunidade não-oficial fica
como possibilidade futura, não prioridade agora — mesma postura já adotada pro caminho do Garmin.
