# SEI Pro — Fork Comunitário ![SEI Pro](/img/icon-32.png)

> **Fork comunitário** do [SEI Pro original](https://github.com/SEI-Pro/sei-pro), mantido pela comunidade de usuários do SEI.
> O projeto original está sem atualizações desde 2023. Este fork tem como objetivo principal a **compatibilidade com o SEI 5** e a continuidade das funcionalidades existentes.

**SEI Pro** adiciona ao [Sistema Eletrônico de Informações (SEI)](https://softwarepublico.gov.br/social/sei) diversas funções avançadas na página inicial e no editor de textos.

| Versão do SEI | Status | Como foi verificado |
|---|---|---|
| SEI 5.0.x | ✅ Funcional | testado em instância **5.0.3** e em ambiente local **5.0.0** |
| SEI 4.1 | ✅ Funcional | testado em instância **4.1** em produção |
| SEI 4.0 | 🔲 Não testado | nenhuma correção é específica de 4.1; sem validação prática |

> Detalhes por funcionalidade: [Matriz de Compatibilidade](./docs/matriz-compatibilidade.md) · Histórico completo: [CHANGELOG](./CHANGELOG.md)

**Limitações conhecidas** — vale ler antes de instalar:

- O **editor de documentos** ainda não foi adaptado ao **CKEditor 5**. O SEI 5 mantém os dois editores e escolhe por documento (configurável por unidade), então onde o CK4 estiver em uso o editor funciona normalmente.
- O domínio `seipro.app`, usado pelo projeto original, foi **desativado**. Isso afeta a descoberta automática do servidor do **Controle de Prazos** — a configuração manual segue disponível nas opções — e a **Legística**.

---

## Como instalar

A extensão ainda **não está publicada nas lojas** (é o próximo passo do projeto). Por enquanto a instalação é manual, e leva cerca de dois minutos.

### Chrome, Edge, Brave e outros baseados em Chromium

1. **Baixe** o arquivo `sei-pro-<versão>.zip` da [última release](https://github.com/rafaelfariasbsb/sei-pro/releases/latest)
2. **Extraia** o ZIP numa pasta definitiva — por exemplo `Documentos\sei-pro`.
   ⚠️ **Não apague nem mova essa pasta depois.** Diferente das extensões de loja, o navegador lê os arquivos direto dali toda vez que inicia; se a pasta sumir, a extensão para de funcionar.
3. Abra `chrome://extensions` (no Edge, `edge://extensions`)
4. Ative o **Modo do desenvolvedor** — no Chrome fica no canto superior direito; no Edge, na barra lateral esquerda
5. Clique em **Carregar sem compactação** e selecione **a pasta extraída** (aquela que contém o arquivo `manifest.json` — não entre em subpastas)
6. O card **SEI Pro Lab** deve aparecer na lista. Abra o SEI e recarregue a página com **Ctrl+F5**

### Firefox

1. Baixe e extraia o ZIP da [última release](https://github.com/rafaelfariasbsb/sei-pro/releases/latest)
2. Abra `about:debugging#/runtime/this-firefox`
3. Clique em **Carregar extensão temporária** e selecione o arquivo `manifest.json` dentro da pasta

> No Firefox a instalação é **temporária**: a extensão é removida ao fechar o navegador e precisa ser carregada de novo. É limitação do próprio Firefox para extensões não assinadas.

### Já usa o SEI Pro original? Desative antes

Se você tem a versão original instalada pela loja, **desative-a** antes de usar este fork. As duas atuam na mesma página do SEI e podem entrar em conflito, gerando comportamento imprevisível.

Em `chrome://extensions`, basta desligar a chave do card da extensão original — não é preciso remover.

### Como atualizar para uma versão nova

1. Baixe o ZIP da nova release e extraia **por cima** da pasta existente, substituindo os arquivos
2. Em `chrome://extensions`, clique no ícone **↻** no card da extensão
3. Recarregue a aba do SEI com **Ctrl+F5**

Confira a versão no próprio card (ex.: **SEI Pro Lab 1.6.3**) para ter certeza de que a atualização foi aplicada.

### Se algo não funcionar

1. Confirme que a versão no card é a que você acabou de instalar
2. Dê **Ctrl+F5** na aba do SEI — a extensão só é injetada quando a página carrega
3. Em `chrome://extensions`, veja se o card mostra o botão **Erros**; se mostrar, o conteúdo dele ajuda muito no diagnóstico
4. [Abra uma issue](https://github.com/rafaelfariasbsb/sei-pro/issues) informando **a versão do SEI** (aparece ao lado do logo), o navegador e o que apareceu em Erros

---

### Lojas oficiais (versão original — SEI 4.x apenas)

> As lojas abaixo distribuem a versão **original** do projeto, que está sem atualizações desde 2023 e **não tem suporte ao SEI 5**. Não são este fork.

<img src="https://edent.github.io/SuperTinyIcons/images/svg/chrome.svg" width="24" title="Chrome"> [Chrome Web Store](https://chrome.google.com/webstore/detail/sei-pro/pdbbapplhjopafpgidbgceccbbmehcjj)

<img src="https://edent.github.io/SuperTinyIcons/images/svg/edge.svg" width="24" title="Edge"> [Microsoft Edge Add-ons](https://microsoftedge.microsoft.com/addons/detail/sei-pro/gkhfbbbminanojfklpfmloaglckmlfne)

<img src="https://edent.github.io/SuperTinyIcons/images/svg/firefox.svg" width="24" title="Firefox"> [Firefox Add-ons](https://addons.mozilla.org/pt-BR/firefox/addon/sei-pro/)

## Funcionalidades disponíveis

- ![Estilo Avançado](/img/icon-estiloavancado.png) [Alterar o layout do SEI (Estilo Avançado + Modo Noturno)](./pages/ESTILOAVANCADO.md)
- ![Estilo Tabela](/img/icon-projetos.png) [Gerenciar projetos](./pages/PROJETOS.md) [MOMENTANEAMENTE DESCONTINUADA]
- ![Favoritos](/img/icon-favoritos.png) [Gerenciar processos favoritos](./pages/FAVORITOS.md)
- ![Gerenciar Prazos](/img/icon-controleprazo.png) [Controle de Prazos](./pages/PRAZOS.md)
- ![Enumerar Normas (Legística)](/img/icon-legistica.png) [Enumerar Normas (Legística)](./pages/LEGISTICA.md)
- ![Agrupar lista](/img/icon-agruparlista.png) [Agrupar  lista de processos por data de recebimento, envio, último acesso, marcadores, tipo, responsável, ponto de controle, unidade de envio e acompanhamento especial](./pages/AGRUPAR.md)
- ![Inserir Documento Externo (HTML)](/img/icon-inserirhtml.png) [Inserir documento externo (HTML, Google Docs e Google Planilhas)](./pages/INSERIRDOC.md)
- ![Estilo Tabela](/img/icon-estilotabela.png) [Adicionar estilo a tabela](./pages/ESTILOTABELA.md)
- ![Link de Legislação](/img/icon-linklegis.png) [Adicionar link de legislação](./pages/LINKLEGIS.md)
- ![Letras Maiúsculas](/img/icon-letramaiusc.png) [Primeira letra maiúscula (exceto artigos e preposições)](./pages/LETRAMAIUSC.md)
- ![Referência Documentos](/img/icon-refdocumentos.png) [Inserir referência de documentos do processo](./pages/REFDOCUMENTOS.md)
- ![Nota Rodapé](/img/icon-notarodape.png) [Inserir nota de rodapé](./pages/NOTARODAPE.md)
- ![Sumário](/img/icon-sumario.png) [Inserir sumário](./pages/SUMARIO.md)
- ![Dados do Processo](/img/icon-dadosprocesso.png) [Inserir dados do processo](./pages/DADOSPROCESSO.md)
- ![Link Curto](/img/icon-linkcurto.png) [Gerar link curto do TinyUrl](./pages/LINKCURTO.md)
- ![Código QR (QRCode)](/img/icon-qrcode.png) [Gerar código QR (QRCode)](./pages/QRCODE.md)
- ![Redimensionar Imagens](/img/icon-redimensionaimg.png) [Redimensionar imagens](./pages/REDIMENSIONAIMG.md)
- ![Quebra de Página](/img/icon-quebrapagina.png) [Inserir quebra de página](./pages/QUEBRAPAGINA.md)
- ![Título da página](/img/icon-titulopagina.png) [Alterar título da página](./pages/TITULOPAGINA.md)
- ![Abrir, editar e remover hiperlinks](/img/icon-abrirlink.png) [Abrir, editar e remover hiperlinks](./pages/ABRIRLINKS.md)
- ![Inserir Equações](/img/icon-equacoes.png) [Inserir equações (fórmulas matemáticas)](./pages/EQUACOES.md)
- ![Menu rápido na árvore de documentos](/img/icon-menurapido.png) [Menu e ícones rápidos na árvore de documentos](./pages/MENURAPIDO.md)
- ![Tabela Rápida](/img/icon-tabelarapida.png) [Tabela rápida](./pages/TABELARAPIDA.md)
- ![Copiar formatação de texto](/img/icon-copiarformatacao.png) [Copiar formatação do texto](./pages/COPIARFORMATACAO.md)
- ![Aumentar fonte](/img/icon-aumentarfonte.png) [Aumentar ou reduzir o tamanho da fonte](./pages/AUMENTARFONTE.md)
- ![Alinhar texto](/img/icon-alinhartexto.png) [Alinhar o texto à esquerda, ao centro, à direita ou justificadamente](./pages/ALINHARTEXTO.md)
- ![Verificar Integridade Hashcode](/img/icon-hashcode.png) [Verificar código de integridade (Hashcode)](./pages/HASHCODE.md)
- ![Adicionar link documento público](/img/icon-docpublico.png) [Adicionar link de documento público](./pages/DOCPUBLICO.md)
- ![Adicionar valores padronizados](/img/icon-valdefault.png) [Adicionar valores padronizados ao criar um novo documento](./pages/VALDEFAULT.md)
- ![Alinhar texto](/img/icon-linkpermanente.png) [Pesquisar link permanente](./pages/LINKPERMANENTE.md)
- ![Aumentar fonte](/img/icon-playvideo.png) [Reproduzir vídeo na visualização de documentos](./pages/PLAYVIDEO.md)
- ![Alinhar texto](/img/icon-listaprocessos.png) [Exportar informações de processos em planilha CSV](./pages/LISTAPROCESSOS.md)
- ![Alinhar texto](/img/icon-marcaminuta.png) [Adicionar marca d'água de minuta ao documento](./pages/MARCAMINUTA.md)
- ![Sigilo Documento](/img/icon-sigilodoc.png) [Adicionar marca de sigilo e tarjas pretas de confidencialidade (LGPD)](./pages/SIGILODOC.md)
- ![Duplicar Documento](/img/icon-duplicardoc.png) [Duplicar documentos com 1 click](./pages/DUPLICARDOC.md)
- ![Enviar documentos](/img/icon-uploaddocs.png) [Enviar múltiplos documentos externos](./pages/UPLOADDOCS.md)
- ![Menu Suspenso](/img/icon-menususpenso.png) [Menu Suspenso](./pages/MENUSUSPENSO.md)
- ![Ordenar Tabelas](/img/icon-ordernartabela.png) [Ordenar tabelas ao clicar no seu cabeçalho](./pages/ORDENARTABELA.md)
- ![Enviar documentos](/img/icon-historicoproc.png) [Histórico de processos visitados](./pages/HISTORICOPROC.md)
- ![Enviar documentos](/img/icon-infoarvore.png) [Informações adicionais na árvore do processo](./pages/INFOARVORE.md)
- ![Enviar documentos](/img/icon-notaarvore.png) [Anotação diretamente pela árvore do processo](./pages/NOTAARVORE.md)
- ![Remover paginação de processos](/img/icon-removerpaginacao.png) [Remover paginação de processos](./pages/REMOVEPAGINACAO.md)
- ![Dividir as informações do documento na árvore do processo em duas linhas](/img/icon-dividirinformacoes.png) [Dividir as informações do documento na árvore do processo em duas linhas](./pages/DIVIDIRLINHASARVORE.md)
- ![SEI Redimensionar automaticamente a árvore do processo pela sua largura total](/img/icon-resizearvore.png) [Redimensionar automaticamente a árvore do processo pela sua largura total](./pages/RESIZEARVORE.md)
- ![Utilizar caixas de seleção inteligentes](/img/icon-cursor.png) [Utilizar caixas de seleção inteligentes](./pages/SUBSTITUIRSELECAO.md)
- ![Ações em Lote](/img/icon-acoeslote.png) [Ações em Lote](./pages/ACOESEMLOTE.md)
- ![Rolagem Infinita](/img/icon-rolageminfinita.png) [Rolagem infinita na pesquisa de processos](./pages/ROLAGEMINFINITA.md)
- ![URL Amigável](/img/icon-urlamigavel.png) [Utilizar endereços amigáveis em processos e documentos](./pages/URLAMIGAVEL.md)
- ![Documentos não assinados](/img/icon-docsnaoassinados.png) [Alertar sobre documentos não assinados ao enviar um processo](./pages/DOCSNAOASSINADOS.md)
- ![Cores marcadores](/img/icon-coresmarcadores.png) [Permitir cores personalizadas em Marcadores](./pages/CORESMARCADORES.md)
- ![Parágrafos Numerados](/img/icon-paragrafosnumerados.png) [Visualizar parágrafos numerados no visualizador de documentos](./pages/PARAGRAFOSNUMERADOS.md)
- ![Certidão Sigilo](/img/icon-certidaosigilo.png) [Gerar Certidão de Documento Oficial com Sigilo (LGPD)](./pages/CERTIDAOSIGILO.md)
- ![Editar Imagens](/img/icon-editarimagens.png) [Enviar múltiplas imagens, formatar e editar opções avançadas](./pages/EDITARIMAGENS.md)
- ![Qualidade Imagens](/img/icon-qualidadeimagens.png) [Reduzir a qualidade das imagens inseridas nos documentos](./pages/QUALIDADEIMAGENS.md)
- ![Teclas Atalho](/img/icon-teclasatalho.png) [Adicionar teclas de atalhos no editor de documentos](./pages/TECLASATALHO.md)
- ![Referencia Interna](/img/icon-referenciainterna.png) [Adicionar referências internas](./pages/REFERENCIAINTERNA.md)
- ![Ferramentas IA](/img/icon-ferramentasia.png) [Ferramentas de Inteligência Artificial (ChatGPT)](./pages/FERRAMENTASIA.md)
- ![Mover ícone de excluir](/img/icon-movericone.png) [Mover ícone de excluir documentos para o final da lista](./pages/MOVERICONE.md)
- ![Autopreencher senha](/img/icon-autopreenchersenha.png) [Autopreencher senha no login (SEI >= 4.0)](./pages/AUTOPREENCHERSENHA.md)
- ![Numerar documentos](/img/icon-numerardocsarvore.png) [Numerar documentos na árvore do processo](./pages/NUMERARDOCSARVORE.md)
- ![Contador de processos não recebidos](/img/icon-contadorprocessoicone.png) [Contador de processos não recebidos no ícone do SEI](./pages/CONTADORPROCESSOICONE.md)
- ![Mostrar especificação do processo](/img/icon-especificacaoprocesso.png) [Mostrar especificação do processo na tabela de controle de processos](./pages/ESPECIFICACAOPROCESSO.md)
- ![Mostrar nomes de usuários](/img/icon-nomesusuarios.png) [Mostrar nomes de usuários na tabela de controle de processos](./pages/NOMESUSUARIOS.md)
- ![Marcar processo não visualizado](/img/icon-naolido.png) [Permitir marcar processos como "Não Visualizado"](./pages/NAOLIDO.md)
- ![Comparador de Documentos](/img/icon-comparardocumentos.png) [Comparador de Documentos](./pages/COMPARARDOCUMENTOS.md)
- ![Reabertura programada de processos](/img/icon-reabrirprocessos.png) [Reabertura programada de processos](./pages/REABRIRPROCESSOS.md)
- ![Ditado no editor de documentos](/img/icon-ditado.png) [Ditado no editor de documentos](./pages/DITADO.md)
- ![Escrita interativa](/img/icon-escritainterativa.png) [Escrita interativa no editor de documentos](./pages/ESCRITAINTERATIVA.md)
- ![Revisão de texto](/img/icon-revisardoc.png) [Revisão de texto no editor de documentos](./pages/REVISARDOC.md)
- ![Documentos em Lote](/img/icon-acoeslote.png) [Documentos em Lote](./pages/DOCUMENTOSEMLOTE.md)


Você ainda pode [Desativar funções da extensão](./pages/DESATIVARFUNCOES.md) que não deseja utilizar.

## Encontrou um erro?

Abra uma [issue](https://github.com/rafaelfariasbsb/sei-pro/issues) informando:
- Versão do SEI
- Navegador e versão
- Descrição do problema e passos para reproduzir

## Documentação Técnica

A pasta [`docs/`](./docs/) contém toda a documentação de engenharia de software e análise de sistema do projeto.

**Engenharia de Software**

| Documento | Descrição |
|---|---|
| [Especificação de Requisitos](./docs/requisitos.md) | Requisitos funcionais e não-funcionais por módulo |
| [Casos de Uso](./docs/casos-de-uso.md) | Fluxos principais e exceções dos casos de uso |
| [Plano de Testes](./docs/plano-testes.md) | Estratégia de testes e critérios de release |
| [Gestão de Riscos](./docs/gestao-riscos.md) | Registro de riscos e planos de mitigação |

**Análise de Sistema**

| Documento | Descrição |
|---|---|
| [Arquitetura](./docs/arquitetura.md) | Componentes, contextos de injeção e fluxo de inicialização |
| [Modelo de Domínio](./docs/modelo-dominio.md) | Entidades, dicionário de dados e regras de negócio |
| [Diagramas](./docs/diagramas.md) | Diagramas de componentes, sequência, estados e fluxo de dados |
| [Seletores DOM do SEI](./docs/seletores-sei.md) | Seletores por versão do SEI (referência para SEI 5) |
| [Matriz de Compatibilidade](./docs/matriz-compatibilidade.md) | Status de cada funcionalidade por versão e navegador |

**Módulos**

| Documento | Módulo |
|---|---|
| [Visão geral dos módulos](./docs/modulos/README.md) | Índice e contextos de injeção |
| [sei-functions-pro](./docs/modulos/sei-functions-pro.md) | Núcleo compartilhado |
| [sei-pro](./docs/modulos/sei-pro.md) | Página principal — lista de processos |
| [sei-pro-editor](./docs/modulos/sei-pro-editor.md) | Editor de documentos |
| [sei-pro-arvore](./docs/modulos/sei-pro-arvore.md) | Árvore do processo |
| [sei-pro-atividades](./docs/modulos/sei-pro-atividades.md) | Controle de prazos |
| [sei-pro-ai](./docs/modulos/sei-pro-ai.md) | Integração com IA / ChatGPT |

**Planejamento e Referência**

| Documento | Descrição |
|---|---|
| [Roadmap](./docs/roadmap.md) | Fases do projeto, entregas e critérios de versão |
| [Especificação SEI 5](./docs/especificacao-sei5.md) | O que mudou no SEI 5 e plano de adaptação por módulo |
| [Padrões de Código](./docs/padroes-codigo.md) | Convenções e boas práticas do projeto |
| [Glossário](./docs/glossario.md) | Termos do domínio SEI e termos técnicos da extensão |
| [Guia de Desenvolvimento](./docs/desenvolvimento.md) | Como montar o ambiente, depurar e adicionar funcionalidades |

**Decisões Arquiteturais (ADR)**

| Documento | Decisão |
|---|---|
| [ADR-001](./docs/adr/001-sem-build-system.md) | Ausência de sistema de build |
| [ADR-002](./docs/adr/002-google-sheets-backend.md) | Google Sheets como backend remoto |
| [ADR-003](./docs/adr/003-estrategia-sei5.md) | Estratégia de compatibilidade com SEI 5 |

## Como contribuir

Este é um projeto comunitário aberto. Veja o [Guia de Contribuição](./CONTRIBUTING.md) para saber como ajudar — mesmo sem programar.

## SEI Pro no ParticiPEN

Participe da Comunidade do Processo Eletrônico Nacional (ParticiPEN):
[https://participen.processoeletronico.gov.br/c/modulos-comunidade/sei-pro/39](https://participen.processoeletronico.gov.br/c/modulos-comunidade/sei-pro/39)

## Histórico de versões

Confira o [CHANGELOG](./CHANGELOG.md).

## Licença

[AGPL-3.0](./LICENSE.txt) — fork do projeto original criado por Pedro Henrique Soares.

## Política de Privacidade

Esta extensão não coleta dados dos usuários. Veja a [Política de Privacidade](./PRIVACY_POLICY.md) completa.

