# Especificação Técnica — Migração para SEI 5

**Projeto:** SEI Pro — Fork Comunitário
**Versão:** 2.0
**Data:** 2026-08-10 (v1.0 em 2025-05-13)
**Status:** Mapeamento **concluído e validado**. Seletores confirmados no fonte do SEI 5.0.0 e 5.0.3 e no DOM de uma instância 5.0.0 rodando. Registro completo em [`seletores-sei.md`](./seletores-sei.md).

> **Mudança em relação à v1.0:** a v1.0 foi escrita por inspeção do código da extensão, antes de ter acesso ao fonte do SEI 5. Várias hipóteses dela **não se confirmaram**. As correções estão marcadas com 🔴 ao longo do documento. Onde a v1.0 acertou, o item está marcado ✅.

---

## 1. Contexto

O SEI 5 mudou o frontend em relação ao SEI 4.x. O projeto original iniciou o suporte (74 referências à flag `isSEI_5` no código) mas parou antes de concluir. Este documento especifica **o que de fato mudou**, o impacto por módulo e o plano de adaptação.

**A conclusão central do mapeamento:** o SEI 5 quebrou **muito menos** do que a v1.0 supunha. O esquema de página novo (`InfraPaginaEsquema3`) **adiciona** classes Bootstrap aos mesmos elementos, preservando IDs. A ruptura real está concentrada no **editor de documentos**.

### Base do mapeamento

| Fonte | Versão | Uso |
|---|---|---|
| Fonte oficial TRF4 | SEI 5.0.0 / Infra 2.30.0 | mapeamento inicial + instância Docker para validar no DOM |
| Fonte de uma instalação em homologação | SEI 5.0.3 / Infra 2.41.1 | validação cruzada entre versões menores |

---

## 2. O que mudou no SEI 5

### 2.1 Editor de documentos — 🔴 a mudança real, e mais complexa que o previsto

**🔴 Correção 1 — o SEI 5 não substituiu o editor; ele mantém os DOIS.**

| `documento.sta_editor` | Editor | Arquivo |
|---|---|---|
| `1` | CKEditor 4 (legado, iframe) | `sei/web/editor/ck4_processar.php` |
| `2` | CKEditor 5 (contenteditable) | `sei/web/editor/ck5_processar.php` |

A escolha é **por documento** (`editor_processar.php:31-67`), e para documento novo vem de `EditorRN::obterTipoEditorUnidade()`, que lê o parâmetro **`SEI_NOVO_EDITOR_UNIDADES`**:

| Valor | Efeito |
|---|---|
| ausente/vazio | **CK4 para todos** (default) |
| `*` | CK5 para todas as unidades |
| `ABC,XYZ` | CK5 só nessas unidades/órgãos |
| `-ABC` | exclui `ABC` (precede o `*`) |

> **Consequência para a extensão:** `isSEI_5 === true` **não** significa CKEditor 5. Um órgão pode rodar SEI 5 inteiro no CK4, e no mesmo SEI 5 o usuário abre um documento antigo em CK4 e o seguinte em CK5. **O editor tem que ser detectado em runtime**, nunca inferido da versão do SEI.

**🔴 Correção 2 — a API global não é `InfraEditor.getInstancia()`** (hipótese da v1.0, incorreta). O objeto real é `window.inicializadorDll`.

| Aspecto | SEI 4.x / CK4 | SEI 5 com CK5 |
|---|---|---|
| Engine | CKEditor 4 | CKEditor 5 (bundle DLL) |
| Detecção | `typeof CKEDITOR !== 'undefined'` | `typeof inicializadorDll !== 'undefined'` (e `CKEDITOR` é `undefined`) |
| Formulário | `#frmEditor` | `.infra-editor__editor-completo` (uma `<div>`) |
| Corpo editável | `#ifrEditor` (iframe) | `.ck-editor__editable` (divs, `id` = nome do root) |
| Toolbar | tabela HTML customizada | `.ck-toolbar` (única, ~35 botões) |
| Acesso ao conteúdo | `ifrEditor.contentDocument.body.innerHTML` | `editores[0].getData({rootName})` |

**🔴 Correção 3 — o editor é MULTI-ROOT e não existe root `'main'`.** É a descoberta que invalida a portabilidade ingênua. Os roots são um por campo do modelo do documento: `txaEditor_200`, `txaEditor_201`, `txaEditor_310`, … Medido ao vivo:

