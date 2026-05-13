# Módulo: sei-pro-arvore.js

**Tamanho:** ~173 KB  
**Contexto:** Injetado por `init_arvore.js` dentro do iframe da árvore de documentos  
**Página SEI:** `?acao=arvore_visualizar` (dentro do iframe `ifrArvore`)  
**Status SEI 5:** 🔲 Não testado  
**Arquivo:** `dist/js/sei-pro-arvore.js`

---

## Responsabilidade

Opera dentro do iframe da árvore de documentos, adicionando:
- Menu rápido de ações ao passar o mouse sobre documentos
- Ícones de ação rápida (duplicar, visualizar, baixar)
- Informações adicionais na linha do documento (tipo, número, data)
- Anotações diretamente pela árvore
- Numeração de documentos
- Detecção de tipo de documento (formulário vs. externo vs. gerado)
- Indicadores visuais de estado do documento

---

## Dependências

- `sei-functions-pro.js` (base)
- Opera no contexto do **iframe** `ifrArvore` — contexto JavaScript isolado da página pai

---

## Pontos críticos para SEI 5

### 1. ID do iframe pai

A extensão referencia o iframe a partir da página pai com o ID `ifrArvore`. Se o SEI 5 alterou este ID, o módulo de inicialização (`init_arvore.js`) não consegue injetar neste contexto.

**Verificar no fonte do SEI 5:** Buscar `ifrArvore` em templates PHP.

### 2. Detecção de tipo de documento via GIF

O SEI 4.x usa ícones GIF para indicar o tipo de documento. A extensão detecta o tipo lendo o atributo `src` da imagem:

```javascript
// SEI 4.x
if ($('img[src*="formulario1.gif"]').length) {
    // é um formulário
}
if ($('img[src*="doc_gerado.gif"]').length) {
    // é documento nato-digital
}
```

**SEI 5:** Usa SVG. A detecção por `src` de GIF quebra completamente. Necessário identificar o padrão de SVG ou atributo alternativo usado pelo SEI 5 para indicar tipo de documento.

**Possíveis abordagens para SEI 5:**
```javascript
// Opção A: Detectar por classe CSS do SVG
if ($('svg.ico-formulario').length) { ... }

// Opção B: Detectar por atributo data-*
if ($('[data-tipo-doc="formulario"]').length) { ... }

// Opção C: Detectar por texto alternativo/título
if ($('img[title="Formulário"]').length) { ... }
```

### 3. Estrutura HTML da árvore

O SEI 4.x renderiza a árvore como uma estrutura de `ul/li` com classes `infraArvoreNo*`. Se o SEI 5 alterou esta estrutura para Bootstrap ou outra convenção, os seletores de nó, ação e tipo quebram.

```javascript
// Seletores críticos a confirmar no SEI 5
'.infraArvoreNoSelecionado'  // nó selecionado atualmente
'.infraArvoreNo'             // nó genérico
'.infraArvoreAcao'           // ação dentro de um nó
'a.infraArvoreNoDesc'        // link do nome do documento
```

---

## Funcionalidades e status no SEI 5

| Funcionalidade | Dependência crítica | Status SEI 5 | Plano |
|---|---|---|---|
| Menu rápido de ações | Estrutura HTML da árvore | 🔲 | Mapear seletores SEI 5 |
| Ícones de ação rápida | Ícones SVG SEI 5 | 🔲 | Adaptar detecção de tipo |
| Informações adicionais | Estrutura HTML da árvore | 🔲 | Mapear seletores SEI 5 |
| Anotações pela árvore | Estrutura HTML da árvore | 🔲 | Mapear seletores SEI 5 |
| Numeração de documentos | Estrutura HTML da árvore | 🔲 | Mapear seletores SEI 5 |
| Detecção tipo de documento | GIF → SVG | 🔲 | Reescrever detecção para SVG |
