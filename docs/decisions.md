# Decisões de arquitetura (ADRs)

Registro de decisões técnicas e de design ao longo do projeto.

## Formato de cada decisão

```
## [data] Título da decisão
Contexto:
Decisão:
Consequências:
```

## [2026-09-02] Stack da interface web

Contexto: primeiro componente de código do projeto (ver `docs/design-system.md`) é uma interface
para visualizar o plano mensal gerado pela skill `treinador-bike`. Precisa ser simples de manter
(RNF03) e acessível de qualquer lugar, incluindo celular durante o treino.

Decisão: **web em Nuxt 3 (Vue + TypeScript)**, que já roda sobre Vite (bundler) e Node.js (server
Nitro) por padrão — não são peças concorrentes, é a stack padrão do framework.

Consequências: a interface web fica pronta para hospedagem simples (Vercel/Netlify têm suporte
nativo a Nuxt). O acesso "de qualquer lugar" via navegador do celular já resolve o caso de uso
mencionado (ver pendência de app abaixo), sem precisar necessariamente de um app nativo.

## [2026-09-02] App mobile — React Native/Expo

Contexto: cogitado acesso via app (Expo/React Native) para consultar o plano "enquanto treina",
inclusive em caráter de emergência (sem depender do PC ligado).

Decisão: **React Native via Expo** (não Kotlin nativo). Publicação via **EAS Update** (tier
grátis: até 1.000 usuários ativos/mês, 100GB de banda, 15 builds/mês por plataforma — muito acima
do necessário para 1 usuário pessoal), acessado a qualquer momento pelo app **Expo Go** (grátis)
através de um link fixo do projeto. Não requer publicação na App Store/Google Play (que tem custo
e não é necessária para uso pessoal) nem o PC ligado — hospedado pela própria Expo.

Consequências: acesso "de emergência" ao plano fica resolvido de duas formas redundantes e
gratuitas: via Expo Go (app) ou via navegador direto na web Nuxt hospedada (ver decisão acima) —
essa segunda nem depende do app estar instalado.

Consequências: revisitar esta decisão quando a interface web estiver funcional — pode ser que o
navegador mobile já seja suficiente e o app nem seja necessário.

## [2026-09-02] Web roda como SPA (`ssr: false`)

