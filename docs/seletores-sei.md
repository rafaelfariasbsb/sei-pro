# Registro de Seletores DOM do SEI

Este documento é o **registro central** de todos os seletores DOM do SEI que a extensão utiliza.

Sempre que adicionar ou modificar um seletor, atualize esta tabela.

---

## Como usar este documento

Antes de referenciar um elemento do SEI no código, consulte esta tabela para:
- Saber o seletor correto para cada versão
- Verificar se o seletor já está mapeado
- Identificar quais funcionalidades são afetadas se o seletor mudar

---

## Seletores por categoria

### Iframes principais

| Elemento | SEI 4.0 | SEI 4.1+ | SEI 5.x | Usado em |
|---|---|---|---|---|
| Iframe da árvore do processo | `ifrArvoreHtml` | `ifrVisualizacao` | _a mapear_ | `sei-pro-arvore.js`, `sei-functions-pro.js` |
| Iframe de visualização de doc | `ifrVisualizacao` | `ifrConteudoVisualizacao` | _a mapear_ | `sei-pro.js`, `sei-functions-pro.js` |
| Iframe da árvore (conteúdo) | `ifrArvore` | `ifrArvore` | _a mapear_ | `sei-pro-arvore.js` |

### Layout e painéis

| Elemento | SEI 4.x | SEI 5.x | Usado em |
|---|---|---|---|
| Painel esquerdo (sidebar) | `#divInfraAreaTelaE` | _a mapear_ | `sei-functions-pro.js`, `sei-pro.js` |
| Painel direito (conteúdo) | `#divInfraAreaTelaD` | _a mapear_ | `sei-functions-pro.js` |
| Sidebar menu container | `#divInfraSidebarMenu` | _a mapear_ | `sei-functions-pro.js` |
| Menu principal (ul) | `#infraMenu` | _a mapear_ | `sei-functions-pro.js` |
| Área de tabela | `#divInfraAreaTabela` | _a mapear_ | `sei-pro.js` |

### Árvore do processo

| Elemento | SEI 4.x | SEI 5.x | Usado em |
|---|---|---|---|
| Nó selecionado na árvore | `.infraArvoreNoSelecionado` | _a mapear_ | `sei-pro-arvore.js` |
| Nós da árvore | `.infraArvoreNo` | _a mapear_ | `sei-pro-arvore.js` |
| Ações da árvore | `.infraArvoreAcao` | _a mapear_ | `sei-pro-arvore.js` |

### Editor de documentos

| Elemento | SEI 4.x | SEI 5.x | Usado em |
|---|---|---|---|
| Formulário do editor | `#frmEditor` | `.infra-editor__editor-completo` | `sei-pro-editor.js` |
| Corpo do editor (iframe) | `#ifrEditor` | _a mapear_ | `sei-pro-editor.js` |
| Barra de comandos | `#divBotoesControleProcessos` | _a mapear_ | `sei-pro-editor.js` |

### Navegação e identidade

| Elemento | SEI 4.x | SEI 5.x | Usado em |
|---|---|---|---|
| Link da unidade atual | `#lnkInfraUnidade` | _a mapear_ | `sei-functions-pro.js` |
| Selector de unidades (legado) | `#selInfraUnidades` | _removido_ | `sei-functions-pro.js` |
| Imagem de versão do SEI | `img[title*="Sistema Eletrônico de Informações - Versão"]` | _a verificar_ | `sei-functions-pro.js` |

### Formulários e controles

| Elemento | SEI 4.x | SEI 5.x | Usado em |
|---|---|---|---|
| Checkboxes | `.infraCheckbox` | `.custom-control-input` (BS4) | Múltiplos módulos |
| Checkboxes (alternativo) | `.infraCheckboxInput` | _a mapear_ | Múltiplos módulos |
| Botões de ação | `.infraButton` | _a mapear_ | Múltiplos módulos |

### Indicadores visuais

| Elemento | SEI 4.x | SEI 5.x | Usado em |
|---|---|---|---|
| Ícone de formulário | `formulario1.gif` (por src) | _SVG — a mapear_ | `sei-pro-arvore.js` |
| Ícones de ação | `*.gif` (por src) | _SVG — a mapear_ | `sei-pro-arvore.js` |

---

## Status do mapeamento SEI 5

> Este mapeamento será completado com base no código-fonte do SEI 5 (`/FontesSEI`).

| Categoria | Status |
|---|---|
| Iframes principais | Pendente |
| Layout e painéis | Pendente |
| Árvore do processo | Pendente |
| Editor de documentos | Parcial (editor form mapeado) |
| Navegação | Pendente |
| Formulários | Pendente |
| Indicadores visuais | Pendente |

---

## Como atualizar este documento

1. Identifique o novo seletor no HTML do SEI (usando DevTools ou o código-fonte)
2. Adicione ou atualize a linha correspondente na tabela
3. Atualize o código da extensão usando o padrão:
   ```javascript
   var meuElemento = isSEI_5 ? '#novo-seletor' : '#seletor-antigo';
   ```
4. Faça commit com a mensagem: `docs: atualiza seletor X para SEI 5`