| Chamada | Resultado |
|---|---|
| `editores[0].getData()` | ❌ lança `datacontroller-get-non-existent-root` |
| `editores[0].getData({rootName:'txaEditor_200'})` | ✅ HTML daquele campo |
| `inicializadorDll.inserirHtml('<p>x</p>')` | ❌ lança `Seção de conteúdo não encontrada.` |

E uma armadilha adicional: com posição `null`, `inserirHtml` cai em `model.insertContent()` puro, que insere **na seleção atual** — o parâmetro `root` é **ignorado**. Medido: passei `txaEditor_200` e o conteúdo foi para `txaEditor_201`.

> ✅ **`inicializador-dll.js` é byte-a-byte idêntico entre 5.0.0 e 5.0.3** — esta análise vale para as duas versões.

---

### 2.2 Layout e painéis — 🔴 hipótese refutada: nada quebrou

A v1.0 supunha que o Bootstrap/Flexbox teria eliminado os elementos `float`-based. **Não eliminou.** Todos os IDs sobrevivem; o esquema 3 só acrescenta classes utilitárias.

| Elemento | SEI 4.x | SEI 5 | Ação |
|---|---|---|---|
| Painel esquerdo | `#divInfraAreaTelaE` | ✅ mesmo ID + `d-flex flex-column` | nenhuma |
| Painel direito | `#divInfraAreaTelaD` | ✅ mesmo ID + classes Bootstrap | nenhuma |
| Sidebar menu | `#divInfraSidebarMenu` | ✅ mesmo ID | nenhuma |
| Menu principal | `ul#infraMenu` | ✅ mesmo ID | nenhuma |
| Área de tabela | `#divInfraAreaTabela` | ✅ mesmo ID | nenhuma |
| Barra de comandos | `#divBotoesControleProcessos` | ✅ mesmo ID + `.barraBotoesSEI` | nenhuma |

> 🔴 **Regra derivada da validação cruzada:** classes Bootstrap **mudam entre versões menores**, IDs não. O `#divInfraAreaTelaD` foi de `class="flex-grow-1 px-3"` (5.0.0) para `class="flex-grow-1 infraAreaTelaDExibeGrande infraAreaTelaDExibePequeno"` (5.0.3). **Ancorar sempre em ID ou classe `infra*`; nunca em utilitária do Bootstrap.**

**Efeito prático:** `getIsNewSEI()` testa `#divInfraSidebarMenu ul#infraMenu` — os dois existem no SEI 5, então a função já funciona.

---

### 2.3 Iframes — 🔴 hipótese refutada: arquitetura mantida

A v1.0 marcava os três iframes como "a confirmar", com impacto "crítico — 432 e 390 referências". **Nenhum ID mudou** em relação ao SEI 4.1.

| Iframe | SEI 4.0 | SEI 4.1+ | SEI 5 |
|---|---|---|---|
| Árvore do processo | `ifrArvoreHtml` | `ifrVisualizacao` | ✅ `ifrVisualizacao` |
| Visualizador de doc | `ifrVisualizacao` | `ifrConteudoVisualizacao` | ✅ `ifrConteudoVisualizacao` |
| Conteúdo da árvore | `ifrArvore` | `ifrArvore` | ✅ `ifrArvore` |
| Editor | `ifrEditor` | `ifrEditor` | apenas quando `sta_editor=1` (CK4) |

Aninhamento confirmado no DOM:

```
document
├── #ifrArvore
└── #ifrConteudoVisualizacao
    └── #ifrVisualizacao
```

✅ **Validado ao vivo:** `getNumProcesso()` da extensão, que atravessa essa cadeia, retornou o número correto no SEI 5 **sem nenhuma alteração**.

---

### 2.4 Formulários e controles — 🔴 hipótese refutada

A v1.0 afirmava `.infraCheckbox` → `.custom-control-input` (Bootstrap 4). **Não se confirma.**

| Renderer | Classe emitida | Status |
|---|---|---|
| `IPTrCheckInfra` | `.infraCheckbox` / `.infraRadio` | **ativo por padrão** |
| `IPTrCheckBS4` | `.custom-control-input` | existe, **nunca instanciado** |

`InfraPagina::getInfraPaginaRendererFactory()` instancia `InfraPaginaRendererInfraFactory` (`InfraPagina.php:161`) e nada no fonte instancia a factory BS4. Medido no DOM: `.custom-control-input` = **0**, `.form-check-input` = **0**.

