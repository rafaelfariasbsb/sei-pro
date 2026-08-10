# Registro de Seletores DOM do SEI

Este documento é o **registro central** de todos os seletores DOM do SEI que a extensão utiliza.

Sempre que adicionar ou modificar um seletor, atualize esta tabela.

**Fonte do mapeamento SEI 5:** código-fonte oficial TRF4 do **SEI 5.0.0** (`SEI_VERSAO = '5.0.0'` em `sei/web/SEI.php`) **+ validação no DOM de uma instância SEI 5.0.0 rodando** (ambiente Docker local, base de demonstração, Bootstrap 5.3.1). Cada linha marcada ✅ tem a evidência do fonte (arquivo:linha) e, quando marcada 🔬, foi **confirmada no navegador**.

> ⚠️ **Escopo da validação:** instância limpa da base de demonstração TRF4, sem módulos de terceiros e sem tema customizado. Instalações com módulos ou temas próprios podem divergir — reconfirmar via DevTools no ambiente alvo.

---

## Como usar este documento

Antes de referenciar um elemento do SEI no código, consulte esta tabela para:
- Saber o seletor correto para cada versão
- Verificar se o seletor já está mapeado
- Identificar quais funcionalidades são afetadas se o seletor mudar

---

## Resumo do mapeamento

**A conclusão principal é que o SEI 5 mudou muito menos do que se supunha.** O SEI 5 mantém a arquitetura de iframes, os IDs de layout, os IDs de navegação e a API JS da árvore do SEI 4.1 — o esquema de página novo (`InfraPaginaEsquema3`) **adiciona** classes Bootstrap aos mesmos elementos, sem trocar os IDs.

A ruptura real está concentrada em **um único ponto: o editor de documentos**.

---

## Seletores por categoria

### Iframes principais

A arquitetura de iframes foi **mantida** no SEI 5. Nenhum ID mudou em relação ao SEI 4.1.

| Elemento | SEI 4.0 | SEI 4.1+ | SEI 5.x | Evidência (fonte SEI 5) |
|---|---|---|---|---|
| Iframe da árvore do processo | `ifrArvoreHtml` | `ifrVisualizacao` | 🔬 `ifrVisualizacao` (inalterado) | `sei/web/int/ProcedimentoINT.php:470` + DOM |
| Iframe de visualização de doc | `ifrVisualizacao` | `ifrConteudoVisualizacao` | 🔬 `ifrConteudoVisualizacao` (inalterado) | `sei/web/procedimento_trabalhar.php:301` + DOM |
| Iframe da árvore (conteúdo) | `ifrArvore` | `ifrArvore` | 🔬 `ifrArvore` (inalterado), `class="ifrArvore w-100"` | `sei/web/procedimento_anexacao.php:195` + DOM |
| Iframe interno da árvore HTML | `ifrArvoreHtml` | `ifrArvoreHtml` | ✅ `ifrArvoreHtml` (inalterado) | `sei/web/arvore_visualizar.php:586` — com `class="w-100 flex-grow-1"` |

> **Nota:** os iframes do SEI 5 ganharam classes Bootstrap, mas o **ID é o mesmo**. Seletores por ID continuam válidos.

🔬 **Aninhamento confirmado no DOM** da tela `procedimento_trabalhar` (processo aberto):

```
document
├── #ifrArvore                        (painel esquerdo — a árvore)
└── #ifrConteudoVisualizacao          (painel direito)
    └── #ifrVisualizacao              (conteúdo renderizado)
```

Dentro de `#ifrArvore`: `.infraArvoreNoSelecionado` = 1, links com `target="ifrVisualizacao"`, ícones `img[src*=".svg"]` = 9 e `img[src*=".gif"]` = **0**.

🔬 **A cadeia crítica da extensão funciona:** `getNumProcesso()` — que faz `$('#ifrArvore').contents().find('a[target="ifrVisualizacao"]').eq(0).text()` — retornou corretamente `26.0.000000001-8` no SEI 5.

