# Changelog

Todas as mudanças notáveis neste projeto serão documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/) e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

---

## [1.6.2] — 2026-08-11

Primeira versão do fork comunitário. Foco em compatibilidade com o **SEI 5**, com todas as correções validadas em instância real (SEI 5.0.0 local e SEI 5.0.3 de homologação).

### Corrigido

- **Capa do processo não aparecia no SEI 5.** `init_arvore.js` e `init_visualizacao.js` disparavam `sei-functions-pro.js` e o módulo dependente em dois `$.getScript` paralelos. O módulo menor vencia a corrida e `sei-pro-arvore.js` morria na linha 12 (`isSEI_5 is not defined`), no escopo do módulo — abortando o arquivo inteiro, inclusive a chamada a `parent.setCapaProcesso()`. Agora o carregamento é encadeado.
- **Links de filtro empilhados no Controle de Processos.** O `#divFiltro` virou uma `row` do Bootstrap no SEI 5; forçar `display: initial` (que computa como `block`) ou `inline-table` destruía o layout flex. Passa-se string vazia, devolvendo o controle ao CSS.
- **Setas do menu lateral desalinhadas.** `setInfraImg()` envolvia a seta (`/infra_css/imagens/menu_seta.png`) num `<span>`, criando um quarto item flex de ~125px que roubava o espaço do texto. A seta passa a ser excluída do wrapper.
- **`siglaUnidadeAtual` duplicada** (ex.: `"ABCABC"`). O `InfraPaginaEsquema3` renderiza a barra do sistema duas vezes, duplicando o ID `#lnkInfraUnidade`; `$(sel).text()` concatenava ambos. Adicionado `.first()`.
- **`Moment Duration Format cannot find Moment.js`.** `moment-duration-format` (4 KB) vencia a corrida contra o `moment` (53 KB). Carregamento encadeado.
- **`width: 50%` indevido no `#divFiltro`** no SEI 5.

### Adicionado

- Mapeamento completo dos seletores DOM do SEI 5 em `docs/seletores-sei.md`, com evidência (arquivo:linha do fonte ou medição no DevTools) para cada item, validado contra SEI 5.0.0 e 5.0.3.
- Levantamento da dependência do domínio `seipro.app` (desativado) no backlog do roadmap.

### Alterado

- `docs/especificacao-sei5.md` reescrito: cinco hipóteses da versão anterior foram **refutadas** por medição — entre elas a troca de `.infraCheckbox` por `.custom-control-input`, que teria quebrado a extensão.
- `docs/roadmap.md`: cinco entregas da Fase 1 canceladas por não haver o que corrigir.

---

## [Não lançado]

### Adicionado
- Estrutura de documentação para projeto comunitário
- `CONTRIBUTING.md` com guia de contribuição
- `CODE_OF_CONDUCT.md` com código de conduta
- `SECURITY.md` com política de segurança
- `docs/arquitetura.md` com visão arquitetural da extensão
- `docs/desenvolvimento.md` com guia de ambiente de desenvolvimento
- `docs/seletores-sei.md` com registro centralizado de seletores DOM
- `docs/matriz-compatibilidade.md` com matriz de compatibilidade por versão do SEI
- ADRs (Architecture Decision Records) documentando decisões técnicas

### Alterado
- Fork do projeto original [SEI-Pro/sei-pro](https://github.com/SEI-Pro/sei-pro) para continuidade comunitária

---

## Histórico do projeto original (SEI-Pro/sei-pro)

> As versões abaixo são do projeto original, mantido por Pedro Henrique Soares.

## [1.6.1] — (data não informada)

Última versão lançada pelo projeto original antes do fork comunitário.

## [1.2.1] — 2023-05-01

Versão com tag de release disponível no projeto original.

---

[Não lançado]: https://github.com/rafaelfariasbsb/sei-pro/compare/v1.6.2...HEAD
[1.6.2]: https://github.com/rafaelfariasbsb/sei-pro/compare/v1.6.1...v1.6.2
