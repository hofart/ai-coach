# Formato de saída do treino gerado

## Formato de exibição

**Decisão**: texto estruturado, com blocos/intervalos completos (não só resumo) — aquecimento,
bloco principal com zona-alvo, volta à calma, ver exemplo abaixo. Exibido na interface web (Nuxt)
e/ou app mobile (React Native/Expo) já decididos em `docs/decisions.md`. Formato de arquivo
(.fit/.zwo) fica só para a fase de exportação pro Garmin (ver "Onde o resultado é entregue"), não
para a exibição em si.

## Exemplo de treino (esboço)

```
Treino: Intervalado VO2max
Duração total: 1h00
Aquecimento: 15min Z1-Z2
Bloco principal: 5x 3min Z5 / 3min Z1 (recuperação)
Volta à calma: 10min Z1
```

## Cadência de geração

**Geração em lote mensal**, não semanal — para não precisar subir arquivo no Garmin toda semana.
A cada geração, produzir o mês inteiro de terça/quinta (alternando semana tipo A/B — ver
`.specs/domain.md`), já calculado de uma vez. Sábado (duração) também pode ser pré-sugerido por
semana, já que depende só do tipo de semana (A/B), não de contexto do dia. Domingo continua sem
sugestão (só registro posterior). Isso alinha bem com a ideia de mesociclo mensal em "Periodização"
(`domain.md`).

## Onde o resultado é entregue

**Decisão**: em duas fases.

1. **Curto prazo**: o treino é gerado no formato de texto estruturado (como os exemplos de `.specs/training-base.md`), exibido numa interface a construir futuramente.
2. **Depois**: gerar um **arquivo compatível com Garmin** (ex.: `.fit` ou `.zwo`, a definir na implementação) para o atleta **subir manualmente** no Garmin Connect.

**Por que não automático via API**: integrar direto com o Garmin Connect Developer Program (Training API, que permite publicar treinos) exige aprovação como parceiro de negócio — sem preço público, mas também sem self-service, inviável para um projeto pessoal de 1 usuário. Diferente do Strava (que passou a cobrar US$ 11,99/mês, mas pelo menos é self-service — ver `.specs/data-model.md`), o Garmin nem oferece esse caminho de forma simples. Por isso o upload manual do arquivo gerado é o plano realista por enquanto; integração automática fica como possibilidade futura, não prioridade.

**Sobre subir o mês inteiro**: o calendário do Garmin Connect aceita planejar até 30 dias à frente,
mas **não existe bulk upload oficial de treinos estruturados** — importação de arquivo funciona
para atividades já feitas, não para treinos planejados. Cada treino estruturado precisa ser
criado/agendado individualmente na interface do Garmin Connect (existem scripts não-oficiais que
usam endpoints internos não documentados para automatizar isso, mas não são suportados e podem
quebrar a qualquer momento).

**Decisão**: por enquanto, **manual, um por um** (~8 treinos/mês). A ferramenta gera o mês inteiro
de uma vez (texto/lista); o atleta cria cada treino no Garmin Connect manualmente a partir disso.
Automatizar via script não-oficial fica como possibilidade futura, não prioridade agora.