---

### Layout e painéis

Todos os IDs **sobreviveram**. O `InfraPaginaEsquema3` (esquema do SEI 5) apenas acrescenta classes utilitárias Bootstrap.

| Elemento | SEI 4.x | SEI 5.x | Evidência (fonte SEI 5) |
|---|---|---|---|
| Painel esquerdo (sidebar) | `#divInfraAreaTelaE` | ✅ `#divInfraAreaTelaE` + `class="divInfraAreaTelaE d-flex flex-column"` | `infra/infra_php/InfraPaginaEsquema3.php:1149` |
| Painel direito (conteúdo) | `#divInfraAreaTelaD` | ✅ `#divInfraAreaTelaD` + `class="flex-grow-1 px-3"` | `infra/infra_php/InfraPaginaEsquema3.php:1153` |
| Sidebar menu container | `#divInfraSidebarMenu` | ✅ `#divInfraSidebarMenu` + `class="infraSidebarMenu flex-grow-1"` | `infra/infra_php/InfraPaginaEsquema3.php:763` |
| Menu principal (ul) | `#infraMenu` | ✅ `#infraMenu` (inalterado) | `infra/infra_php/InfraPaginaEsquema3.php:776` (`<ul id="infraMenu">`) |
| Campo de busca no menu | — | ✅ `#txtInfraPesquisarMenu` (novo no esquema 3) | `infra/infra_php/InfraPaginaEsquema3.php:767` |
| Área de tabela | `#divInfraAreaTabela` | ✅ `#divInfraAreaTabela` (inalterado) | `sei/web/protocolo_pesquisa.php:533`, `sei/web/acesso_federacao_envio.php:579` |
| Barra de botões (controle de processos) | `#divBotoesControleProcessos` | ✅ `#divBotoesControleProcessos` + `class="barraBotoesSEI"` | `sei/web/procedimento_controlar.php:1424` |

> **Consequência prática:** `getIsNewSEI()` da extensão testa `$('#divInfraSidebarMenu ul#infraMenu').length`. **Os dois existem no SEI 5** → a função retorna `true` corretamente. Nenhuma correção necessária.

---

### Árvore do processo

A árvore continua sendo construída pela mesma API JS (`InfraArvore`), com as mesmas classes e o mesmo contrato de objetos.

| Elemento | SEI 4.x | SEI 5.x | Evidência (fonte SEI 5) |
|---|---|---|---|
| Nó selecionado na árvore | `.infraArvoreNoSelecionado` | ✅ `.infraArvoreNoSelecionado` (inalterado) | `infra/infra_js/arvore/InfraArvore.js:619` |
| Construtor de nó (JS) | `new infraArvoreNo(...)` | ✅ `new infraArvoreNo(...)` (inalterado) | `sei/web/arvore_montar.php:242`, `sei/web/js/arvore_montar.js:469` |
| Construtor de ação (JS) | `new infraArvoreAcao(...)` | ✅ `new infraArvoreAcao(...)` (inalterado) | `sei/web/arvore_montar.php:283`, `sei/web/int/ProcedimentoINT.php:473` |
| Div de informação da árvore | `#divArvoreInformacao` | ✅ `#divArvoreInformacao` (inalterado) | `sei/web/arvore_visualizar.php:581` |
| Âncora de visualização | `a.ancoraVisualizacaoArvore` | ✅ `a.ancoraVisualizacaoArvore` (inalterado) | `sei/web/int/ProcedimentoINT.php:697,892` |

---

### Editor de documentos

**Este é o ponto de ruptura real do SEI 5.**

#### O SEI 5 tem DOIS editores, escolhidos por documento

O SEI 5 **não substituiu** o editor antigo — ele mantém os dois e escolhe em tempo de execução:

| `sta_editor` | Editor carregado | Arquivo |
|---|---|---|
| `1` | CKEditor 4 (legado, iframe) | `sei/web/editor/ck4_processar.php` |
| `2` | CKEditor 5 (novo, contenteditable) | `sei/web/editor/ck5_processar.php` |

