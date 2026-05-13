# Especificação Técnica — Migração para SEI 5

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13  
**Status:** Em elaboração — seletores marcados como "a confirmar" precisam de validação contra instância SEI 5

---

## 1. Contexto

O SEI 5 introduziu mudanças significativas no frontend em relação ao SEI 4.x. O projeto original iniciou suporte ao SEI 5 (74 referências à flag `isSEI_5` no código) mas foi abandonado antes de concluir a adaptação. Este documento especifica o que mudou, o impacto em cada módulo e o plano de adaptação.

---

## 2. O que mudou no SEI 5

### 2.1 Editor de documentos

| Aspecto | SEI 4.x | SEI 5 |
|---|---|---|
| Engine do editor | Editor proprietário legado | **CKEditor 5** (UMD bundle) |
| Seletor do formulário | `#frmEditor` | `.infra-editor__editor-completo` |
| Corpo editável | `#ifrEditor` (iframe) | `.ck-editor__editable` (div contenteditable) |
| Barra de ferramentas | Customizada (tabela HTML) | Toolbar CKEditor 5 |
| API de acesso ao conteúdo | `ifrEditor.contentDocument.body.innerHTML` | `editor.getData()` / `editor.setData()` |
| Inicialização | Global `CKEDITOR` (CKEditor 4) | Instância via `InfraEditor.getInstancia()` |

**Impacto:** O módulo `sei-pro-editor.js` foi construído sobre o editor legado. A integração com CKEditor 5 requer reescrita dos pontos de acesso ao conteúdo e da barra de ferramentas adicional.

**Nota:** O SEI 5 expõe o editor via objeto `InfraEditor`. Investigar a API pública em `infra/infra_js/editor/inicializador.js` no fonte do SEI.

---

### 2.2 Layout e painéis

| Elemento | SEI 4.x | SEI 5 | Status |
|---|---|---|---|
| Painel esquerdo | `#divInfraAreaTelaE` (float) | A confirmar (Bootstrap flex) | A mapear |
| Painel direito | `#divInfraAreaTelaD` (float) | A confirmar | A mapear |
| Sidebar menu | `#divInfraSidebarMenu` | A confirmar | A confirmar |
| Menu principal (ul) | `ul#infraMenu` | A confirmar | A confirmar |
| Área de tabela | `#divInfraAreaTabela` | A confirmar | A mapear |
| Barra de comandos | `#divInfraBarraComandos` | A confirmar | A mapear |

**Contexto:** O SEI 5 adotou Bootstrap 4/5 com Flexbox, possivelmente alterando ou eliminando os elementos `float`-based do SEI 4.x. Classes `infra*` legadas ainda presentes mas podem coexistir com novas classes Bootstrap.

---

### 2.3 Iframes

| Iframe | SEI 4.0 | SEI 4.1+ | SEI 5 | Impacto |
|---|---|---|---|---|
| Árvore de documentos | `ifrArvoreHtml` | `ifrVisualizacao` | A confirmar | Crítico — 432 refs |
| Visualizador de doc | `ifrVisualizacao` | `ifrConteudoVisualizacao` | A confirmar | Crítico — 390 refs |
| Conteúdo da árvore | `ifrArvore` | `ifrArvore` | A confirmar | Crítico |
| Editor | `ifrEditor` | `ifrEditor` | Removido (CKEditor 5 usa div) | Editor quebrado |

---

### 2.4 Formulários e controles

| Elemento | SEI 4.x | SEI 5 | Impacto |
|---|---|---|---|
| Checkboxes | `.infraCheckbox` | `.custom-control-input` (Bootstrap 4) | Médio — 26 refs |
| Labels de checkbox | `.infraCheckboxLabel` | `.custom-control-label` | Médio |
| Botões | `.infraButton` | `btn btn-*` (Bootstrap) | Médio |
| Inputs | `.infraInput` | Misto (`form-control` + `infra*`) | Baixo |

---

### 2.5 Ícones e imagens

| Elemento | SEI 4.x | SEI 5 | Impacto |
|---|---|---|---|
| Ícone de formulário | `formulario1.gif` (src) | SVG (sistema de ícones SVG) | Médio — detecção por src de GIF quebra |
| Ícones de ação na árvore | `*.gif` (src) | SVG | Médio |
| Imagem de versão | `img[title*="Versão"]` | A confirmar | Crítico — detecção de versão |

