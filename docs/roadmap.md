# Roadmap do Projeto

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 2.0  
**Data:** 2026-08-10 (v1.0 em 2025-05-13)

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

> **Resultado do diagnóstico (10/08/2026):** o mapeamento provou que **o SEI 5 quebrou muito menos do que esta fase supunha**. Cinco das entregas abaixo foram **canceladas por não haver o que corrigir** — e uma delas (checkboxes) teria **quebrado a extensão** se executada como planejado. Detalhes e evidências em [`especificacao-sei5.md`](./especificacao-sei5.md) e [`seletores-sei.md`](./seletores-sei.md).

| Entrega | Prioridade | Status |
|---|---|---|
| Mapear seletores DOM alterados no SEI 5 (fonte 5.0.0 + 5.0.3 + DOM ao vivo) | 🔴 Alta | ✅ Concluído |
| Preencher `docs/seletores-sei.md` com mapeamento SEI 4→5 | 🔴 Alta | ✅ Concluído |
| Corrigir `siglaUnidadeAtual` — ID `#lnkInfraUnidade` duplicado no SEI 5 | 🔴 Alta | ✅ Concluído |
| Corrigir detecção de versão (`getSeiVersionPro`) para SEI 5 | 🔴 Alta | ❌ Cancelado — já funciona; detecta por `title`, que não mudou |
| Corrigir seletores de iframe (ifrArvore, ifrVisualizacao) | 🔴 Alta | ❌ Cancelado — IDs inalterados desde o SEI 4.1 |
| Corrigir seletores de painéis (divInfraAreaTelaE/D) | 🔴 Alta | ❌ Cancelado — IDs preservados; só classes Bootstrap novas |
| Adaptar detecção de ícones GIF → SVG no SEI 5 | 🟡 Média | ❌ Cancelado — extensão já usa `src*=`, que tolera a query string |
| Corrigir seletores de checkboxes (Bootstrap 4 no SEI 5) | 🟡 Média | ❌ Cancelado — **premissa falsa**; renderer BS4 é código morto |
| Smoke test completo no SEI 5 (Chrome) | 🔴 Alta | Pendente |
| Smoke test completo no SEI 4.1 — validar não-regressão | 🔴 Alta | Pendente |
| Configurar GitHub Actions — release automático (zip) | 🟡 Média | Pendente |
| Publicar primeira release do fork (v1.6.2) | 🔴 Alta | Pendente |

### Lições que valem como regra de projeto

1. **Mapear antes de codar funcionou.** Quatro das cinco "correções" previstas eram desnecessárias e uma era ativamente danosa. Executar a Fase 1 como escrita teria introduzido regressões.
2. **Ancorar em ID ou classe `infra*`, nunca em utilitária do Bootstrap.** Entre 5.0.0 e 5.0.3 o `#divInfraAreaTelaD` trocou de `px-3` para `infraAreaTelaDExibeGrande infraAreaTelaDExibePequeno` — o ID não mudou.
3. **Nenhum seletor entra na documentação sem evidência** (arquivo:linha do fonte, ou medição no DevTools).

---

## Fase 2 — Compatibilidade do Editor com SEI 5

**Objetivo:** Restaurar o módulo de editor de documentos no SEI 5, que usa CKEditor 5 (diferente do editor anterior).

**Critério de conclusão:** Barra de ferramentas da extensão funcionando no editor do SEI 5; funcionalidades core do editor operando.

> **Esta é a fase onde está o trabalho real.** O diagnóstico confirmou a ruptura do editor — e mostrou que ela é **maior** do que o previsto: o SEI 5 mantém CK4 **e** CK5 escolhidos por documento (CK5 é opt-in por unidade via `SEI_NOVO_EDITOR_UNIDADES`), e o CK5 é **multi-root sem root `'main'`**, o que faz `getData()` e `inserirHtml()` lançarem exceção se usados da forma óbvia.

| Entrega | Prioridade | Status |
|---|---|---|
| Analisar API do CKEditor 5 usado pelo SEI 5 | 🔴 Alta | ✅ Concluído — API é `window.inicializadorDll` (não `InfraEditor.getInstancia()`) |
| Detecção de editor em runtime (CK4 × CK5), substituindo `isSEI_5` | 🔴 Alta | Pendente |
| Camada de acesso multi-root (get/set/inserir por root) | 🔴 Alta | Pendente |
| Injetar a toolbar da extensão na `.ck-toolbar` | 🔴 Alta | Pendente |
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