A escolha vem de `documento.sta_editor` (coluna do próprio documento) ou, para documento novo, de `EditorRN::obterTipoEditorUnidade()` — **configuração por unidade**.
Evidência: `sei/web/editor/editor_processar.php:31-67`.

#### O CK5 é OPT-IN por órgão/unidade

`EditorRN::obterTipoEditorUnidade()` (`sei/web/editor/rn/EditorRN.php:1960-1978`) lê o parâmetro **`SEI_NOVO_EDITOR_UNIDADES`**, um CSV de siglas:

| Valor | Efeito |
|---|---|
| ausente / vazio | **CK4 para todo mundo** (default) |
| `*` | CK5 para todas as unidades |
| `ABC,XYZ` | CK5 só nessas unidades/órgãos |
| `-ABC` | exclui `ABC` (tem precedência sobre o `*`) |

Há ainda `SEI_NOVO_EDITOR_MODELOS` para o editor de modelos (`obterTipoEditorSimples()`).

> 🔴 **Consequência:** um órgão pode rodar SEI 5.0.0 **inteiro no CK4**. `isSEI_5 === true` **não** significa CKEditor 5. A instância de referência deste mapeamento tem `SEI_NOVO_EDITOR_UNIDADES = '*'` (🔬 confirmado: documento criado nasceu com `sta_editor = 2`).

> 🔴 **Implicação para a extensão:** o editor **não pode ser decidido por `isSEI_5`**. No mesmo SEI 5, o usuário abre um documento antigo em CK4 (com `#ifrEditor`) e o seguinte em CK5 (sem iframe). A extensão precisa detectar **qual editor está montado na página**, não qual versão do SEI está rodando.
>
> Detecção sugerida em runtime:
> ```javascript
> var isCK5 = typeof window.inicializadorDll !== 'undefined'
>          && inicializadorDll.editores.length > 0;
> ```

#### Seletores por editor

Medido no DOM da página `acao=editor_montar` de um documento com `sta_editor=2`:

| Elemento | SEI 4.x / CK4 | SEI 5 com CK5 | Ocorrências medidas |
|---|---|---|---|
| Formulário do editor | `#frmEditor` | 🔬 `.infra-editor__editor-completo` (é uma `<div>`) | `#frmEditor` = **0** · `.infra-editor__editor-completo` = **1** |
| Corpo editável | `#ifrEditor` (iframe) | 🔬 `.ck-editor__editable` (div contenteditable) | `#ifrEditor` = **0** · `.ck-editor__editable` = **10** |
| Conteúdo | corpo do iframe | 🔬 `.ck-content` | **8** |
| Barra de ferramentas | toolbar do editor legado | 🔬 `.ck-toolbar` (única, **35 botões**) — ponto de injeção dos botões da extensão | **1** |
| Form de salvamento (CK4) | `#frmEditor` + `ifrEditorSalvar` | ✅ ainda existe quando `sta_editor=1` | `sei/web/editor/ck4_processar.php:961` |
| UI de comentários | — | `.comentario__ui`, `#comentario__active-style` | `inicializador-dll.js` (método `restaurar`) |

> 🔬 **`window.CKEDITOR` é `undefined`** na página CK5 — o global do CKEditor 4 não existe. Serve como teste negativo confiável.
>
> 🔬 Cada `.ck-editor__editable` tem **`id` igual ao nome do root** (`txaEditor_200`, `txaEditor_201`, …) e só **parte** deles é `contenteditable="true"` — os demais são regiões travadas do modelo do documento. Não assuma que todo editável aceita escrita.

#### API JavaScript do CKEditor 5 no SEI 5

O SEI 5 expõe um objeto global próprio. **Não é `InfraEditor.getInstancia()`** (suposição anterior da especificação, incorreta) — o objeto real é:

