# Módulo: sei-pro-editor.js

**Tamanho:** ~440 KB  
**Contexto:** Injetado por `init_visualizacao.js` e `init_visualizacao_html.js`  
**Página SEI:** Editor e visualizador de documentos (`?acao=editor_montar`, `?acao=documento_visualizar`)  
**Status SEI 5:** ❌ Quebrado — CKEditor 5 tem API incompatível  
**Arquivo:** `dist/js/sei-pro-editor.js`

---

## Responsabilidade

Adiciona funcionalidades avançadas ao editor de documentos do SEI:
- Barra de ferramentas extra com todas as funcionalidades de edição
- Nota de rodapé, sumário, quebra de página
- Inserção de equações (LaTeX)
- Marca d'água, sigilo LGPD, tarjas pretas
- Autossalvamento
- Ditado por voz (Web Speech API)
- Parágrafos numerados (visualizador)
- Inserção de dados do processo
- Inserção de referências de documentos
- Copiar/colar formatação
- Tabela rápida, estilo de tabela
- Referências internas, numeração de parágrafos

---

## Dependências

- `sei-functions-pro.js` (base)
- CKEditor (legado — via instância global do SEI 4.x)
- MathJax ou similar (para equações)
- `lib/` — sem dependências diretas extras além das globais

---

## Arquitetura interna

```
sei-pro-editor.js
├── Inicialização
│   ├── Detecta versão do SEI (isSEI_5)
│   ├── Localiza o editor (frmEditor / .infra-editor__editor-completo)
│   └── Injeta barra de ferramentas adicional
│
├── Módulo: Barra de ferramentas
│   ├── Criação dos botões
│   ├── Gerenciamento de estado (ativo/inativo)
│   └── Tooltips
│
├── Módulo: Nota de Rodapé
│   ├── Detecta cursor no editor
│   ├── Insere marcação numerada no cursor
│   └── Insere nota no rodapé do documento
│
├── Módulo: Sumário
│   ├── Varre o documento por títulos (H1–H4)
│   ├── Gera índice HTML
│   └── Insere no início do documento
│
├── Módulo: Autossalvamento
│   ├── setInterval a cada N segundos
│   ├── Captura conteúdo do editor
│   ├── Persiste no sessionStorage
│   └── Exibe indicador de tempo
│
├── Módulo: Ditado por Voz
│   ├── Usa Web Speech API (SpeechRecognition)
│   ├── Insere texto reconhecido no cursor
│   └── Indicador de gravação ativo
│
└── Módulo: Parágrafos Numerados
    ├── Opera no modo visualizador (não editor)
    └── Adiciona numeração CSS sem modificar o DOM do documento
```

---

## Integração com o editor do SEI

### SEI 4.x (editor legado)

```javascript
// Acesso ao documento editável
var ifrEditor = document.getElementById('ifrEditor');
var doc = ifrEditor.contentDocument;
var body = doc.body;

// Inserção no cursor
doc.execCommand('insertHTML', false, htmlParaInserir);

// Leitura do conteúdo
var conteudo = body.innerHTML;

// Instância CKEditor 4 (se disponível)
var editor = CKEDITOR.instances['txtDescricao'];
```

### SEI 5 (CKEditor 5) — A IMPLEMENTAR

> ⚠️ **API confirmada no fonte e no DOM (10/08/2026).** Não é `InfraEditor.getInstancia()` — essa era uma hipótese antiga, incorreta. Ver [`../seletores-sei.md`](../seletores-sei.md).

```javascript
// Acesso à instância CKEditor 5 via wrapper do SEI
var editor = inicializadorDll.editores[0];   // window.inicializadorDll

// O editor é MULTI-ROOT e NÃO existe root 'main':
// os roots são um por campo do modelo (txaEditor_200, txaEditor_201, …)
var root = editor.model.document.selection.getFirstPosition().root.rootName;

// Leitura do conteúdo — rootName é OBRIGATÓRIO
var conteudo = editor.getData({ rootName: root });
// editor.getData()  →  LANÇA datacontroller-get-non-existent-root

// Definir conteúdo (por root)
editor.setData({ [root]: novoConteudo });

// Inserção no cursor (modelo CKEditor 5)
editor.model.change(function(writer) {
    var insertPosition = editor.model.document.selection.getFirstPosition();
    writer.insertText(texto, insertPosition);
});

// Inserção de HTML no cursor — o SEI já expõe um helper pronto:
inicializadorDll.inserirHtml(htmlParaInserir);
// GOTCHA: sem posição, insere na SELEÇÃO ATUAL e IGNORA o root informado.
// Chamado sem foco no editor, lança "Seção de conteúdo não encontrada."

// Equivalente manual, se precisar de controle fino:
var viewFragment = editor.data.processor.toView(htmlParaInserir);
var modelFragment = editor.data.toModel(viewFragment);
editor.model.insertContent(modelFragment);
```

---

## Funcionalidades e status no SEI 5

| Funcionalidade | Dependência crítica | Status SEI 5 | Plano |
|---|---|---|---|
| Barra de ferramentas extra | Localização do editor | ❌ | Aguarda mapeamento do editor CK5 |
| Nota de rodapé | `execCommand` / modelo CK5 | ❌ | Reescrever com API CK5 |
| Sumário automático | DOM do documento | ❌ | Reescrever com `editor.getData()` + parser |
| Quebra de página | `execCommand` | ❌ | Reescrever com API CK5 |
| Autossalvamento | Leitura do conteúdo | ❌ | Usar `editor.getData()` |
| Ditado por voz | Inserção no cursor | ❌ | Reescrever inserção com API CK5 |
| Equações LaTeX | DOM do documento | ❌ | Reescrever com API CK5 |
| Marca d'água | DOM do documento | ❌ | Reescrever |
| Sigilo / tarjas LGPD | DOM do documento | ❌ | Reescrever |
| Parágrafos numerados | Visualizador (não editor) | 🔲 | Provavelmente funciona — testar |
| Copiar formatação | `execCommand` | ❌ | Reescrever |
| Tabela rápida | DOM do documento | ❌ | Reescrever com API CK5 |
| Inserção de dados do processo | `execCommand` | ❌ | Reescrever |
| Referências internas | DOM do documento | ❌ | Reescrever |
| Teclas de atalho | Event listeners no editor | ❌ | Reescrever com API CK5 |

---

## Plano de adaptação

**Passo 1 — Investigação (sem código):**
- Ler `infra/infra_js/editor/inicializador.js` no fonte do SEI 5
- Mapear API pública de `InfraEditor`
- Documentar métodos disponíveis para: getData, setData, inserir no cursor, ouvir eventos

**Passo 2 — Adaptação do ponto de entrada:**
- Corrigir localização do editor no SEI 5
- Garantir que `frmEditor = isSEI_5 ? '.infra-editor__editor-completo' : '#frmEditor'` funciona
- Obter referência à instância CKEditor 5

**Passo 3 — Adaptação do autossalvamento (menor risco):**
- Substituir leitura do iframe por `editor.getData()`
- Validar que o conteúdo capturado é o HTML do documento
- Testar persistência e recuperação

**Passo 4 — Adaptação da barra de ferramentas:**
- Decidir: injetar toolbar externa ao CKEditor, ou integrar como plugin CK5
- Implementar e testar posicionamento

**Passo 5 — Adaptar funcionalidades uma a uma:**
- Prioridade: autossalvamento → notas de rodapé → sumário → sigilo LGPD → demais
