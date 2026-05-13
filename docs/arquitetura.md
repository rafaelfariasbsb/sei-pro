# Arquitetura da Extensão SEI Pro

## Visão Geral

O SEI Pro é uma **extensão de navegador (Manifest V3)** que injeta scripts JavaScript dentro das páginas do SEI (Sistema Eletrônico de Informações) para adicionar funcionalidades. Não há servidor próprio — toda a lógica roda no navegador do usuário.

```
Navegador
└── Extensão SEI Pro
    ├── Service Worker (background.js)     ← executa em segundo plano
    ├── Content Scripts                    ← injetados nas páginas do SEI
    │   ├── init_all.js                    ← todas as páginas SEI
    │   ├── init.js                        ← página principal (lista de processos)
    │   ├── init_arvore.js                 ← iframe da árvore do processo
    │   ├── init_visualizacao.js           ← iframe de visualização de documento
    │   ├── init_visualizacao_html.js      ← iframe de documento HTML
    │   ├── init_db.js                     ← frame de banco de dados
    │   └── init_pwd.js                    ← página de login
    └── Página de Opções (options.html)    ← configurações da extensão
```

---

## Contextos de Injeção

O SEI usa múltiplos **iframes** em uma única página. A extensão injeta scripts diferentes em cada contexto:

| Content Script | URL alvo (padrão) | Contexto |
|---|---|---|
| `init_all.js` | `*://*.br/sei/*` | Todas as páginas SEI |
| `init.js` | `?acao=procedimento_trabalhar` | Página de trabalho do processo |
| `init_arvore.js` | `?acao=arvore_visualizar` | Iframe da árvore de documentos |
| `init_visualizacao.js` | `?acao=documento_visualizar` | Iframe de visualização |
| `init_visualizacao_html.js` | `?acao=documento_visualizar` (HTML) | Documento HTML inline |
| `init_db.js` | `?acao=usuario_login` e afins | Frames de autenticação |
| `init_pwd.js` | Página de login | Login com preenchimento auto |

---

## Módulos Principais

| Arquivo | Tamanho | Responsabilidade |
|---|---|---|
| `sei-functions-pro.js` | ~732 KB | Funções utilitárias compartilhadas, detecção de versão, seletores DOM |
| `sei-pro.js` | ~198 KB | Features da página principal (lista de processos) |
| `sei-pro-all.js` | ~54 KB | Features presentes em todas as páginas |
| `sei-pro-editor.js` | ~440 KB | Features do editor de documentos |
| `sei-pro-arvore.js` | ~173 KB | Features da árvore do processo |
| `sei-pro-favoritos.js` | ~108 KB | Gestão de processos favoritos |
| `sei-pro-projetos.js` | ~136 KB | Gestão de projetos (Kanban + Gantt) |
| `sei-pro-atividades.js` | ~2.1 MB | Atividades e controle de prazos |
| `sei-pro-ai.js` | ~114 KB | Integração com IA (ChatGPT/OpenAI) |
| `sei-pro-docs-lote.js` | ~67 KB | Operações em lote em documentos |
| `sei-pro-icons.js` | ~28 KB | Utilitários de ícones |
| `sei-pro-prescricoes.js` | ~27 KB | Controle de prescrições/prazos |
| `sei-legis.js` | ~35 KB | Ferramentas de Legística |

---

## Detecção de Versão do SEI

A extensão detecta a versão do SEI em runtime para adaptar seu comportamento:

```javascript
// sei-functions-pro.js
function getSeiVersionPro() {
    // Lê da sessionStorage (cache)
    // Extrai da tag img com title "Sistema Eletrônico de Informações - Versão X.X"
}

var isNewSEI = $('#divInfraSidebarMenu ul#infraMenu').length > 0;
var isSEI_5  = isNewSEI && compareVersionNumbers(getSeiVersionPro(), '5') >= 0;
```

### Flags de versão

| Flag | Significado |
|---|---|
| `isNewSEI` | SEI com sidebar menu (≥ 4.x) |
| `isSEI_5` | SEI versão 5.x ou superior |

---

## Armazenamento

| Mecanismo | Usado para |
|---|---|
| `chrome.storage.local` | Configurações persistentes da extensão |
| `sessionStorage` | Cache de versão do SEI, dados temporários da sessão |
| `localStorage` | Favoritos, histórico, dados de módulos |
| Google Sheets API | Backend remoto opcional para Projetos (configurado pelo usuário) |

---

## Fluxo de Inicialização

```
1. Página SEI carrega
2. manifest.json → browser injeta content scripts conforme URL
3. init_all.js executa:
   - Detecta versão do SEI (isNewSEI, isSEI_5)
   - Carrega sei-pro-all.js (features globais)
4. init.js executa (na página principal):
   - Carrega sei-pro.js, sei-pro-favoritos.js, sei-pro-atividades.js, etc.
5. init_arvore.js executa (dentro do iframe da árvore):
   - Carrega sei-pro-arvore.js
6. Cada módulo:
   - Lê configurações do storage
   - Verifica flags de versão (isSEI_5)
   - Injeta UI e registra event listeners
```

---

## Dependências de Terceiros

Todas as dependências são **vendorizadas** (copiadas em `dist/js/lib/`). Não há gerenciador de pacotes. Ver lista completa em [`docs/desenvolvimento.md`](./desenvolvimento.md#dependências).

---

## Limitações Conhecidas

- **Sem build system** — arquivos JS editados diretamente, sem transpilação ou minificação automática (ver [ADR-001](./adr/001-sem-build-system.md))
- **Sem testes automatizados** — validação feita manualmente em instâncias do SEI
- **Acoplamento ao DOM do SEI** — mudanças de IDs/classes no SEI quebram funcionalidades (ver [`docs/seletores-sei.md`](./seletores-sei.md))
- **Módulos muito grandes** — `sei-pro-atividades.js` com 2.1 MB dificulta manutenção
