---
name: teste-fthr
description: Skill especializada em testes de FC limiar (FTHR/LTHR) do atleta — protocolo de teste, cálculo de LTHR, cadência de reteste recomendada, e regras para decidir quando atualizar as zonas de FC vigentes em docs/atleta.md. Usar sempre que o atleta reportar um teste de FTHR/limiar, pedir para calcular LTHR, perguntar quando deve retestar, ou pedir para revisar/atualizar as zonas de FC.
---

# Teste de FTHR (FC limiar)

O atleta não tem medidor de potência (`metodo_medicao: fc` em `docs/atleta.md`), então o
equivalente ao teste de FTP é o **teste de FTHR/LTHR** (FC limiar), usado para calibrar as zonas
de FC vigentes em `docs/atleta.md` (modelo genérico de cálculo descrito em `.specs/domain.md`).

## Antes de rodar o teste — checar se é seguro

Consultar `docs/atleta.md` ("Restrições de saúde"). Se houver uma restrição marcada como **ativa
ou recorrente** (ex.: lesão inflamada no momento), **não aplicar o protocolo de esforço máximo**
abaixo — um teste de 20min no limiar pode agravar a lesão. Sinalizar isso ao atleta e sugerir
esperar liberação (fisio/sensação) antes de retestar. Manter as zonas atuais de `docs/atleta.md`
enquanto o teste estiver suspenso por esse motivo.

## Protocolo padrão de teste

1. Aquecimento leve (~20min) + ativação (ex.: cadência alta por blocos curtos), evitando esforço
   pesado demais antes do teste — fadiga residual distorce o resultado (ver seção "Fatores que
   distorcem o teste").
2. **20 minutos o mais rápido possível, com constância** — não é um sprint, é ritmo forte sustentado.
3. Nos últimos ~3 minutos, esforço máximo.
4. Idealmente em subida ou terreno controlado sem muitas interrupções (ex.: subidas de referência
   em `.specs/climbs.md` — Silveira Martins já foi usada para isso).

## Cálculo de LTHR

```
LTHR ≈ 0,95 × FC média dos 20 minutos
```

(Fonte: protocolo padrão de Joe Friel, amplamente usado como equivalente por FC ao teste de FTP
de Coggan — ver `.specs/methodology.md`.)

## Fatores que distorcem o teste (checar antes de confiar no resultado)

- Aquecimento/ativação forte demais antes do teste (fadiga residual pode reduzir a FC média atingível).
- Detreino recente / volume baixo nas semanas anteriores (ver `docs/evolucao-mensal.md` para contexto de carga recente).
- Terreno muito irregular (paradas, cruzamentos) quebrando a constância do esforço.
- Calor/hidratação/estado de saúde no dia.

## Regra de decisão: atualizar as zonas ou não?

- Se o novo LTHR calculado for **próximo** (poucos bpm) do que as zonas atuais em `docs/atleta.md`
  já implicam, pode atualizar direto.
- Se houver uma **discrepância grande** (ex.: >5-8 bpm) em relação às zonas atuais, **não
  sobrescrever automaticamente**. Sinalizar a discrepância ao atleta, listar possíveis causas
  (seção acima) e sugerir confirmar com um reteste em condições mais controladas antes de mudar
  `docs/atleta.md`.
- Toda atualização de zonas feita deve ser registrada com data em `docs/decisions.md` e aplicada
  na tabela "Zonas atuais" de `docs/atleta.md`.
- Todo teste realizado (mesmo que não gere mudança de zona) deve ser registrado em `docs/teste-fthr.md`, mantendo o histórico de testes (data, protocolo, resultado, LTHR calculado, decisão tomada).

## Cadência de reteste recomendada

**A cada 6–8 semanas**, não 6 meses — período longo demais deixa as zonas desatualizadas por
blocos inteiros de treino. Momento ideal: fim de um bloco/mesociclo, ou logo após uma semana de
recuperação (atleta fresco, não fatigado). Reavaliar a data do próximo teste sempre que uma
mudança relevante de contexto acontecer (lesão, retorno de detreino, mudança de volume).

Fontes:
- [How Often Should You Test FTP? — Roadman Cycling](https://roadmancycling.com/question/how-often-test-ftp)
- [When to Test FTP: How Often and Which Protocol to Use — Roadman Cycling](https://roadmancycling.com/blog/when-to-test-ftp-cycling)
- [Joe Friel's Quick Guide to Setting Zones — TrainingPeaks](https://www.trainingpeaks.com/learn/articles/joe-friel-s-quick-guide-to-setting-zones/)