```javascript
window.inicializadorDll          // instância de InfraEditor.inicializadorDll.InfraEditor
window.InfraEditor               // namespace
window.InfraEditorException      // classe de exceção
window.INFRA_EDITOR_CONFIG       // configuração serializada pelo PHP
window.INFRA_EDITOR_PULAR_INICIALIZACAO
```

Métodos públicos de `inicializadorDll` (extraídos de `infra/infra_js/editor/ck5/inicializador-dll.js`):

| Método | Assinatura | Uso |
|---|---|---|
| `editores` | `Array` | **instâncias CKEditor 5 ativas** — `inicializadorDll.editores[0]` é o editor principal |
| `montar(config)` | `{}` → `Promise<editor>` | monta o editor |
| `desmontar()` | — | destrói os editores (grava `getData()` no `sourceElement` antes) |
| `atualizar(config)` | — | `desmontar()` + `montar()` |
| `inserirHtml(html, posicao, editor, root)` | `root` default `'main'` | **insere HTML no modelo** — é o substituto direto do `execCommand('insertHTML')` |
| `restaurar(state)` | base64 ou objeto | restaura estado serializado |

🔬 Confirmado no DOM: `typeof inicializadorDll === 'object'`, `editores.length === 1`, métodos expostos exatamente `montar, desmontar, atualizar, inserirHtml, restaurar` (+ privados `_getEditorPackage`, `_handleAfterEditorInit`, `_removeLoading`).

#### 🔴 O editor é MULTI-ROOT e NÃO existe root `'main'`

Esta é a descoberta que invalida a portabilidade ingênua. Os roots do documento são **um por campo do modelo**, nomeados a partir das textareas:

```
txaEditor_310, txaEditor_200, txaEditor_201, txaEditor_202,
txaEditor_203, txaEditor_204, txaEditor_207, txaEditor_311
```

**Não há root `'main'`** — que é justamente o default de `inserirHtml()`. Testado ao vivo:

| Chamada | Resultado medido |
|---|---|
| `editores[0].getData()` | ❌ **lança** `datacontroller-get-non-existent-root` |
| `editores[0].getData({rootName:'txaEditor_200'})` | ✅ retorna o HTML daquele campo |
| `inicializadorDll.inserirHtml('<p>x</p>')` | ❌ **lança** `Seção de conteúdo não encontrada.` |
| `inicializadorDll.inserirHtml(html, null, ed, 'txaEditor_200')` | ✅ não lança |

> ⚠️ **Armadilha extra medida:** no caso acima, passei o root `txaEditor_200` e o HTML **foi parar no `txaEditor_201`**. Lendo o fonte: quando a posição é `null`, o método cai em `model.insertContent(a)` puro, que insere **na seleção atual** — o parâmetro `root` só é usado quando se passa uma posição. Ou seja, com posição `null` o root informado é **ignorado**, e vale onde o cursor está.

Equivalências corrigidas para portar `sei-pro-editor.js`:

| Operação | CK4 (SEI 4.x) | CK5 (SEI 5) |
|---|---|---|
| Ler conteúdo | `ifrEditor.contentDocument.body.innerHTML` | `editores[0].getData({rootName: r})` — **root obrigatório** |
| Ler tudo | leitura única do iframe | iterar `editores[0].model.document.getRoots()` |
| Escrever conteúdo | manipulação do DOM do iframe | `editores[0].setData(...)` — **por root** |
| Inserir no cursor | `document.execCommand('insertHTML', …)` | `inicializadorDll.inserirHtml(html)` **só depois de garantir foco/seleção no root certo** |
| Obter seleção | `ifrEditor.contentDocument.getSelection()` | `editores[0].model.document.selection` |
| Autossalvamento | `setInterval` + leitura do iframe | `setInterval` + `getData({rootName})` por root |

**Descobrir o root ativo** (padrão sugerido):

```javascript
var ed = inicializadorDll.editores[0];
var rootAtivo = ed.model.document.selection.getFirstPosition().root.rootName;
```

---

### Navegação e identidade