> **Escopo provavelmente menor que o previsto.** O mapeamento não encontrou mudança de DOM que afete estes módulos: árvore, tabelas de processo (`#tblProcessosRecebidos`/`Gerados`/`Detalhado`), checkboxes (`chkRecebidosItem`) e paginação (`.infraAreaPaginacao`) estão inalterados. O trabalho aqui tende a ser **retestar e corrigir o que aparecer**, não reescrever. Os itens abaixo devem ser lidos como "**validar no SEI 5**", e só viram trabalho de código se o teste falhar.

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
| **Auditoria dinâmica de tráfego** (DevTools > Network numa sessão real) para confirmar o que sai do navegador | 🔴 Alta | Backlog |
| **ADR — dependência do domínio `seipro.app`** (desativado; ver levantamento abaixo) | 🔴 Alta | Backlog |
| **Legística: desativar ou reimplementar** — hoje envia referências de normas sem opt-in | 🔴 Alta | Backlog |
| Atualizar jQuery para versão atual (3.7+) | 🟡 Média | Pendente |
| Atualizar demais dependências com CVEs conhecidos | 🔴 Alta | Pendente |
| Refatorar `sei-pro-atividades.js` (2.1 MB → módulos menores) | 🟡 Média | Pendente |
| Centralizar todos os seletores DOM em objeto de configuração | 🟡 Média | Pendente |
| Adicionar ESLint com regras básicas | 🟢 Baixa | Pendente |
| Avaliar adoção de sistema de build (Vite) | 🟢 Baixa | Pendente |
| Publicar nas lojas (Chrome Web Store, Firefox Add-ons) | 🟡 Média | Pendente |

---

### Levantamento — dependência do domínio `seipro.app` (10/08/2026)

`https://seipro.app` foi desativado: **todas as rotas fazem 302 para o repositório do GitHub**, e como o GitHub não envia `Access-Control-Allow-Origin`, as chamadas falham por CORS. São 6 referências no código:

| Uso | Onde | Impacto real |
|---|---|---|
| `/servers/` — registro de domínios autorizados + descoberta do backend | `sei-pro-atividades.js:26207` | Bloqueia apenas o **onboarding** do Controle de Prazos |
| `/legis/` — resolve normas citadas no documento | `sei-legis.js:604`, `sei-pro-editor.js:1885` | Legística inoperante |
| `/legis/search.php` — busca de normas no editor | `sei-pro-editor.js:1951` | idem |
| Link de atribuição no mapa | `sei-pro-favoritos.js:1570`, `:1698` | Cosmético |

**O impacto é menor do que aparenta:**
- **Favoritos funcionam offline** — gravados em `localStorage['configDataFavoritesPro']`. O servidor só faz **sincronização entre dispositivos**.
- **Controle de Prazos só depende disso no primeiro uso.** `initPerfilLoginAtiv()` usa o perfil salvo (`configBasePro_atividades` → `URL_API`) e só chama `getServersPro()` quando **não há perfil**. A tela de opções permite configurar **URL do Servidor + Chave de Acesso** manualmente.
- O ramo de auto-configuração via `client_id` já estava **comentado** no fonte.

**Sobre o que trafega** (auditoria estática — confirmar com auditoria dinâmica):
- `/servers/` é `GET` **sem corpo**; o filtro por domínio roda no cliente sobre a lista inteira.
- O backend de Atividades é travado por `if (urlServerAtiv && userHashAtiv != '')` — **nada sai sem servidor e chave configurados**, e o destino é o servidor escolhido pelo administrador.
- ⚠️ **A Legística não tem essa trava:** `getDadosNormas()` faz `POST` das referências de normas (ex.: `lei8666`) sem verificar configuração, e `sei-legis.js` é carregado sempre que o editor abre. Não envia o texto do documento, mas é conteúdo derivado dele indo para terceiro sem opt-in.

**Encaminhamento proposto (a decidir no ADR):** remover a chamada a `/servers/` e documentar a configuração manual; desativar a Legística até que seja reimplementada com trava de configuração explícita.

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
