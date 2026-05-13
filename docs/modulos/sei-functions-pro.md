# Módulo: sei-functions-pro.js

**Tamanho:** ~732 KB  
**Contexto:** Carregado por todos os outros módulos  
**Status SEI 5:** ⚠️ Parcialmente adaptado  
**Arquivo:** `dist/js/sei-functions-pro.js`

---

## Responsabilidade

Núcleo compartilhado da extensão. Fornece:
- Detecção de versão do SEI
- Flags globais de versão (`isNewSEI`, `isSEI_5`)
- Seletores DOM centralizados por versão
- Funções utilitárias usadas por todos os módulos
- Acesso ao storage (localStorage, sessionStorage, chrome.storage)
- Funções de UI comuns (modais, notificações, spinners)

---

## Dependências

- jQuery (via `lib/jquery-3.4.1.min.js`)
- DOMPurify (via `lib/purify.min.js`)
- CryptoJS (via `lib/cryptojs/`)
- Moment.js (via `lib/moment/`)

---

## Variáveis globais exportadas

Estas variáveis são definidas neste módulo e consumidas pelos demais:

| Variável | Tipo | Descrição |
|---|---|---|
| `isNewSEI` | Boolean | `true` se SEI >= 4.x com sidebar menu |
| `isSEI_5` | Boolean | `true` se SEI >= 5.0 |
| `selUnidadeAtual` | String | Seletor do link da unidade atual |
| `divComandos` | String | Seletor da barra de comandos |
| `ifrVisualizacao_` | String | ID do iframe de visualização (varia por versão) |
| `ifrArvoreHtml_` | String | ID do iframe HTML da árvore |
| `frmEditor` | String | Seletor do formulário do editor |
| `mainMenu` | String | Seletor do menu principal |
| `idMenu` | String | Seletor completo do menu (com container) |

---

## Funções principais

### Detecção de versão

```
getSeiVersionPro()
  └── Retorna: String com versão do SEI (ex: "5.0.3") ou null
  └── Fonte: img[title*="Sistema Eletrônico de Informações - Versão"]
  └── Cache: sessionStorage['versaoSei']
  └── SEI 5: ⚠️ Validar se a img ainda existe no SEI 5

compareVersionNumbers(v1, v2)
  └── Retorna: -1, 0 ou 1
  └── Usado para: if (compareVersionNumbers(getSeiVersionPro(), '5') >= 0)
```

### Storage

```
seiProGetItem(chave)
  └── Lê de localStorage com tratamento de erro

seiProSetItem(chave, valor)
  └── Grava em localStorage com tratamento de erro

seiProGetConfig(chave)
  └── Lê configuração de chrome.storage.local

seiProSetConfig(chave, valor)
  └── Grava configuração em chrome.storage.local
```

### UI compartilhada

```
seiProModal(titulo, conteudo, opcoes)
  └── Exibe modal genérico da extensão
  └── Retorna: Promise resolvida com a ação do usuário

seiProNotificacao(mensagem, tipo)
  └── Exibe toast/notificação (tipo: 'sucesso', 'erro', 'aviso')

seiProSpinner(ativar)
  └── Exibe/oculta indicador de carregamento

esperarElemento(seletor, callback, tentativas)
  └── Aguarda elemento aparecer no DOM (polling)
  └── Útil para elementos carregados via AJAX pelo SEI
```

### DOM do SEI

```
getNumeroProcessoAtual()
  └── Extrai número do processo da URL ou DOM atual

getSiglaUnidadeAtual()
  └── Retorna sigla da unidade do usuário logado
  └── SEI 4.x: $('#lnkInfraUnidade').text()
  └── SEI 5: a confirmar

getIframeArvore()
  └── Retorna referência ao iframe da árvore de documentos
  └── SEI 4.x: document.getElementById('ifrArvore')
  └── SEI 5: a confirmar

getIframeVisualizacao()
  └── Retorna referência ao iframe de visualização
  └── SEI 4.1+: document.getElementById('ifrConteudoVisualizacao')
  └── SEI 5: a confirmar
```

---

## Status de adaptação SEI 5

| Elemento | Status | Observação |
|---|---|---|
| `getSeiVersionPro()` | ⚠️ | Necessita fallbacks — img pode não existir no SEI 5 |
| `isSEI_5` flag | ⚠️ | Depende de `getSeiVersionPro()` — corrigir em cascata |
| `frmEditor` | ✅ | Já adaptado: `.infra-editor__editor-completo` |
| `ifrVisualizacao_` | ⚠️ | Valor SEI 5 a confirmar via fonte |
| `divComandos` | ⚠️ | Valor SEI 5 a confirmar |
| Seletores de painel | ❌ | Não adaptados — bloqueiam layout |
| `getSiglaUnidadeAtual` | ⚠️ | `#lnkInfraUnidade` — confirmar no SEI 5 |

---

## Pontos de atenção para manutenção

1. **Tamanho do arquivo (732 KB):** contém funções de todos os domínios misturadas. Candidato a refatoração futura em módulos menores (ver [ADR-001](../adr/001-sem-build-system.md)).

2. **Seletores espalhados:** apesar de ser o "núcleo", alguns seletores ainda estão hardcoded nos módulos filhos. Oportunidade de centralização.

3. **jQuery 3.4.1:** versão desatualizada. Atualizar para 3.7+ é seguro e resolve CVEs conhecidos.