| Elemento | SEI 4.x | SEI 5.x | Evidência (fonte SEI 5) |
|---|---|---|---|
| Link da unidade atual | `#lnkInfraUnidade` | 🔬 `#lnkInfraUnidade` + `class="form-control infraAcaoBarraConjugada"` — **renderizado DUAS VEZES, ver bug abaixo** | `infra/infra_php/InfraPaginaEsquema3.php:734` + DOM |
| Selector de unidades (legado) | `#selInfraUnidades` | 🔬 removido da UI (0 no DOM), mas o **POST** ainda aceita `selInfraUnidades` | `infra/infra_php/InfraSessao.php:154-168` + DOM |
| Imagem de versão do SEI | `img[title*="Sistema Eletrônico de Informações - Versão"]` | 🔬 **mesmo `title`**, `src` mudou de GIF para `svg/sei_barra.svg` — **também duplicada** | `sei/web/PaginaSEI.php:53` + DOM |

#### 🔴 BUG CONFIRMADO — `#lnkInfraUnidade` duplicado quebra `siglaUnidadeAtual`

No SEI 5 o elemento é renderizado **duas vezes com o mesmo ID** (uma cópia oculta e uma visível — a barra responsiva do esquema 3 emite as duas variantes). Confirmado no DOM: `document.querySelectorAll('#lnkInfraUnidade').length === 2`, ambas com texto `TESTE`.

A extensão faz (`sei-functions-pro.js:37`):

```javascript
var siglaUnidadeAtual = isNewSEI ? $('#lnkInfraUnidade').text().trim() : ...
```

`$(sel).text()` **concatena o texto de todos os elementos casados**. Resultado medido no SEI 5:

| | valor |
|---|---|
| O que a extensão calcula hoje | `"TESTETESTE"` ❌ |
| Valor correto | `"TESTE"` |

**Correção:** usar `.first()`/`.eq(0)` (ou `:visible`) — `$('#lnkInfraUnidade').first().text().trim()`.

> `idUnidade` **não** é afetado: vem de `.attr('onclick')`, que retorna só o primeiro elemento, e as duas cópias têm a mesma URL (`infra_unidade_atual` idêntico).

#### Detecção de versão — **funciona sem alteração**

`sei/web/PaginaSEI.php:53` emite:

```php
'<img src="svg/sei_barra.svg?' . $this->getNumVersao() . '" title="Sistema Eletrônico de Informações - Versão ' . SEI_VERSAO . ' ' . SEI_RC . '"/>';
```

A extensão (`setSeiVersionPro()`) seleciona **por `title`**, não por `src` — então a troca GIF→SVG **não quebra a detecção**.

🔬 **Validado no DOM:** simulando a expressão exata da extensão no SEI 5.0.0, o retorno é `"5.0.0"`. `isSEI_5` fica `true` corretamente. **Nenhuma correção necessária.**

> ⚠️ **Risco residual:** se a instalação tiver `SEI_RC` preenchido (ex.: `RC1`), o `match(/[0-9.]/g)` capta o dígito do RC e produz `"5.0.01"`. `compareVersionNumbers` ainda retorna `>= 0` contra `'5'`, então `isSEI_5` continua correto — mas comparações mais finas (ex.: `>= 5.0.1`) ficam erradas. Corrigir o regex para capturar só o primeiro grupo `\d+(\.\d+)*` (sugerido).

---

### Formulários e controles

> 🔴 **Correção da especificação:** a premissa de que o SEI 5 troca `.infraCheckbox` por `.custom-control-input` **não se confirma** nesta base.

O SEI 5 tem dois renderizadores de checkbox, e o **legado é o ativo por padrão**:

| Renderer | Classe emitida | Arquivo |
|---|---|---|
| `IPTrCheckInfra` (**padrão ativo**) | `.infraCheckbox` / `.infraRadio` | `infra/infra_php/infrapagina/IPTrCheckInfra.php:28-32` |
| `IPTrCheckBS4` (existe, **não instanciado**) | `.custom-control-input` | `infra/infra_php/infrapagina/IPTrCheckBS4.php:30-39` |