> 🔴 **Prova documental** — comentário no fonte, `InfraPagina.php:1775`:
> ```php
> //todo quando migrarem para php7, remover o createTrCheck e usar exatamente o render do IPTrCheckBS4
> ```
> O renderer BS4 é trabalho **futuro**, condicionado a uma migração de PHP que não ocorreu. Migrar os seletores da extensão para ele quebraria a extensão apostando num caminho que o SEI ainda não tomou.

**Detalhe medido:** o radio real é `class="infraRadioInput"` dentro de `.infraRadioDiv` com `label.infraRadioLabel` — não `.infraRadio`.

**Bootstrap:** `PaginaSEI::getVersaoBootstrap()` sobrescreve a infra e retorna `'5.3.1'`. O SEI 5 roda **Bootstrap 5.3.1** + shim `bootstrap-migracao-4-5.css`. Ou seja, `custom-control-input` é justamente classe que o BS5 **removeu**.

**Ação:** nenhuma troca. Onde for preciso tolerância, usar `$('.infraCheckbox, .custom-control-input[type=checkbox]')`.

---

### 2.5 Ícones — ✅ hipótese confirmada, com uma armadilha nova

GIF → SVG confirmado (0 GIFs no DOM, 44 SVGs). Mas os ícones continuam sendo `<img src>`, **não** `<svg>` inline — seletores `img[src*="…"]` seguem válidos.

| Elemento | SEI 4.x | SEI 5 |
|---|---|---|
| Documento interno | `sei_documento_interno.gif` | `svg/documento_interno.svg` |
| Formulário | `formulario1.gif` | `svg/documento_formulario1.svg` |

> 🔴 **GOTCHA — cache buster.** Todas as constantes de `Icone.php` acrescentam query string de versão:
> ```php
> const DOCUMENTO_INTERNO = DIR_SEI_SVG . '/documento_interno.svg?' . self::VERSAO;
> ```
> O `src` renderizado é `svg/documento_interno.svg?5.0.0-2.30.0-…`. Logo `img[src$=".svg"]`, `endsWith()` e `===` **nunca casam**; só `*=` funciona.
>
> ✅ **Auditado na extensão:** os dois usos de `nameDocInterno` já empregam `img[src*="…"]`, e não há nenhuma comparação por `===`/`endsWith`/`$=` sobre `.svg`/`.gif`. **Nenhuma correção necessária.**

---

### 2.6 Detecção de versão — 🔴 hipótese refutada: funciona sem alteração

A v1.0 supunha que a detecção poderia "falhar silenciosamente" e propunha três fallbacks. **Não é necessário.** `PaginaSEI.php:53` emite:

```php
'<img src="svg/sei_barra.svg?…" title="Sistema Eletrônico de Informações - Versão ' . SEI_VERSAO . ' ' . SEI_RC . '"/>';
```

O `src` virou SVG, mas a extensão seleciona **por `title`**. ✅ Medido ao vivo: a expressão exata de `setSeiVersionPro()` retorna `"5.0.0"`, e `isSEI_5` fica `true`. Idêntico em 5.0.3.

> ⚠️ **Risco residual (não corrigido):** com `SEI_RC` preenchido (ex.: `RC1`), `match(/[0-9.]/g)` captura o dígito do RC e produz `"5.0.01"`. Para `>= '5'` ainda funciona, mas comparações mais finas ficam erradas. Corrigir o regex para `\d+(\.\d+)*` quando houver oportunidade.

---

## 3. Impacto por módulo (revisado)

| Módulo | Impacto na v1.0 | Impacto **real** | Ação |
|---|---|---|---|
| `sei-functions-pro.js` | Crítico | **Baixo** — 3 linhas | ✅ corrigido (ver §3.1) |
| `sei-pro-editor.js` | Crítico | **Crítico** — confirmado, e pior que o previsto (multi-root) | Fase 2 |
| `sei-pro-arvore.js` | Crítico | **Nenhum** — classes e API da árvore inalteradas | reteste |
| `sei-pro.js` | Alto | **Nenhum** — IDs de tabela e checkbox inalterados | reteste |
| `sei-pro-favoritos.js` | Médio | **Nenhum previsto** | reteste |
| `sei-pro-atividades.js` | Médio | **Nenhum previsto** | reteste |
| `sei-pro-docs-lote.js` | Médio | **Nenhum previsto** — `chkRecebidosItem` bate | reteste |
| `sei-pro-ai.js` | Baixo/Médio | depende do editor | após Fase 2 |
| `sei-legis.js` | Médio | depende do editor | após Fase 2 |
| `sei-pro-projetos.js` | Baixo | **Nenhum** — UI própria | reteste |

