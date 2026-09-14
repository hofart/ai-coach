# Design de sistema — ai-coach

> Visão geral de como as peças do projeto se conectam. Arquitetura ainda leve — hoje o "sistema"
> é majoritariamente Claude Code + specs em markdown, sem código próprio ainda.

## Visão geral

```
Usuário (Matheus)
   │  "gera o treino do mês"
   ▼
Skill treinador-bike  ──lê──►  .specs/domain.md          (regras: perfil, rotina, zonas, saúde)
   │                            .specs/methodology.md     (embasamento: Coggan/Allen, Friel, etc.)
   │                            .specs/training-base.md   (biblioteca de padrões de treino)
   │                            .specs/climbs.md          (subidas — sinal de carga)
   │                            .specs/output-format.md   (cadência mensal, formato)
   │                            docs/evolucao-mensal.md   (histórico real de carga)
   │
   ├──delega para──► Skill teste-fthr (quando o pedido é sobre teste/LTHR/zona)
   │
   ▼
Plano mensal (texto estruturado: terça/quinta com blocos, sábado com duração, domingo como log)
   │
   ▼
Usuário sobe cada treino manualmente no Garmin Connect (~8x/mês)
```

### Diagrama (Mermaid)

```mermaid
flowchart TB
    U["Usuário (Matheus)"]
    T["Skill treinador-bike<br/>(motor de geração)"]
    F["Skill teste-fthr<br/>(protocolo/LTHR)"]
    P["Plano mensal<br/>(texto estruturado)"]
    G["Usuário sobe cada treino<br/>manualmente no Garmin Connect"]

    subgraph SPECS[".specs/ + docs/ (regras e histórico)"]
        direction TB
        d1["domain.md"]
        d2["methodology.md"]
        d3["training-base.md"]
        d4["climbs.md"]
        d5["output-format.md"]
        d6["data-model.md"]
        d7["requirements.md"]
        d8["docs/evolucao-mensal.md"]
        d9["docs/teste-fthr.md"]
    end

    U -->|"gera o treino do mês"| T
    T <-->|"delega quando é sobre<br/>teste/LTHR/zona"| F
    T -->|lê| SPECS
    T --> P
    P -->|"upload manual<br/>(~8x/mês)"| G
```

## Componentes atuais

- **`.specs/*.md`** — a fonte da verdade do domínio (regras de negócio). Tratado como dado, não
  como código: a skill lê esses arquivos toda vez que precisa decidir algo.
- **`.claude/skills/treinador-bike/`** — o "motor" de geração de treino. Orquestra as regras de
  `.specs/` e produz o plano mensal.
- **`.claude/skills/teste-fthr/`** — sub-rotina especializada em teste/calibração de LTHR, usada
  pelo treinador-bike quando o assunto é teste de FC limiar.
- **`docs/*.md`** — dados históricos reais (evolução mensal, testes de FTHR) e decisões (ADRs).
  Não são regra, são fato/registro.

## Modelo de dados

Ver `.specs/data-model.md` para as entidades (Atleta, ZonaFC, TesteFTHR, Subida,
TreinoEstruturado, SemanaTreino, PlanoMensal, HistoricoMensal) e como se relacionam.

Hoje esse modelo é conceitual — não há banco de dados nem persistência própria; o "estado" vive
nos arquivos markdown (specs + docs) e na conversa com o Claude Code.

## Evolução futura

- **Interface web**: **decidido** — Nuxt 3 (Vue + TypeScript), sobre Vite/Node (ver `docs/decisions.md`). Vai exibir o plano mensal gerado pela skill; acesso também resolve o caso de uso mobile via navegador.
- **App mobile**: **decidido** — React Native/Expo, publicado via EAS Update (tier grátis) e acessado pelo app Expo Go, sem precisar de App Store/Google Play (ver `docs/decisions.md`).
- **Exportador de arquivo Garmin**: gerar `.fit`/`.zwo` a partir do plano mensal, para reduzir o
  trabalho manual de criar cada treino no Garmin Connect (hoje: 100% manual, ~8x/mês).
- **Persistência**: se/quando sair do modelo "specs em markdown lidas pela skill" para uma
  aplicação de verdade, decidir onde os dados do modelo em `.specs/data-model.md` vão morar
  (arquivo local, banco simples, etc.) — sem necessidade disso enquanto o uso for só via Claude Code.

Essas decisões ficam em aberto de propósito — resolver quando o desenvolvimento começar de fato,
não antes.