---

### 2.6 Detecção de versão

O mecanismo atual lê a versão do SEI a partir de uma tag `<img>` com o título contendo a string `"Sistema Eletrônico de Informações - Versão X.X"`. Se o SEI 5 alterou esta imagem ou removeu este elemento, a detecção falha silenciosamente (retorna `undefined`) e `isSEI_5` nunca é `true`.

**Verificar no fonte do SEI 5:** Buscar em templates PHP a string `"Sistema Eletrônico de Informações - Versão"` para confirmar onde a versão é exibida.

**Estratégia de fallback:**
```javascript
function getSeiVersionPro() {
    // Tentativa 1: img legada
    var imgVersao = $('img[title*="Sistema Eletrônico de Informações - Versão"]');
    if (imgVersao.length) { /* extrai versão */ }

    // Tentativa 2: span ou elemento alternativo no SEI 5 (a identificar)
    var spanVersao = $('#spnSeiVersao, .sei-versao, [data-versao-sei]');
    if (spanVersao.length) { /* extrai versão */ }

    // Tentativa 3: leitura via meta tag (se existir)
    var metaVersao = $('meta[name="sei-version"]');
    if (metaVersao.length) { /* extrai versão */ }

    return null; // versão indeterminada
}
```

---

## 3. Impacto por módulo

### 3.1 `sei-functions-pro.js` — Núcleo compartilhado

**Impacto:** Crítico. Todas as demais funções dependem deste arquivo.

| Função / Variável | Impacto SEI 5 | Ação necessária |
|---|---|---|
| `getSeiVersionPro()` | Pode falhar se img removida | Adicionar fallbacks de detecção |
| `isSEI_5` | Depende de `getSeiVersionPro()` | Validar após correção da detecção |
| `isNewSEI` | Depende de `#divInfraSidebarMenu ul#infraMenu` | Confirmar presença no SEI 5 |
| `siglaUnidadeAtual` | Usa `#lnkInfraUnidade` | Confirmar seletor no SEI 5 |
| `divComandos` | `#divBotoesControleProcessos` | Confirmar seletor no SEI 5 |
| `ifrVisualizacao_` | `ifrConteudoVisualizacao` | Confirmar seletor no SEI 5 |
| `frmEditor` | `.infra-editor__editor-completo` | Já adaptado — validar |

**Padrão a adotar para todos os seletores:**
```javascript
var SEI = {
    painelEsq:    isSEI_5 ? '#novo-painel-esq'  : '#divInfraAreaTelaE',
    painelDir:    isSEI_5 ? '#novo-painel-dir'  : '#divInfraAreaTelaD',
    ifrArvore:    isSEI_5 ? 'novoIfrArvore'     : 'ifrArvore',
    ifrVisual:    isSEI_5 ? 'novoIfrVisual'     : 'ifrConteudoVisualizacao',
    frmEditor:    isSEI_5 ? '.infra-editor__editor-completo' : '#frmEditor',
    checkboxes:   isSEI_5 ? '.custom-control-input' : '.infraCheckbox',
};
```

---

### 3.2 `sei-pro-editor.js` — Editor de documentos

**Impacto:** Crítico. CKEditor 5 tem API completamente diferente do editor legado.

| Funcionalidade | Abordagem SEI 4.x | Adaptação necessária para SEI 5 |
|---|---|---|
| Acessar conteúdo do editor | `ifrEditor.contentDocument.body.innerHTML` | `InfraEditor.getInstancia().editor.getData()` |
| Modificar conteúdo | Manipulação direta do iframe DOM | `editor.setData(html)` ou comandos CKEditor |
| Inserir no cursor | `document.execCommand('insertHTML', ...)` | `editor.model.change(writer => ...)` |
| Obter seleção atual | `ifrEditor.contentDocument.getSelection()` | `editor.model.document.selection` |
| Barra de ferramentas extra | Injeção de HTML na toolbar do editor legado | Plugin CKEditor 5 ou toolbar externa |
| Nota de rodapé | DOM direto no iframe | Reescrever usando API do modelo CKEditor 5 |
| Sumário | DOM direto | Reescrever usando API do modelo |
| Autossalvamento | `setInterval` + leitura do iframe | `setInterval` + `editor.getData()` |

