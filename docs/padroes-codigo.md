# Padrões e Convenções de Código

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

---

## 1. Princípios gerais

- **Não quebre o SEI 4.x** — toda mudança deve manter suporte às versões anteriores
- **Visibilidade antes de abstração** — prefira código legível a código "inteligente"
- **Seletor isolado** — nunca espalhe strings de seletores DOM no código; use variáveis nomeadas
- **Falhe visivelmente** — se um elemento do SEI não é encontrado, registre no console com contexto suficiente para diagnóstico

---

## 2. Suporte a múltiplas versões do SEI

### 2.1 Flags de versão

Sempre use as flags já estabelecidas no projeto:

```javascript
// Disponíveis globalmente após sei-functions-pro.js
var isNewSEI; // true para SEI >= 4.x com sidebar menu
var isSEI_5;  // true para SEI >= 5.0
```

Não crie novas flags de versão sem discutir na issue correspondente.

### 2.2 Padrão para seletores com diferença por versão

**Errado — string literal espalhada no código:**
```javascript
$('#divInfraAreaTelaE').css('width', '300px');
```

**Correto — variável nomeada no topo do módulo:**
```javascript
// No topo do módulo ou em sei-functions-pro.js
var selPainelEsquerdo = isSEI_5 ? '#novo-painel-sei5' : '#divInfraAreaTelaE';

// No uso:
$(selPainelEsquerdo).css('width', '300px');
```

### 2.3 Padrão para blocos de comportamento diferente por versão

**Para diferenças pequenas — operador ternário:**
```javascript
var idFrame = isSEI_5 ? 'novoFrameSei5' : 'ifrArvore';
```

**Para diferenças maiores — bloco if/else com comentário:**
```javascript
// ATENÇÃO: para o EDITOR, ramifique pelo editor presente, NÃO por isSEI_5.
// O SEI 5 mantém CK4 e CK5, escolhidos por documento (CK5 é opt-in por unidade).
if (typeof inicializadorDll !== 'undefined') {
    // CKEditor 5: multi-root — rootName é OBRIGATÓRIO, não existe root 'main'
    var ed = inicializadorDll.editores[0];
    var root = ed.model.document.selection.getFirstPosition().root.rootName;
    var conteudo = ed.getData({ rootName: root });
} else {
    // Editor legado (CK4): acessa diretamente o iframe
    var conteudo = document.getElementById('ifrEditor')
        .contentDocument.body.innerHTML;
}
```

### 2.4 Registro em `docs/seletores-sei.md`

Todo seletor DOM do SEI introduzido ou modificado **deve** ser registrado na tabela do arquivo `docs/seletores-sei.md`. Esta é uma regra obrigatória para PRs que alteram seletores.

---

## 3. Variáveis e nomenclatura

### 3.1 Idioma

- **Nomes de variáveis e funções:** português ou inglês — manter consistência com o arquivo sendo editado
- **Comentários:** português (o projeto é brasileiro e o público-alvo também)
- **Mensagens para o usuário final:** sempre em português

### 3.2 Convenções existentes no projeto

O projeto usa convenções que devem ser mantidas para consistência:

```javascript
// Prefixo 'sel' para seletores DOM
var selPainelEsq = '#divInfraAreaTelaE';

// Prefixo 'ifr' para referências a iframes
var ifrArvore = document.getElementById('ifrArvore');

// Prefixo 'btn' para elementos de botão injetados
var btnFavorito = $('<button class="sei-pro-btn">...</button>');

// Sufixo 'Pro' em funções públicas do namespace da extensão
function getSeiVersionPro() { ... }
function adicionarFavoritoPro(numProcesso) { ... }
```

### 3.3 Constantes

Use `UPPER_SNAKE_CASE` para valores constantes:

```javascript
var AUTOSAVE_INTERVALO_MS = 30000;
var MAX_FAVORITOS = 100;
var STORAGE_KEY_FAVORITOS = 'seiPro_favoritos';
```

---

## 4. Manipulação do DOM

### 4.1 Sempre verificar existência antes de usar

```javascript
// Errado — quebra se elemento não existe
$('#meuElemento').text('valor');

// Correto
var $el = $('#meuElemento');
if ($el.length) {
    $el.text('valor');
} else {
    console.warn('[SEI Pro] Elemento #meuElemento não encontrado — versão SEI incompatível?');
}
```

### 4.2 Sanitização de HTML

Todo HTML criado a partir de dados externos (input do usuário, dados do SEI, APIs) deve ser sanitizado com DOMPurify:

```javascript
// Errado
$('#container').html(dadosDaAPI.descricao);

// Correto
$('#container').html(DOMPurify.sanitize(dadosDaAPI.descricao));
```

**Exceção:** HTML criado inteiramente pelo código da extensão (sem interpolação de dados externos) não precisa de sanitização.

### 4.3 Acesso a conteúdo de iframes

```javascript
// Padrão para acessar conteúdo de iframe com segurança
function getIframeDocument(iframeId) {
    var iframe = document.getElementById(iframeId);
    if (!iframe) {
        console.warn('[SEI Pro] iframe ' + iframeId + ' não encontrado');
        return null;
    }
    return iframe.contentDocument || iframe.contentWindow.document;
}
```