`InfraPagina::getInfraPaginaRendererFactory()` instancia `InfraPaginaRendererInfraFactory` por padrão (`infra/infra_php/InfraPagina.php:161`), e nenhum ponto do fonte instancia `InfraPaginaRendererFactoryBS4`.

| Elemento | SEI 4.x | SEI 5.x | Ação |
|---|---|---|---|
| Checkboxes | `.infraCheckbox` | ✅ `.infraCheckbox` (inalterado no padrão) | **nenhuma** — não trocar o seletor |
| Radios | `.infraRadio` | 🔬 **`.infraRadioInput`**, dentro de `.infraRadioDiv`, com `label.infraRadioLabel` | usar `.infraRadioInput` |
| Checkboxes (variante BS4) | — | 🔬 `.custom-control-input` = **0 no DOM** (só se o órgão ativar o renderer BS4) | tratar como **fallback**, não substituto |
| Botões de ação | `.infraButton` | 🔬 `.infraButton` presente (coexiste com `.btn` do Bootstrap) | manter ambos |

🔬 **Medido no DOM** (tela de Controle de Processos + Iniciar Processo): `.custom-control-input` = 0, `.form-check-input` = 0. HTML real de um radio no SEI 5:

```html
<div class="infraRadioDiv">
  <input type="radio" name="rdoNivelAcesso" id="optPublico" value="0" class="infraRadioInput" tabindex="1000">
  <label class="infraRadioLabel" for="optPublico"></label>
</div>
```

> **Padrão recomendado:** usar seletor tolerante às formas — `$('.infraCheckbox, .custom-control-input[type=checkbox]')` — em vez de trocar por `isSEI_5`.

**Versão do Bootstrap:** `InfraPaginaEsquema3::getVersaoBootstrap()` retorna `'4.6.2'`, mas `PaginaSEI::getVersaoBootstrap()` **sobrescreve para `'5.3.1'`** (`sei/web/PaginaSEI.php:298`). 🔬 Confirmado no DOM — os CSS carregados são:

```
/infra_css/bootstrap/bootstrap-5.3.1.min.css?5.0.0-2.30.0-…
/infra_css/bootstrap/menu-bootstrap.css?…
/infra_css/bootstrap/bootstrap-migracao-4-5.css?…      ← shim de migração BS4→BS5
```

Ou seja, o SEI 5 roda **Bootstrap 5.3.1** com um shim de compatibilidade. Classes específicas de BS4 (como `custom-control-input`) são justamente as que o BS5 removeu — mais um motivo para **não** migrar os seletores para elas.

---

### Tabela de processos e paginação (Controle de Processos)

Mapeado no fonte **SEI 5.0.3** (`procedimento_controlar.php`). Nada mudou em relação ao SEI 4.x — os IDs de tabela que a extensão usa continuam existindo.

| Elemento | SEI 4.x | SEI 5.x | Evidência (5.0.3) |
|---|---|---|---|
| Tabela de processos recebidos | `#tblProcessosRecebidos` | ✅ inalterado | `procedimento_controlar.php` — `class="infraTable tabelaControle"` |
| Tabela de processos gerados | `#tblProcessosGerados` | ✅ inalterado | idem |
| Tabela do modo detalhado | `#tblProcessosDetalhado` | ✅ inalterado | idem |
| Container recebidos / gerados | `#divRecebidos` / `#divGerados` | ✅ inalterado (classes Bootstrap `d-block`/`d-none`) | idem |
| Container do modo detalhado | `#divTabelaDetalhado` | ✅ inalterado | idem |
| Barra de botões | `#divBotoesControleProcessos` | ✅ inalterado + `class="barraBotoesSEI"` | idem |
| Tabelas de filtro | `#tblMarcadores`, `#tblTiposProcedimento`, `#tblTiposPrioridade` | ✅ inalterado | idem |
| Área de paginação | `.infraAreaPaginacao` | ✅ inalterado | `IPAreaPaginacaoInfra.php` (renderer ativo) |