> A maioria dos módulos mudou de "quebrado" para "**precisa ser retestado**". O trabalho da Fase 3 provavelmente é bem menor do que o roadmap original previa — mas isso só se confirma testando cada funcionalidade no SEI 5.

### 3.1 `sei-functions-pro.js` — corrigido

| Função / Variável | Situação | Ação |
|---|---|---|
| `getSeiVersionPro()` | ✅ funciona | nenhuma |
| `isSEI_5` | ✅ funciona | nenhuma |
| `isNewSEI` | ✅ funciona | nenhuma |
| `siglaUnidadeAtual` | 🔴 **bug** — retornava `"TESTETESTE"` | ✅ `.first()` aplicado |
| `ifrVisualizacao_` / `ifrArvoreHtml_` | ✅ funcionam | nenhuma |
| `divComandos` | ✅ funciona | nenhuma |
| `frmEditor` | ✅ já adaptado | validar na Fase 2 |

**Causa do bug:** `InfraPaginaEsquema3` emite a barra do sistema duas vezes — `#divInfraBarraSistemaMovel` (`d-md-none`) e `#divInfraBarraSistemaPadrao` — e as duas contêm o mesmo bloco. Resultado: `#lnkInfraUnidade` e a `<img>` de versão aparecem **duplicados no DOM**, e `$(sel).text()` concatena. Presente em 5.0.0 e 5.0.3.

### 3.2 `sei-pro-editor.js` — o trabalho real da migração

Equivalências corrigidas:

| Operação | CK4 | CK5 |
|---|---|---|
| Detectar o editor | `typeof CKEDITOR` | `typeof inicializadorDll` |
| Ler conteúdo | `ifrEditor.contentDocument.body.innerHTML` | `editores[0].getData({rootName: r})` — **root obrigatório** |
| Ler tudo | leitura única | iterar `editores[0].model.document.getRoots()` |
| Escrever | DOM do iframe | `setData()` **por root** |
| Inserir no cursor | `execCommand('insertHTML', …)` | `inicializadorDll.inserirHtml(html)` **com foco/seleção garantidos** |
| Seleção | `getSelection()` do iframe | `editores[0].model.document.selection` |
| Autossalvamento | `setInterval` + iframe | `setInterval` + `getData({rootName})` por root |

Root ativo:

```javascript
var ed = inicializadorDll.editores[0];
var rootAtivo = ed.model.document.selection.getFirstPosition().root.rootName;
```

---

## 4. Plano de adaptação (revisado)

```
FASE 1 — quase toda cancelada por não haver o que corrigir
├── ✅ Mapear seletores (fonte 5.0.0 + 5.0.3 + DOM ao vivo)  — CONCLUÍDO
├── ✅ Corrigir siglaUnidadeAtual (ID duplicado)              — CONCLUÍDO
├── ❌ Corrigir getSeiVersionPro()   — CANCELADO: já funciona
├── ❌ Corrigir seletores de iframe  — CANCELADO: nada mudou
├── ❌ Corrigir seletores de painel  — CANCELADO: nada mudou
├── ❌ Corrigir checkboxes p/ BS4    — CANCELADO: quebraria a extensão
├── ❌ Adaptar detecção GIF→SVG      — CANCELADO: extensão já usa src*=
└── ⏳ Smoke test em SEI 5 e SEI 4.1

FASE 2 — o trabalho de verdade
├── 1. Detecção de editor em runtime (inicializadorDll), não por isSEI_5
├── 2. Camada de acesso multi-root (get/set/inserir por root)
├── 3. Injeção da toolbar da extensão na .ck-toolbar
└── 4. Restaurar funcionalidades do editor sobre essa camada

FASE 3 — retestar módulos (provavelmente menor que o previsto)
└── Nenhuma mudança de DOM esperada; validar funcionalidade a funcionalidade
```

---

## 5. Como contribuir com este documento

Documento **vivo** — atualizar conforme o trabalho avança.

**Ao confirmar um seletor:** registrar em [`seletores-sei.md`](./seletores-sei.md) **com a evidência** (arquivo:linha do fonte, ou medição no DevTools). Nunca registrar seletor por suposição.

**Ao concluir a adaptação de um módulo:** atualizar a tabela do §3, a [matriz de compatibilidade](./matriz-compatibilidade.md) e o `CHANGELOG.md`.
