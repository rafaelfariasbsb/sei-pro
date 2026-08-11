# Changelog

Todas as mudanças notáveis neste projeto serão documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/) e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

---

## [1.6.3] — 2026-08-11

Primeira versão validada também em **SEI 4.1** (produção). A 1.6.2 havia sido testada apenas em SEI 5.

### Corrigido

- **Sub-itens do menu lateral desalinhados no SEI 4.1.** Itens sem ícone próprio (Avaliação CPAD, Avaliação Documental, Documentos para Eliminação, Editais de Eliminação, Localizadores) apareciam com o texto deslocado para a direita. O SEI usa `vazio.svg` como espaçador nesses casos, e `setInfraImg()` o envolvia num `<span>` que, dentro da âncora `display:flex`, ocupava largura e empurrava o texto. Medido em produção: **todos** os itens desalinhados tinham o wrapper, **todos** os alinhados não tinham. Bug pré-existente do projeto original — o CSS do Estilo Avançado já escondia esse `<span>`, mas só quando o Estilo Avançado estava ligado.

### Alterado

- **Os blocos do painel lateral da árvore agora nascem recolhidos.** São 11 blocos (Especificação, Anotações, Atribuição, Marcador, Acompanhamento Especial, Tipo de Processo, Nível de Acesso, Bloco Interno, Interessados, Assuntos, Observações e Atividades). Antes o painel abria inteiramente expandido, ocupando toda a lateral. A preferência de quem já recolheu ou expandiu continua sendo respeitada.
- Documentação preparada para uso público: instruções de instalação passo a passo para Chromium e Firefox, orientação para atualizar, aviso sobre conflito com a extensão original, e matriz de compatibilidade indicando explicitamente **o que foi testado** e o que permanece não verificado.

---

## [1.6.2] — 2026-08-11

Primeira versão do fork comunitário. Foco em compatibilidade com o **SEI 5**, com todas as correções validadas em instância real (SEI 5.0.0 local e SEI 5.0.3 de homologação).

### Corrigido — Capa do processo e painel lateral

Três bugs **independentes** produziam o mesmo sintoma: o painel de dados do processo (Histórico de tramitação, QR Code, Data de Autuação, Assuntos, Interessados, Nível de Acesso, Marcador) simplesmente não aparecia. Nenhum deles resolvia sozinho.

- **Corrida de carregamento matava o `sei-pro-arvore.js`.** `init_arvore.js` e `init_visualizacao.js` disparavam `sei-functions-pro.js` e o módulo dependente em dois `$.getScript` paralelos. O módulo menor vencia, e `sei-pro-arvore.js` morria na linha 12 (`isSEI_5 is not defined`) — no escopo do módulo, o que aborta o arquivo **inteiro**, inclusive a chamada a `parent.setCapaProcesso()`.
- **`target` errado impedia a coleta de iniciar.** `initDadosProcesso()` procurava `a[target="ifrConteudoVisualizacao"]` dentro do `#topmenu`, mas ali o link aponta para `ifrVisualizacao`. Com a condição nunca satisfeita, a função reagendava a si mesma até esgotar o tempo e **desistia em silêncio** — o iframe de coleta nunca era criado.
- **Ninguém chamava a montagem depois que os dados chegavam.** As chamadas existentes disparam no load dos iframes, antes de a coleta assíncrona terminar; a retentativa interna só existe dentro do bloco de sucesso. Agora a capa e o painel lateral são acionados ao final da coleta.
- **Painel lateral da árvore** (Especificação, Anotações, Atribuição, Marcador, Acompanhamento Especial): mesma causa — `initDadosProcessoArvore()` só tentava por **5 segundos**, bem menos que a duração da coleta.

### Corrigido — Layout no SEI 5

- **Links de filtro empilhados no Controle de Processos.** O `#divFiltro` virou uma `row` do Bootstrap; forçar `display: initial` (que computa como `block`) ou `inline-table` destruía o layout flex. A decisão passou a ser tomada **pela classe `.row` do próprio elemento**, não por detecção de versão — a primeira tentativa dependia de `isSEI_5` e o bug reaparecia de forma intermitente.
- **Setas do menu lateral desalinhadas.** `setInfraImg()` envolvia a seta num `<span>`, criando um quarto item flex de ~125px que roubava o espaço do texto (317px → 209px).
- **`siglaUnidadeAtual` duplicada** (ex.: `"ABCABC"`). O `InfraPaginaEsquema3` renderiza a barra do sistema duas vezes, duplicando o ID `#lnkInfraUnidade`.
- **`width: 50%` indevido no `#divFiltro`.**

### Corrigido — Carregamento de scripts

Quatro defeitos com a mesma raiz: `$.getScript` é assíncrono e o projeto disparava dependências em paralelo, sem encadear.

- **`checkHostLimit is not defined`.** `initSeiPro()` chamava a função antes de o `sei-functions-pro.js` (~1,2 MB) terminar de carregar; o `ReferenceError` abortava a inicialização.
- **`$(...).resizable is not a function`** — a barra que redimensiona a árvore parava de funcionar. O `init.js` **recarregava o jQuery**, substituindo a instância à qual o jQuery UI havia se acoplado. O recarregamento era logicamente redundante: a própria linha usava `$.getScript`, então o jQuery já existia.
- **`Moment Duration Format cannot find Moment.js`.** O plugin (4 KB) vencia a corrida contra o `moment` (53 KB).
- **`Cannot read properties of null (reading 'NAMESPACE_SPRO')`.** Quatro declarações liam propriedades de `parent._P()` sem verificar nulo — sendo que a linha vizinha já fazia essa verificação. Por estar no escopo do módulo, abortava o arquivo inteiro.

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

[Não lançado]: https://github.com/rafaelfariasbsb/sei-pro/compare/v1.6.3...HEAD
[1.6.3]: https://github.com/rafaelfariasbsb/sei-pro/compare/v1.6.2...v1.6.3
[1.6.2]: https://github.com/rafaelfariasbsb/sei-pro/compare/v1.6.1...v1.6.2