**Checkboxes das linhas** — gerados por `PaginaSEI::getTrCheck()` → `IPTrCheckInfra::render()`:

```php
$strTagId = 'chk' . $strNomeSelecao . 'Item' . $this->numItem;
$elInput  = '<input class="infraCheckbox" id="'.$strTagId.'" …  type="checkbox" …/>';
```

Nomes de seleção usados na tela: `Recebidos`, `Gerados`, `Detalhado`, `Marcadores`, `TiposProcessos`, `TiposPrioridades`. Resultado: `#chkRecebidosItem0`, `#chkRecebidosItem1`, … — **exatamente o padrão que a extensão já procura** (`chkRecebidosItem`). Classe `.infraCheckbox`, como mapeado.

> 🔴 **Prova definitiva de que o renderer BS4 é código morto** — comentário no fonte, `InfraPagina.php:1775`:
> ```php
> //todo quando migrarem para php7, remover o createTrCheck e usar exatamente o render do IPTrCheckBS4
> ```
> O `IPTrCheckBS4` é trabalho **futuro**, condicionado a uma migração de PHP que não aconteceu. Migrar os seletores da extensão para `.custom-control-input` seria apostar num caminho que o próprio SEI ainda não tomou.

---

### Indicadores visuais (ícones)

Confirmada a migração GIF → SVG, mas **os ícones continuam sendo `<img src>`** — não viraram `<svg>` inline. Seletores `img[src*="…"]` continuam funcionando; seletores que casam `.gif` quebram.

| Elemento | SEI 4.x | SEI 5.x | Evidência (fonte SEI 5) |
|---|---|---|---|
| Ícone de documento interno | `sei_documento_interno.gif` | ✅ `svg/documento_interno.svg` | `sei/web/Icone.php:140` |
| Ícone de formulário | `formulario1.gif` | ✅ `svg/documento_formulario1.svg` (e `…formulario2.svg`) | `sei/web/Icone.php:133-134` |
| Ícones em geral | `imagens/*.gif` | ✅ `svg/*.svg` via constantes de `Icone.php` | `sei/web/Icone.php` (classe inteira) |
| Ícones legados | — | `sei/web/imagens/*.gif` **ainda presentes** no pacote | diretório `sei/web/imagens/` |

> 🔴 **GOTCHA — cache buster:** todas as constantes de `Icone.php` acrescentam **query string de versão**:
> ```php
> const DOCUMENTO_INTERNO = DIR_SEI_SVG . '/documento_interno.svg?' . self::VERSAO;
> ```
> O `src` renderizado é `svg/documento_interno.svg?5.0.0-…`. Portanto:
> - `img[src$=".svg"]` → **nunca casa**
> - `src.endsWith('documento_interno.svg')` → **nunca casa**
> - `img[src*="documento_interno.svg"]` → ✅ casa
>
> A extensão já usa `nameDocInterno = 'documento_interno.svg'` com comparação por conteúdo — **verificar se cada uso é `*=` e não `$=`/`===`**.

---

## Status do mapeamento SEI 5

| Categoria | Status | Resultado |
|---|---|---|
| Iframes principais | 🔬 Validado no DOM | Inalterados em relação ao SEI 4.1 |
| Layout e painéis | 🔬 Validado no DOM | IDs preservados; classes Bootstrap adicionadas |
| Árvore do processo | 🔬 Validado no DOM | API e classes inalteradas; `getNumProcesso()` funciona |
| Navegação e identidade | 🔬 Validado no DOM | **1 bug encontrado** (`#lnkInfraUnidade` duplicado) |
| Detecção de versão | 🔬 Validado no DOM | Funciona sem alteração — retorna `"5.0.0"` |
| Formulários (checkbox/radio) | 🔬 Validado no DOM | Inalterado — premissa anterior refutada; radio é `.infraRadioInput` |
| Indicadores visuais (ícones) | 🔬 Validado no DOM | GIF→SVG confirmado (0 GIFs) + gotcha de query string |
| Editor CK5 — API e DOM | 🔬 Validado no DOM | **multi-root sem `'main'`**; `getData()` e `inserirHtml()` sem root lançam exceção |
| Editor CK5 — toolbar/plugins | ⚠️ Parcial | toolbar única com 35 botões localizada; falta desenhar a injeção dos botões da extensão |
| Tabela de processos / paginação | ✅ Mapeado (fonte 5.0.3) | IDs e `.infraCheckbox` inalterados; `chkRecebidosItem` bate com a extensão |

