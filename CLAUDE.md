# ai-coach

Gerador de treinos de bike (ciclismo).

## Status

Especificação e arquitetura fechadas. Stack decidida (web: Nuxt 3 + Vite + Node/TS; mobile: React
Native/Expo — ver `docs/decisions.md`). Código ainda não iniciado.

## Estrutura do projeto

- `.specs/requirements.md` — requisitos funcionais (RF) e não funcionais (RNF) do sistema.
- `.specs/domain.md` — regras **genéricas** de domínio: como tratar zonas de FC, rotina/alternância de carga, restrições de saúde, tipos de treino, periodização. Nunca contém dado específico de pessoa — isso vive em `docs/atleta.md`.
- `.specs/athlete-profile-schema.md` — schema genérico do perfil do atleta (campos) e o questionário de onboarding/atualização usado pela skill `treinador-bike` para preencher `docs/atleta.md`.
- `.specs/methodology.md` — síntese metodológica (Training and Racing with a Power Meter, Anatomia do Ciclismo, Planeje seu Pedal) que embasa as regras acima.
- `.specs/climbs.md` — subidas/serras reais usadas no treino (São Martinho da Serra, Silveira Martins), específicas da região registrada em `docs/atleta.md`; dados a confirmar. Fica fixo por região — não é generalizado pelo schema do atleta.
- `.specs/weather.md` — fonte (Open-Meteo, grátis/sem chave) e regra de contingência climática (rolo/academia/rua) para dias estruturados de estrada dentro de ~7 dias; arquivo regional como `climbs.md`.
- `.specs/data-model.md` — estrutura de dados (atleta, histórico, plano, treino).
- `.specs/output-format.md` — como o treino gerado deve ser estruturado/exibido.
- `.specs/training-base.md` — base de treinos: biblioteca de padrões de treino da assessoria (tipos de estímulo, estrutura), reutilizável diretamente ou como referência.
- `docs/atleta.md` — **dados reais atuais** do atleta (instância do schema acima): perfil, zonas de FC vigentes, rotina semanal, restrições de saúde. Fonte única desses valores.
- `docs/design-system.md` — design de sistema: como as peças se conectam (skills, specs, docs), modelo de dados, evolução futura.
- `docs/diagrama-arquitetura.excalidraw` — diagrama visual da arquitetura (abrir em excalidraw.com).
- `docs/decisions.md` — decisões de arquitetura (ADRs) e notas de design.
- `docs/evolucao-mensal.md` — histórico mensal real de volume/FC/elevação (assessoria).
- `docs/teste-fthr.md` — histórico de testes de FC limiar (data, protocolo, resultado, LTHR calculado).
- `.claude/skills/treinador-bike/SKILL.md` — skill que age como treinador, usando os arquivos acima (inclui onboarding do perfil quando `docs/atleta.md` não existe/está desatualizado).
- `.claude/skills/teste-fthr/SKILL.md` — skill especializada em testes de FTHR/LTHR (protocolo, cálculo, cadência de reteste); usada pela skill do treinador.
- `web/` — projeto Nuxt (interface web), com suas próprias specs técnicas (`web/.specs/`) e skill (`web/.claude/skills/web-dev/`). Ver `web/CLAUDE.md`.

## Convenções

(a preencher quando o código dos projetos Nuxt/Expo for criado)

## Como rodar / testar

(a preencher quando o código dos projetos Nuxt/Expo for criado)