**Investigar:** A API pública do objeto `InfraEditor` no SEI 5 (arquivo `inicializador.js`). Se `InfraEditor.getInstancia()` retorna a instância CKEditor, grande parte da integração fica mais simples.

---

### 3.3 `sei-pro-arvore.js` — Árvore de documentos

**Impacto:** Crítico. Opera dentro do iframe da árvore, que pode ter ID diferente no SEI 5.

| Elemento | SEI 4.x | SEI 5 | Ação |
|---|---|---|---|
| Iframe da árvore | `ifrArvore` | A confirmar | Mapear seletor |
| Nó selecionado | `.infraArvoreNoSelecionado` | A confirmar | Mapear classe |
| Ícones de ação | `a img[src*=".gif"]` | `a svg` ou `a img[src*=".svg"]` | Adaptar seletor |
| Tipo do documento | Detectado via `src` do GIF | Detectado via classe SVG ou atributo | Reescrever detecção |

---

### 3.4 `sei-pro.js` — Página principal (lista de processos)

**Impacto:** Alto. Depende da estrutura da tabela de processos e dos painéis.

| Elemento | Impacto |
|---|---|
| Estrutura da tabela de processos | Pode ter mudado com Bootstrap 4/5 |
| Checkboxes da tabela | `.infraCheckbox` → `.custom-control-input` |
| Paginação | Componente Bootstrap diferente |
| Área de controle | Seletores de painel podem ter mudado |

---

### 3.5 `sei-pro-favoritos.js` — Favoritos

**Impacto:** Médio. Depende da tabela de processos e injeção de UI no painel.

Aguarda mapeamento do painel principal no SEI 5.

---

### 3.6 `sei-pro-atividades.js` — Controle de Prazos

**Impacto:** Médio. A UI do módulo é majoritariamente própria (painel modal). Principal dependência é a leitura de números de processo da tabela.

Aguarda mapeamento da tabela de processos no SEI 5.

---

### 3.7 `sei-pro-ai.js` — Integração com IA

**Impacto:** Baixo no núcleo (chamadas à API OpenAI não mudam). Médio na UI (painel lateral depende do editor).

Aguarda adaptação do editor (Fase 2) antes de retestar.

---

### 3.8 `sei-pro-docs-lote.js` — Ações em Lote

**Impacto:** Médio. Depende de checkboxes e estrutura da árvore de documentos.

Aguarda mapeamento da árvore e checkboxes no SEI 5.

---

### 3.9 `sei-pro-projetos.js` — Projetos / Kanban

**Impacto:** Baixo. UI completamente própria (modal), sem forte dependência do DOM do SEI. Aguarda Fase 3.

---

### 3.10 `sei-legis.js` — Legística

**Impacto:** Médio. Opera no editor de documentos — aguarda adaptação do editor (Fase 2).

---

## 4. Plano de adaptação por prioridade

```
PRIORIDADE CRÍTICA (Fase 1)
├── 1. Corrigir getSeiVersionPro() — adicionar fallbacks
├── 2. Mapear ifrArvore e ifrVisualizacao no SEI 5
├── 3. Mapear divInfraAreaTelaE/D no SEI 5
├── 4. Corrigir checkboxes (.infraCheckbox → .custom-control-input)
└── 5. Smoke test geral

PRIORIDADE ALTA (Fase 2)
├── 6. Adaptar editor para CKEditor 5
│       ├── Investigar API InfraEditor
│       ├── Reescrever acesso ao conteúdo
│       ├── Reescrever inserção no cursor
│       └── Restaurar funcionalidades core do editor
└── 7. Smoke test do editor

PRIORIDADE MÉDIA (Fase 3)
├── 8. Restaurar Favoritos
├── 9. Restaurar Controle de Prazos
├── 10. Restaurar Ações em Lote
├── 11. Restaurar Agrupamento de processos
└── 12. Release v2.0.0
```

---

## 5. Como contribuir com este documento

Este documento é **vivo** — deve ser atualizado conforme o mapeamento avança.

**Ao confirmar um seletor:**
1. Substitua `A confirmar` pelo valor encontrado no SEI 5
2. Atualize também `docs/seletores-sei.md`
3. Commit com: `docs: confirma seletor X no SEI 5`

**Ao concluir a adaptação de um módulo:**
1. Atualize o status na tabela de impacto para `✅ Adaptado`
2. Atualize `docs/matriz-compatibilidade.md`
3. Atualize `CHANGELOG.md`