### Validação cruzada 5.0.0 × 5.0.3

O mapeamento foi reconferido contra o fonte do **SEI 5.0.3** (Infra 2.41.1) — obtido de uma instalação em ambiente de homologação.

**Todas as âncoras se mantêm.** Duas observações que valem como regra de projeto:

1. **`inicializador-dll.js` é byte-a-byte idêntico** entre 5.0.0 e 5.0.3 (5.075 bytes). Toda a análise do CK5 — multi-root, ausência de `'main'`, comportamento de `inserirHtml` — vale nas duas versões.
2. 🔴 **Classes Bootstrap mudam entre versões menores; IDs não.** O mesmo elemento em duas versões 5.0.x:
   ```
   5.0.0:  <div id="divInfraAreaTelaD" class="flex-grow-1 px-3">
   5.0.3:  <div id="divInfraAreaTelaD" class="flex-grow-1 infraAreaTelaDExibeGrande infraAreaTelaDExibePequeno">
   ```
   → **preferir sempre seletor por ID ou por classe `infra*`**; nunca ancorar em classe utilitária do Bootstrap (`px-3`, `d-flex`, `w-100`).

**Causa raiz do ID duplicado** (bug #1 abaixo): `InfraPaginaEsquema3` emite a barra do sistema **duas vezes** — `#divInfraBarraSistemaMovel` (`d-md-none`, linha 1270) e `#divInfraBarraSistemaPadrao` (linha 1283) — e ambas contêm o mesmo bloco. Por isso `#lnkInfraUnidade` e a `<img>` de versão aparecem em duplicata. **Presente igual em 5.0.0 e 5.0.3.**

### Bugs da extensão descobertos por este mapeamento

| # | Onde | Problema | Correção |
|---|---|---|---|
| 1 | `sei-functions-pro.js:37` | `siglaUnidadeAtual` retorna `"TESTETESTE"` — ID duplicado no SEI 5 | `.first()` antes de `.text()` |
| 2 | `sei-pro-editor.js` (porte CK5) | `getData()` sem `rootName` **lança exceção** no SEI 5 | iterar roots explicitamente |
| 3 | `sei-pro-editor.js` (porte CK5) | `inserirHtml()` sem root **lança exceção**; com root e posição `null` o root é **ignorado** | garantir foco/seleção antes de inserir |
| 4 | vários | `isSEI_5` **não implica CKEditor 5** — CK5 é opt-in por unidade | detectar via `window.inicializadorDll` |
| 5 | `sei-functions-pro.js` (ícones) | `src` de SVG carrega query string `?versão` | usar sempre `*=`, nunca `$=`/`===` |

---

## Como atualizar este documento

1. Identifique o novo seletor no HTML do SEI (usando DevTools ou o código-fonte)
2. Adicione ou atualize a linha correspondente na tabela, **com a evidência** (arquivo:linha ou print do DevTools)
3. Atualize o código da extensão usando o padrão:
   ```javascript
   var meuElemento = isSEI_5 ? '#novo-seletor' : '#seletor-antigo';
   ```
   — mas prefira **seletor tolerante às duas formas** quando ambos coexistirem (caso dos checkboxes)
4. Faça commit com a mensagem: `docs: atualiza seletor X para SEI 5`
