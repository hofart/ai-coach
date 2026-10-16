# Modelo de dados

> Entidades derivadas do que já está decidido nas outras specs. Schema concreto (tipos de coluna, etc.) fica pra quando a stack for escolhida — aqui é o modelo conceitual.

## Entidades

### Atleta
- Schema genérico definido em `.specs/athlete-profile-schema.md`; instância com os dados reais do
  atleta atual em `docs/atleta.md` (fonte única — `.specs/domain.md` não guarda mais valor
  específico de pessoa).
- Campos: regiao, nivel_experiencia, objetivo/prova_alvo, peso_kg, altura_cm, metodo_medicao
  (`fc`|`potencia`|`ambos`), zonas_atuais, equipamentos, rotina_semanal, alternancia_carga,
  outras_atividades, restricoes_saude.

### ZonaFC
- zona (Z1–Z5), faixa_min_bpm, faixa_max_bpm, nome
- calculada_a_partir_de → TesteFTHR (referência ao teste que a gerou/confirmou)
- vigente_desde (data)

### TesteFTHR
- data
- protocolo (aquecimento + 20min esforço)
- fc_media_20min
- lthr_calculado (fc_media_20min × 0,95)
- decisão: atualizou zona? (sim/não + motivo)

### Subida
- nome (São Martinho da Serra, Silveira Martins, Subida dos Porcos, São Marcos, Itaara)
- localização
- inclinação_aprox, perfil (sobe-e-desce vs. constante)
- distância/desnível exatos: nulo até serem confirmados
- uso: sinal_de_carga (não é destino a prescrever)

### TreinoEstruturado (terça/quinta)
- dia_semana, tipo (intervalado | Z2)
- estrutura: lista de blocos — cada bloco tem {tipo (aquecer/repetir/pedalar/descansar/desaquecer), duração, zona_alvo opcional, cadência_alvo opcional, distância_alvo opcional}
- duração_total_estimada

### DiaSemLog (sábado/domingo)
- sábado: duração_sugerida (não tem estrutura) — depende do tipo de semana (A/B)
- domingo: duração_reportada (~2h MTB, só log, não sugestão)

### SemanaTreino
- tipo (A = 2 intervalados | B = 1 intervalado + Z2)
- referências: terça (TreinoEstruturado), quinta (TreinoEstruturado), sábado (duração), domingo (log opcional)

### PlanoMensal
- mês/ano
- semanas: lista de SemanaTreino (alternando A/B)
- gerado_em (data de geração — cadência mensal, ver `output-format.md`)

### HistoricoMensal (dados reais da assessoria — já populado em `docs/evolucao-mensal.md`)
- mês, treinos_propostos, treinos_feitos, pct_feitos
- dist_feita_km, dist_media_km, tempo_feito, tempo_medio
- vel_media_kmh, pace_min_km, fc_media, elevação_m, calorias

## Relações

```
Atleta (docs/atleta.md) 1—N TesteFTHR
Atleta 1—N ZonaFC (vigente_desde controla qual é a atual; valores vigentes ficam em docs/atleta.md)
TesteFTHR 1—N ZonaFC (uma calibração pode gerar as 5 zonas)
Atleta 1—N PlanoMensal
PlanoMensal 1—N SemanaTreino
SemanaTreino 1—2 TreinoEstruturado (terça, quinta)
SemanaTreino 1—1 DiaSemLog (sábado)
SemanaTreino 1—1 DiaSemLog (domingo)
Subida N—N TreinoEstruturado (quando um treino referencia uma subida nomeada)
Atleta 1—N HistoricoMensal
```

## Fonte dos dados

- Entrada manual do usuário (colar dados ou exportar GPX/TCX/CSV de uma atividade) — caminho padrão por enquanto.
- Integração com Strava API: **desde 01/06/2026 a Strava API é paga** (US$ 11,99/mês, tier Standard, até 10 usuários) — não é mais gratuita para novos apps. Isso deixa de fazer sentido para um projeto pessoal de 1 usuário; usar exportação manual em vez de integração direta, a menos que o custo mensal seja aceitável.
- Integração com Garmin: **Training API é aprovação de parceiro**, sem self-service — não viável agora.
- Integração com iGPSPORT (dispositivo usado para seguir o treino na bike desde 16/10/2026, ver `docs/atleta.md`): **sem API pública/self-service** para publicar treino estruturado no dispositivo; único caminho oficial é upload manual via USB-C. Projetos de comunidade (engenharia reversa) existem, mas não são suportados. Upload de treino gerado é manual (ver `output-format.md`).
