# Teste de FTHR (FC limiar) — registro

> Data do teste: **22/08/2026**.

## 1. Aquecimento / ativação pré-teste

Indo na direção da "roda d'água". Estrutura:

1. 20min leve
2. 3x (1min cadência 100+ rpm / 1min descanso)
3. 5min leve
4. 5min contra relógio — constante e forte
5. 10min leve

**Resultado:**
- Distância: 21,765 km — Tempo: 00:46:02 (2:07 min/km | 28,37 km/h)
- FC Média: 130 bpm — Calorias: 478 — TRIMP: 46,03

## 2. Teste FTHR na Serra de Silveira Martins

Protocolo: 20 minutos o mais rápido possível com constância, últimos 3 min no máximo. Início no pórtico de Silveira, na subida.

**Resultado:**
- Distância: 5,629 km — Tempo: 00:20:01 (3:33 min/km | 16,87 km/h)
- Ganho de elevação: +251 m (consistente com o perfil de subida de `.specs/climbs.md`)
- **FC Média: 157 bpm** — Calorias: 278 — Percepção de esforço: 7 (muito difícil) — TRIMP: 140,12

## Cálculo de LTHR

Fórmula padrão (protocolo de 20min, Joe Friel): **LTHR ≈ 95% × FC média dos 20min**.

```
LTHR ≈ 0,95 × 157 = 149,15 ≈ 149 bpm
```

## Comparação com as zonas atuais (`.specs/domain.md`)

| Zona atual | Faixa (bpm) | Implica LTHR de referência ~ |
|---|---|---|
| Z4 (Limiar) | 148–159 | ~159–160 bpm |
| Z5 (VO2max) | 160–177 | acima de 159–160 bpm |

O novo teste sugere **LTHR ≈ 149 bpm**, cerca de **10 bpm abaixo** do que as zonas atuais implicam.
Isso é uma queda relevante, não um ruído de medição.

> Nota: o teste foi em 22/08/2026, antes da lesão (29/08/2026) — não relacionar um ao outro.

### Possíveis explicações (não excludentes)
- **Fadiga acumulada do aquecimento pré-teste**: a ativação já incluía um esforço forte de 5min (chegando a 147 bpm) antes do teste principal — pode ter deixado o atleta menos fresco do que o ideal para um teste de LTHR.
- **Perfil de subida**: o teste foi feito subindo (+251m em 20min), o que é o uso pretendido (especificidade para o terreno real de treino), mas pode se comportar diferente de um teste em plano.
- **Zonas atuais desatualizadas**: o LTHR de referência das zonas atuais (~159–160) pode simplesmente estar desatualizado — não sabemos quando/como essas zonas foram calibradas originalmente.

### Recomendação
Não sobrescrever as zonas de `.specs/domain.md` automaticamente a partir de um único teste com um resultado tão discrepante. Sugerido:
1. Confirmar a data deste teste.
2. Se possível, repetir o teste em condições mais frescas (sem aquecimento pesado antes) para validar se ~149 bpm se confirma.
3. Se confirmado, atualizar a tabela de zonas em `.specs/domain.md` e registrar a mudança em `docs/decisions.md` com a data.

## Cadência de reteste recomendada

**Não usar 6 em 6 meses.** Prática recomendada (Friel / TrainingPeaks / coaching): **retestar a cada 6–8 semanas**, idealmente:
- No fim de um bloco/mesociclo de treino, ou
- Logo após uma semana de recuperação (atleta fresco — não em fadiga acumulada).

Com o teste em **22/08/2026**, a próxima janela de reteste recomendada é **entre 03/10/2026 e 17/10/2026** (6–8 semanas depois). Ajustar a data exata para coincidir com fim de bloco/semana de recuperação, não só pela contagem de semanas.

Testar mais frequente que isso (ex.: a cada 2–4 semanas) tende a ser prejudicado por fadiga residual e não reflete ganho real. Menos frequente que 8 semanas (e principalmente 6 meses) deixa as zonas desatualizadas por bloco/etapas inteiras de treino.

Fontes:
- [How Often Should You Test FTP? — Roadman Cycling](https://roadmancycling.com/question/how-often-test-ftp)
- [When to Test FTP: How Often and Which Protocol to Use — Roadman Cycling](https://roadmancycling.com/blog/when-to-test-ftp-cycling)
- [Joe Friel's Quick Guide to Setting Zones — TrainingPeaks](https://www.trainingpeaks.com/learn/articles/joe-friel-s-quick-guide-to-setting-zones/)
