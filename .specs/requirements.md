# Requisitos

## Requisitos Funcionais (RF)

- **RF01** — Gerar plano de treino mensal: terça e quinta com estrutura completa (blocos, zonas, duração), sábado com duração sugerida (sem estrutura/rota), domingo apenas como registro de carga.
- **RF02** — Alternar automaticamente entre semana tipo A (2 intervalados) e tipo B (1 intervalado + Z2), ajustando a duração do longão de sábado conforme o tipo.
- **RF03** — Prescrever todo treino por zona de frequência cardíaca (Z1–Z5), nunca por potência, enquanto o atleta não tiver medidor de potência.
- **RF04** — Usar a biblioteca de padrões (`training-base.md`) podendo repetir um treino exemplo diretamente ou mesclar estímulos para propor um novo.
- **RF05** — Tratar as subidas nomeadas (`climbs.md`) como sinal qualitativo de carga do sábado, nunca como rota/destino a prescrever.
- **RF06** — Executar e registrar testes de FTHR/LTHR (protocolo, cálculo, decisão de atualizar zona ou não), via skill `teste-fthr`.
- **RF07** — Registrar histórico mensal de evolução real (volume, FC média, elevação, % feito vs. proposto).
- **RF08** — Sinalizar quando o volume real exceder significativamente o proposto, como alerta de risco (não como capacidade extra).
- **RF09** — Nunca prescrever dois dias de alta carga consecutivos sem folga/redução compensatória.
- **RF10** — Gerar a saída em texto estruturado, em lote mensal (não semanal).
- **RF11** *(futuro)* — Exportar o plano gerado em arquivo compatível com o dispositivo usado para seguir o treino na bike (hoje: iGPSPORT 200S, ver `docs/atleta.md`; Garmin permanece só como fonte de FC) para reduzir o trabalho manual de criar cada treino.
- **RF12** — Para dias estruturados de estrada dentro de ~7 dias da data atual, consultar a previsão do tempo da região do atleta (`.specs/weather.md`) e, havendo risco relevante de chuva na janela do treino, oferecer alternativas (rolo em casa e/ou bike na academia, conforme equipamento do atleta em `docs/atleta.md`) além do treino normal na rua — nunca sugerir uma alternativa que o atleta não tem.

## Requisitos Não Funcionais (RNF)

- **RNF01 — Sem dependência de API paga**: funcionar com exportação/entrada manual de dados (Strava e Garmin Training API descartados por custo/inviabilidade — ver `data-model.md`).
- **RNF02 — Specs como fonte única da verdade**: regras de domínio vivem em `.specs/*.md`, versionadas, legíveis por humano e por IA — nenhuma regra de negócio deve viver só "na cabeça" do sistema.
- **RNF03 — Baixo custo de manutenção**: projeto pessoal de 1 usuário; soluções simples (arquivos markdown, geração sob demanda) preferidas sobre infraestrutura pesada.
- **RNF04 — Auditabilidade**: toda mudança relevante (zona de FC, decisão de periodização) deve ficar registrada com data em `docs/decisions.md` ou equivalente, não só aplicada silenciosamente.
- **RNF05 — Extensibilidade**: preparado para adicionar zonas de potência (quando o medidor de potência chegar) sem descartar o que já existe em FC.
- **RNF06 — Privacidade**: dados pessoais de saúde/treino ficam em arquivos locais; nenhuma integração externa deve receber esses dados além do estritamente necessário e explicitamente autorizado.
- **RNF07 — Idioma**: conteúdo gerado em português (PT-BR).
- **RNF08 — Performance**: geração deve ser praticamente instantânea — as specs são pequenas (poucos KB), não há necessidade de processamento pesado ou de subagentes paralelos para isso (ver decisão anterior sobre não usar sub-agentes para "rodar as specs").
- **RNF09 — Usabilidade da saída**: o texto gerado precisa ser claro o suficiente para o atleta transcrever/copiar manualmente para o dispositivo de treino (hoje: iGPSPORT 200S, ver `docs/atleta.md`) sem ambiguidade (nomes de zona, duração, estrutura de blocos bem definidos).
