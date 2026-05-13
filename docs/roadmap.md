# Roadmap do Projeto

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

---

## Visão de longo prazo

Tornar o SEI Pro a extensão de referência para usuários do SEI, com suporte completo ao SEI 5, base de contribuidores ativa e código sustentável a longo prazo.

---

## Fase 0 — Fundação do Fork ✅ Concluída

**Objetivo:** Estabelecer a infraestrutura do fork comunitário antes de qualquer alteração de código.

| Entrega | Status |
|---|---|
| Fork do repositório original no GitHub | ✅ |
| Configuração de remotes (origin = fork, upstream = original) | ✅ |
| Issue aberta no projeto original (#152) | ✅ |
| README atualizado com aviso de fork e status SEI 5 | ✅ |
| CONTRIBUTING.md | ✅ |
| CODE_OF_CONDUCT.md | ✅ |
| CHANGELOG.md | ✅ |
| SECURITY.md | ✅ |
| Documentação de engenharia e análise de sistema (`docs/`) | ✅ |

---

## Fase 1 — Diagnóstico e Correções Cirúrgicas

**Objetivo:** Identificar exatamente o que está quebrado no SEI 5 e aplicar correções de alto impacto com baixo risco.

**Critério de conclusão:** Funcionalidades críticas (layout, árvore, lista de processos) operando no SEI 5.

| Entrega | Prioridade | Status |
|---|---|---|
| Mapear seletores DOM alterados no SEI 5 (usar fonte `/FontesSEI`) | 🔴 Alta | Pendente |
| Preencher `docs/seletores-sei.md` com mapeamento SEI 4→5 | 🔴 Alta | Pendente |
| Corrigir detecção de versão (`getSeiVersionPro`) para SEI 5 | 🔴 Alta | Pendente |
| Corrigir seletores de iframe (ifrArvore, ifrVisualizacao) | 🔴 Alta | Pendente |
| Corrigir seletores de painéis (divInfraAreaTelaE/D) | 🔴 Alta | Pendente |
| Adaptar detecção de ícones GIF → SVG no SEI 5 | 🟡 Média | Pendente |
| Corrigir seletores de checkboxes (Bootstrap 4 no SEI 5) | 🟡 Média | Pendente |
| Smoke test completo no SEI 5 (Chrome) | 🔴 Alta | Pendente |
| Smoke test completo no SEI 4.1 — validar não-regressão | 🔴 Alta | Pendente |
| Configurar GitHub Actions — release automático (zip) | 🟡 Média | Pendente |
| Publicar primeira release do fork (v1.6.2) | 🔴 Alta | Pendente |

---

## Fase 2 — Compatibilidade do Editor com SEI 5

**Objetivo:** Restaurar o módulo de editor de documentos no SEI 5, que usa CKEditor 5 (diferente do editor anterior).

**Critério de conclusão:** Barra de ferramentas da extensão funcionando no editor do SEI 5; funcionalidades core do editor operando.

| Entrega | Prioridade | Status |
|---|---|---|
| Analisar API do CKEditor 5 usado pelo SEI 5 | 🔴 Alta | Pendente |
| Adaptar `sei-pro-editor.js` para integrar com CKEditor 5 | 🔴 Alta | Pendente |
| Restaurar: nota de rodapé | 🔴 Alta | Pendente |
| Restaurar: sumário automático | 🔴 Alta | Pendente |
| Restaurar: quebra de página | 🟡 Média | Pendente |
| Restaurar: autossalvamento | 🔴 Alta | Pendente |
| Restaurar: marca d'água de minuta | 🟡 Média | Pendente |
| Restaurar: sigilo/tarjas LGPD | 🔴 Alta | Pendente |
| Restaurar: inserção de dados do processo | 🟡 Média | Pendente |
| Restaurar: hiperlinks (abrir/editar/remover) | 🟡 Média | Pendente |
| Atualizar `docs/matriz-compatibilidade.md` com resultados | 🟡 Média | Pendente |

---

## Fase 3 — Compatibilidade dos Demais Módulos

**Objetivo:** Restaurar os módulos restantes no SEI 5, em ordem de impacto para o usuário.

| Entrega | Prioridade | Status |
|---|---|---|
| Restaurar: Favoritos no SEI 5 | 🔴 Alta | Pendente |
| Restaurar: Controle de Prazos no SEI 5 | 🔴 Alta | Pendente |
| Restaurar: Ações em Lote no SEI 5 | 🔴 Alta | Pendente |
| Restaurar: Agrupar lista de processos no SEI 5 | 🟡 Média | Pendente |
| Restaurar: Menu rápido na árvore no SEI 5 | 🟡 Média | Pendente |
| Restaurar: Estilo Avançado / Modo Noturno no SEI 5 | 🟡 Média | Pendente |
| Restaurar: Histórico de processos | 🟡 Média | Pendente |
| Restaurar: Exportar CSV | 🟢 Baixa | Pendente |
| Restaurar: IA (ChatGPT) no SEI 5 | 🟢 Baixa | Pendente |
| Restaurar: Projetos / Kanban no SEI 5 | 🟢 Baixa | Pendente |
| Atualizar `docs/matriz-compatibilidade.md` completo | 🟡 Média | Pendente |
| Publicar release v2.0.0 (SEI 5 compatível) | 🔴 Alta | Pendente |

---

## Fase 4 — Qualidade e Sustentabilidade

**Objetivo:** Tornar o projeto sustentável a longo prazo com ferramentas modernas.

| Entrega | Prioridade | Status |
|---|---|---|
| Atualizar jQuery para versão atual (3.7+) | 🟡 Média | Pendente |
| Atualizar demais dependências com CVEs conhecidos | 🔴 Alta | Pendente |
| Refatorar `sei-pro-atividades.js` (2.1 MB → módulos menores) | 🟡 Média | Pendente |
| Centralizar todos os seletores DOM em objeto de configuração | 🟡 Média | Pendente |
| Adicionar ESLint com regras básicas | 🟢 Baixa | Pendente |
| Avaliar adoção de sistema de build (Vite) | 🟢 Baixa | Pendente |
| Publicar nas lojas (Chrome Web Store, Firefox Add-ons) | 🟡 Média | Pendente |

---

## Fase 5 — Novas Funcionalidades (Backlog)

> Funcionalidades sugeridas pela comunidade, priorizadas após estabilidade do SEI 5.

| Ideia | Origem |
|---|---|
| Integração com outros provedores de IA (Gemini, Claude) | Comunidade |
| Notificações de novos processos em tempo real | Comunidade |
| Exportar documentos do SEI para Word/PDF | Comunidade |
| Painel de estatísticas de produtividade | Comunidade |
| Suporte a temas customizáveis pelo usuário | Comunidade |
| Integração com sistemas de gestão de tarefas externos | Comunidade |

---

## Critérios de versionamento

| Versão | Critério |
|---|---|
| 1.6.2 | Primeira release do fork — fundação + correções da Fase 1 |
| 1.7.x | Correções incrementais de módulos individuais para SEI 5 |
| 2.0.0 | SEI 5 com ≥ 80% das funcionalidades compatíveis (Fase 3 concluída) |
| 2.x.x | Melhorias de qualidade e novas funcionalidades |

---

## Como contribuir com o roadmap

- Sugira novas funcionalidades abrindo uma [issue](https://github.com/rafaelfariasbsb/sei-pro/issues) com label `enhancement`
- Vote em issues existentes com 👍 para indicar prioridade
- Reporte testes realizados no SEI 5 atualizando a [matriz de compatibilidade](./matriz-compatibilidade.md) via PR
