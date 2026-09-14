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