Contexto: ao montar o scaffold do Nuxt (Nuxt 4.5.2 + Nuxt UI 4.11.0), o componente `UApp`/
`ConfigProvider` do Nuxt UI gera um erro de SSR ("[Vue warn]: SSR-optimized slot function
detected..." seguido de `NUXT_E4011` e tela em branco no cliente, com `Cannot read properties of
null (reading 'ce')` no hydrate). Reproduzido mesmo com `app.vue` mínimo (`<UApp><NuxtPage /></UApp>`),
com Nuxt e Nuxt UI já nas versões mais recentes — não é bug do código do projeto, é incompatibilidade
entre essas duas versões específicas.

Decisão: **`ssr: false` em `nuxt.config.ts`** — o app roda como SPA (client-only). Aceitável porque
este é um painel pessoal de 1 usuário, sem necessidade de SEO/indexação (RF-WEB, requisitos da web).

Consequências: sem renderização no servidor — o HTML inicial vem vazio, conteúdo só aparece após o
JS carregar no navegador. Reavaliar se algum dia SEO/compartilhamento público importar.

## [2026-10-16] Equipamento novo (iGPSPORT 200S + sensor de cadência) e dispositivo de entrega do treino

Contexto: o atleta adquiriu um ciclocomputador **iGPSPORT 200S** e um **sensor de cadência**.
Passou a ser necessário decidir (a) se isso muda a fonte de FC e (b) qual dispositivo o atleta usa
para seguir o treino estruturado na bike, já que antes a decisão de entrega (`output-format.md`)
era pensada para o Garmin Connect.

Decisão:
- **FC continua vindo do relógio Garmin** — o iGPSPORT 200S não substitui essa fonte, só adiciona
  cadência e navegação/tela no guidão.
- **Cadência passa a ser métrica real disponível** para prescrição (antes só existia como
  vocabulário nos exemplos antigos de `.specs/training-base.md`).
- **O iGPSPORT 200S passa a ser o dispositivo usado para seguir o treino** durante a pedalada, no
  lugar do Garmin. Pesquisa (ver `.specs/output-format.md`, `.specs/data-model.md`) confirmou que o
  iGPSPORT não tem API pública/self-service para publicar treino estruturado — caminho oficial é
  upload manual via USB-C. Existem ferramentas de comunidade (engenharia reversa da API) para
  sincronizar com Intervals.icu, mas não são suportadas. Mesma lógica de decisão já usada pro
  Garmin (upload manual por enquanto, automação não-oficial fica como possibilidade futura).

Consequências: `docs/atleta.md` (equipamento), `.specs/output-format.md` ("Onde o resultado é
entregue") e `.specs/data-model.md` ("Fonte dos dados") atualizados para refletir o iGPSPORT como
dispositivo de entrega, mantendo o Garmin só como fonte de FC.

## [2026-10-16] Síndrome do trato iliotibial reclassificada como condição ativa/recorrente

Contexto: a lesão (primeiro episódio 29/08/2026) havia sido tratada em `docs/atleta.md` como
histórico encerrado. O atleta voltou a pedalar sem retomar o estímulo de forma progressiva, a dor
recidivou, e a fisioterapia (15/10/2026) confirmou inflamação novamente.

Decisão: tratar a lesão como **ativa/recorrente**, não mais como fato histórico fechado — o corpo
está em processo de adaptação ao ciclismo estruturado, e reincidência é esperada por um tempo.
Enquanto estiver nessa fase:
- Suspender o teste de FTHR de esforço máximo (skill `teste-fthr`) até liberação.
- Priorizar redução de carga em caso de dúvida, e fortalecimento complementar sempre (não só
  ocasionalmente).
- Reavaliar o status (ativa vs. resolvida) a cada geração/ajuste de plano, não assumir resolvida
  sem confirmação.

Consequências: `docs/atleta.md` ("Restrições de saúde"), `.claude/skills/treinador-bike/SKILL.md`
(regra 4) e `.claude/skills/teste-fthr/SKILL.md` (nova seção "antes de rodar o teste") atualizados.

## [2026-10-16] Automação da entrega pro iGPSPORT avaliada e postergada

Contexto: após a decisão anterior (entrega manual via USB-C pro iGPSPORT 200S), avaliamos
automatizar via **Intervals.icu + relay comunitário self-hosted** (`igpsport-intervals-relay`,
Docker) — a iGPSPORT está desenvolvendo integração oficial com o Intervals.icu desde maio/2026,
mas sem data de lançamento confirmada até agora.

Decisão: **não automatizar agora** — continuar com upload manual via USB-C
(`.specs/output-format.md`). Dois motivos concretos:
- **Sem infraestrutura sempre ligada**: o relay precisa rodar continuamente (Docker em servidor
  24/7); hoje não há PC/Raspberry Pi/NAS dedicado para isso — só geração local, sob demanda.
  Infra gratuita (ex. Supabase) é uma possibilidade considerada para o futuro, mas para a camada de
  persistência da aplicação web/mobile (ver `docs/design-system.md` — "Evolução futura"), não
  especificamente para rodar esse relay.
- **Risco/custo do relay não-oficial**: exigiria salvar a senha da conta iGPSPORT Cloud num
  arquivo de configuração de uma ferramenta de terceiro (engenharia reversa da API), o que vai
  contra a postura de privacidade do projeto (RNF06) — preferível esperar a integração oficial.

Gatilho para revisitar: quando a iGPSPORT lançar a integração nativa com o Intervals.icu
(acompanhar o fórum/changelog da iGPSPORT), reavaliar a automação sem precisar do relay não-oficial
nem de credencial salva em terceiro.

## [2026-10-16] Contingência climática (rolo/academia/rua) via Open-Meteo

Contexto: dias estruturados de estrada (terça/quinta/sábado) dependem do tempo; o atleta pediu
para considerar chuva na prescrição e oferecer opções (rolo em casa, treino normal, bike na
academia), já que hoje não tem rolo — só acesso à academia.

Decisão: usar **Open-Meteo** (API pública, gratuita, sem chave) como fonte de previsão, dentro de
uma janela de confiabilidade de ~7 dias. Regra e coordenadas em `.specs/weather.md` (arquivo
regional, como `climbs.md`); regra de decisão genérica em `.specs/domain.md` ("Contingência
climática"). O onboarding do perfil (`.specs/athlete-profile-schema.md`) ganhou os campos
`rolo_indoor`/`acesso_academia`, para que a regra funcione para qualquer atleta, não só o atual.

Consequências: `docs/atleta.md` registra que o atleta não tem rolo hoje (só academia, 40–60min de
sessão adaptada); `.claude/skills/treinador-bike/SKILL.md` ganhou o passo de consultar o tempo
antes de prescrever dias dentro da janela de 7 dias; `.specs/output-format.md` ganhou exemplo do
formato com as opções climáticas; `RF12` registrado em `requirements.md`.

## [2026-10-16] Rotina de fortalecimento/core prescrita (prevenção de recidiva)

Contexto: a síndrome do trato iliotibial está em recidiva ativa (ver ADR de 16/10/2026 acima). A
regra 4 da skill `treinador-bike` já pedia "sugerir fortalecimento complementar sempre", mas sem
uma rotina concreta — ficava vago demais pra seguir de fato.

Decisão: prescrever uma rotina fixa de fortalecimento (fora da bike), baseada na lógica de
anatomia aplicada de `.specs/methodology.md` (seção 2 — glúteo médio, cadeia lateral do quadril):

1. Ponte glútea unilateral — 3x12 cada lado
2. Caminhada lateral com elástico (monster walk) — 3x15 passos cada direção
3. Concha (clamshell) com elástico — 3x15 cada lado
4. Prancha lateral — 3x20–30s cada lado
5. Prancha frontal — 3x30–45s
6. Bird-dog — 3x10 cada lado

Frequência: **2–3x/semana**, fora da bike (dias de folga ou à noite em dia de treino leve) —
registrada em `docs/atleta.md` ("Restrições de saúde"), não em `.specs/domain.md` (é rotina
concreta deste atleta, não regra genérica).

Consequências: enquanto a lesão estiver em recidiva, todo plano gerado pela skill deve mencionar
essa rotina (ou perguntar se já está sendo feita), não só "sugerir fortalecimento" de forma vaga.

## [2026-10-16] Sábado deixa de assumir pelotão fixo — passa a ter treino estruturado sugerido

Contexto: a regra original assumia que sábado era sempre longão com pelotão (por isso não se
prescrevia estrutura/zonas, só duração). O atleta pediu para inverter essa suposição: a skill deve
sugerir um treino estruturado completo pro sábado por padrão, e a decisão de ir ou não com o
pelotão fica com o atleta, no dia.

Decisão: sábado passa a ser **"estruturado por sugestão, com opção social"** — a skill prescreve
zonas/blocos como qualquer outro dia estruturado; se o atleta optar pelo pelotão naquele dia, só a
duração/carga da saída entra no cálculo da semana, a estrutura sugerida não precisa ser seguida.
Continua **sem rota/subida nomeada** (regra 7 inalterada) — isso era sobre não prescrever destino,
não sobre não ter estrutura.

Consequências: `docs/atleta.md` (tabela de disponibilidade), `.specs/domain.md` ("Rotina semanal e
alternância de carga") e `.claude/skills/treinador-bike/SKILL.md` (regra 2 e seção "Como montar um
treino") atualizados.

## [2026-09-02] Nuxt UI removido — componentes próprios com Sass

Contexto: mesmo com `ssr: false`, o app continuou instável (erro `ENOENT ... .nuxt/ui.css` do
plugin `@tailwindcss/vite`, telas em branco recorrentes) por causa da dependência do Nuxt UI no
pipeline Tailwind v4 + geração de CSS no `.nuxt`. Removendo os componentes `U*` (`UApp`, `UHeader`,
etc.) da página, o app carregou. Diagnóstico: o problema estava concentrado no Nuxt UI (`UApp`/
`ConfigProvider` e o pipeline Tailwind associado), não no Nuxt em si.

Decisão: **remover `@nuxt/ui`, `tailwindcss`, `@tailwindcss/vite` e os pacotes de ícone
(`@iconify-json/*`)** do projeto. Nenhum componente `U*` é usado. Estilo passa a ser **Sass**
(`sass` como dependência, `app/assets/scss/`, `<style lang="scss" scoped>` por componente,
variáveis em `_variables.scss`). Componentes próprios, sem biblioteca de UI.

Consequências:
- `ssr: false` (decisão anterior) foi mantido mesmo com a causa raiz removida — não foi retestado
  com SSR ligado para evitar mais um ciclo de instabilidade; pode ser revisitado depois com calma.
- Toda a estrutura de pastas/specs (`architecture.md`, `conventions.md`, skill `web-dev`) foi
  atualizada para remover referências a Nuxt UI/Tailwind e refletir Sass.
- Sem design system pronto — visual mais simples/manual por enquanto, aceitável para um painel
  pessoal (RNF03).