---

## 5. Armazenamento de dados

### 5.1 Chaves de storage

Todas as chaves de `localStorage`, `sessionStorage` e `chrome.storage` devem:
- Começar com o prefixo `seiPro_`
- Estar documentadas em `docs/modelo-dominio.md` (seção Dicionário de Dados)

```javascript
// Correto
localStorage.setItem('seiPro_favoritos', JSON.stringify(lista));

// Errado — sem prefixo, risco de colisão com o próprio SEI
localStorage.setItem('favoritos', JSON.stringify(lista));
```

### 5.2 Leitura defensiva do storage

```javascript
function carregarFavoritos() {
    try {
        var raw = localStorage.getItem('seiPro_favoritos');
        return raw ? JSON.parse(raw) : [];
    } catch (e) {
        console.error('[SEI Pro] Erro ao carregar favoritos:', e);
        return [];
    }
}
```

### 5.3 Estrutura de dados versionada

Para estruturas de dados que podem mudar entre versões da extensão, inclua versão:

```javascript
var dadosFavoritos = {
    versao: 1,
    itens: []
};
```

---

## 6. Comentários

### 6.1 Quando comentar

Comente apenas quando o **porquê** não é óbvio pelo código. Não comente o **o quê**.

```javascript
// Errado — descreve o óbvio
// Adiciona classe de destaque ao elemento
$el.addClass('sei-pro-destaque');

// Correto — explica decisão não óbvia
// O SEI 5 usa Bootstrap e já tem 'active' — precisamos de classe própria
// para não conflitar com o comportamento nativo do menu
$el.addClass('sei-pro-destaque');
```

### 6.2 Comentários de compatibilidade de versão

Quando um trecho de código existe especificamente para uma versão do SEI, documente:

```javascript
// SEI 5: CKEditor 5 expõe o conteúdo via getData(), sem acesso ao iframe
if (isSEI_5) {
    conteudo = editor.getData();
}
```

### 6.3 TODOs

Use o formato padronizado para itens pendentes:

```javascript
// TODO(sei5): mapear seletor correto quando instância SEI 5 estiver disponível
var selPainel = isSEI_5 ? '#a-confirmar' : '#divInfraAreaTelaE';
```

---

## 7. Tratamento de erros

### 7.1 Prefixo de log

Todas as mensagens de console devem incluir o prefixo `[SEI Pro]` e o módulo:

```javascript
console.log('[SEI Pro][favoritos] Carregando lista...');
console.warn('[SEI Pro][editor] Autossalvamento desativado: editor não encontrado');
console.error('[SEI Pro][storage] Falha ao salvar:', erro);
```

### 7.2 Falha silenciosa vs. falha visível

- **Funcionalidades core** (layout, lista de processos): registrar `console.warn` — não lançar exceção
- **Funcionalidades opcionais** (IA, projetos): registrar `console.info` quando indisponíveis
- **Erros inesperados**: registrar `console.error` com stack trace

```javascript
try {
    aplicarEstiloAvancado();
} catch (e) {
    console.error('[SEI Pro][layout] Erro ao aplicar estilo avançado:', e);
    // Não relança — a extensão continua funcionando sem o estilo
}
```

---

## 8. Injeção de UI

### 8.1 Classes CSS próprias

Todo elemento HTML injetado pela extensão deve ter pelo menos uma classe com prefixo `sei-pro-`:

```javascript
// Correto
var $botao = $('<button class="sei-pro-btn sei-pro-btn-favorito">★</button>');

// Errado — pode conflitar com CSS do SEI
var $botao = $('<button class="btn-favorito">★</button>');
```

### 8.2 Ordem de injeção

Aguardar o DOM do SEI estar pronto antes de injetar elementos:

```javascript
$(document).ready(function() {
    // Aguardar elementos específicos do SEI (podem carregar via AJAX)
    esperarElemento('#divInfraAreaTabela', function() {
        injetarBotaoFavorito();
    });
});
```

### 8.3 Limpeza

Funcionalidades que injetam elementos devem ser capazes de remover o que injetaram, especialmente para suportar o toggle de ativar/desativar:

```javascript
function ativarFavoritos() {
    injetarColunasNaTabela();
    registrarEventListeners();
}

function desativarFavoritos() {
    $('.sei-pro-coluna-favorito').remove();
    removerEventListeners();
}
```

---

## 9. Performance

- Prefira manipulação de DOM em batch (fora do loop) a manipulação individual por iteração
- Cache referências jQuery que são usadas múltiplas vezes em uma função
- Use `sessionStorage` para cache de dados que não mudam durante a sessão (ex: versão do SEI)
- Evite `setInterval` com intervalos menores que 1000ms sem necessidade justificada

```javascript
// Errado — busca no DOM a cada iteração
processos.forEach(function(proc) {
    $('#tabela-processos').append(criarLinha(proc));
});

// Correto — busca uma vez, monta tudo, insere uma vez
var $tabela = $('#tabela-processos');
var linhas = processos.map(criarLinha).join('');
$tabela.append(linhas);
```
