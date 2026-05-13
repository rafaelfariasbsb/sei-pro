# Especificação de Requisitos de Software (ERS)

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13  
**Referência:** IEEE Std 830-1998 (adaptado)

---

## 1. Introdução

### 1.1 Propósito

Este documento especifica os requisitos funcionais e não-funcionais do SEI Pro, uma extensão de navegador que aprimora o Sistema Eletrônico de Informações (SEI) com funcionalidades avançadas. Destina-se a desenvolvedores, contribuidores e mantenedores do projeto.

### 1.2 Escopo

O SEI Pro atua como uma camada de aprimoramento sobre o SEI, injetando código JavaScript nas páginas do sistema para adicionar funcionalidades que não existem nativamente. A extensão **não** modifica o servidor do SEI nem seus dados — opera exclusivamente no lado do cliente (navegador).

### 1.3 Definições

Veja o [Glossário](./glossario.md) para definições completas de termos técnicos e do domínio SEI.

### 1.4 Visão geral do sistema hospedeiro

O SEI é um sistema PHP server-rendered, com frontend jQuery + Bootstrap 4/5 (versão 5.x). Utiliza múltiplos iframes em uma única página e roteamento via parâmetro `?acao=`. A extensão depende da estabilidade dos elementos DOM expostos pelo SEI — qualquer alteração no HTML do sistema impacta diretamente as funcionalidades da extensão.

---

## 2. Requisitos Funcionais

Os requisitos são organizados por módulo funcional, seguindo a estrutura modular da extensão.

### RF-01: Módulo de Layout e Interface

| ID | Requisito | Prioridade |
|---|---|---|
| RF-01.1 | A extensão deve permitir aplicar um tema visual alternativo (Estilo Avançado) ao SEI | Alta |
| RF-01.2 | A extensão deve oferecer modo noturno (dark mode) para todas as páginas do SEI | Alta |
| RF-01.3 | A extensão deve redimensionar automaticamente a árvore de documentos conforme o conteúdo | Média |
| RF-01.4 | A extensão deve oferecer menu flutuante/suspenso alternativo ao menu lateral | Média |
| RF-01.5 | A extensão deve converter URLs do SEI em formato amigável (legível) | Média |
| RF-01.6 | A extensão deve dividir as informações do documento na árvore em duas linhas quando necessário | Baixa |

### RF-02: Módulo de Processos

| ID | Requisito | Prioridade |
|---|---|---|
| RF-02.1 | O usuário deve poder marcar processos como favoritos e acessá-los rapidamente | Alta |
| RF-02.2 | A extensão deve permitir agrupar a lista de processos por diferentes critérios (data, tipo, responsável, marcador, etc.) | Alta |
| RF-02.3 | A extensão deve exibir histórico de processos visitados recentemente | Alta |
| RF-02.4 | A extensão deve permitir rolagem infinita na pesquisa de processos, eliminando paginação | Média |
| RF-02.5 | A extensão deve permitir exportar a lista de processos para arquivo CSV | Média |
| RF-02.6 | O usuário deve poder marcar processos como "Não Visualizado" manualmente | Média |
| RF-02.7 | A extensão deve exibir contador de processos não recebidos no ícone da extensão | Média |
| RF-02.8 | A extensão deve alertar sobre documentos não assinados ao tentar enviar um processo | Alta |
| RF-02.9 | A extensão deve permitir reabertura programada de processos em data/hora futura | Baixa |
| RF-02.10 | A extensão deve permitir cores personalizadas em marcadores de processos | Baixa |

### RF-03: Módulo de Controle de Prazos

| ID | Requisito | Prioridade |
|---|---|---|
| RF-03.1 | A extensão deve permitir cadastrar prazos associados a processos | Alta |
| RF-03.2 | A extensão deve exibir os prazos em formato de gráfico de Gantt | Alta |
| RF-03.3 | A extensão deve destacar visualmente prazos vencidos ou próximos do vencimento | Alta |
| RF-03.4 | Os dados de prazos devem persistir entre sessões do navegador | Alta |

### RF-04: Módulo do Editor de Documentos

| ID | Requisito | Prioridade |
|---|---|---|
| RF-04.1 | A extensão deve permitir inserir notas de rodapé em documentos | Alta |
| RF-04.2 | A extensão deve gerar sumário automático com base nos títulos do documento | Alta |
| RF-04.3 | A extensão deve permitir inserir quebras de página | Alta |
| RF-04.4 | A extensão deve permitir inserir equações matemáticas (LaTeX/MathML) | Média |
| RF-04.5 | A extensão deve oferecer ferramenta de copiar/colar formatação de texto | Média |
| RF-04.6 | A extensão deve permitir inserir dados do processo automaticamente no documento | Alta |
| RF-04.7 | A extensão deve adicionar marca d'água de "minuta" em documentos em elaboração | Média |
| RF-04.8 | A extensão deve permitir adicionar marcas de sigilo e tarjas de confidencialidade (LGPD) | Alta |
| RF-04.9 | A extensão deve salvar automaticamente o documento em intervalos regulares | Alta |
| RF-04.10 | A extensão deve permitir ditado de texto por voz no editor | Média |
| RF-04.11 | A extensão deve oferecer parágrafos numerados no visualizador de documentos | Baixa |
| RF-04.12 | A extensão deve permitir adicionar referências internas entre partes do documento | Média |
| RF-04.13 | A extensão deve oferecer atalhos de teclado configuráveis no editor | Média |
| RF-04.14 | A extensão deve permitir inserção de tabela rápida com dimensões configuráveis | Média |
| RF-04.15 | A extensão deve permitir abrir, editar e remover hiperlinks | Alta |

### RF-05: Módulo de Documentos

| ID | Requisito | Prioridade |
|---|---|---|
| RF-05.1 | A extensão deve permitir duplicar documentos com um único clique | Alta |
| RF-05.2 | A extensão deve permitir envio de múltiplos documentos externos simultaneamente | Alta |
| RF-05.3 | A extensão deve permitir inserir documentos HTML, Google Docs e Google Planilhas | Média |
| RF-05.4 | A extensão deve oferecer editor de imagens integrado | Média |
| RF-05.5 | A extensão deve permitir redimensionar e reduzir qualidade de imagens | Média |
| RF-05.6 | A extensão deve realizar OCR em imagens inseridas nos documentos | Baixa |
| RF-05.7 | A extensão deve permitir comparar duas versões de um documento (diff) | Média |
| RF-05.8 | A extensão deve gerar Certidão de Documento com Sigilo (LGPD) | Alta |
| RF-05.9 | A extensão deve gerar QR Code de acesso ao documento | Baixa |
| RF-05.10 | A extensão deve verificar a integridade de documentos por código hash | Média |
| RF-05.11 | A extensão deve permitir ações em lote sobre múltiplos documentos | Alta |
| RF-05.12 | A extensão deve exibir menu rápido de ações na árvore de documentos | Alta |

### RF-06: Módulo de Inteligência Artificial

| ID | Requisito | Prioridade |
|---|---|---|
| RF-06.1 | A extensão deve integrar com a API da OpenAI (ChatGPT) usando chave configurada pelo usuário | Média |
| RF-06.2 | A extensão deve oferecer prompts predefinidos para: resumo, reescrita, pesquisa jurídica e tradução | Média |
| RF-06.3 | A extensão deve permitir ao usuário criar e salvar prompts personalizados | Baixa |

### RF-07: Módulo Jurídico / Legística

| ID | Requisito | Prioridade |
|---|---|---|
| RF-07.1 | A extensão deve numerar automaticamente normas jurídicas seguindo as regras da Legística | Média |
| RF-07.2 | A extensão deve criar links automáticos para legislação em portais como Planalto e Legisweb | Média |
| RF-07.3 | A extensão deve aplicar capitalização correta conforme regras gramaticais do português | Baixa |

### RF-08: Módulo de Projetos

| ID | Requisito | Prioridade |
|---|---|---|
| RF-08.1 | A extensão deve permitir gerenciar projetos em quadro Kanban | Baixa |
| RF-08.2 | A extensão deve exibir cronograma de projetos em gráfico de Gantt | Baixa |
| RF-08.3 | A extensão deve suportar backend remoto via Google Sheets para dados compartilhados | Baixa |

### RF-09: Requisitos de Configuração

| ID | Requisito | Prioridade |
|---|---|---|
| RF-09.1 | O usuário deve poder ativar/desativar individualmente cada funcionalidade da extensão | Alta |
| RF-09.2 | As configurações devem persistir entre sessões e atualizações da extensão | Alta |
| RF-09.3 | A extensão deve oferecer página de opções acessível pelo ícone no navegador | Alta |
| RF-09.4 | A extensão deve suportar preenchimento automático de credenciais na tela de login | Média |

---

## 3. Requisitos Não-Funcionais

### RNF-01: Compatibilidade

| ID | Requisito |
|---|---|
| RNF-01.1 | A extensão deve funcionar no Google Chrome versão 100 ou superior |
| RNF-01.2 | A extensão deve funcionar no Microsoft Edge versão 100 ou superior |
| RNF-01.3 | A extensão deve funcionar no Mozilla Firefox versão 100 ou superior |
| RNF-01.4 | A extensão deve ser compatível com SEI versões 4.0, 4.1 e 5.x |
| RNF-01.5 | A extensão deve seguir o padrão Manifest V3 (MV3) |
| RNF-01.6 | A extensão deve funcionar em instalações do SEI em diferentes domínios (*.gov.br, *.org) |

### RNF-02: Desempenho

| ID | Requisito |
|---|---|
| RNF-02.1 | A inicialização da extensão não deve adicionar mais de 500ms ao tempo de carregamento das páginas do SEI |
| RNF-02.2 | O autossalvamento do editor não deve interromper a digitação do usuário |
| RNF-02.3 | O OCR deve processar imagens de até 5MB sem travar a interface |

### RNF-03: Segurança e Privacidade

| ID | Requisito |
|---|---|
| RNF-03.1 | A extensão não deve transmitir dados de documentos do SEI para servidores externos, exceto quando explicitamente solicitado pelo usuário (ex: IA, Google Sheets) |
| RNF-03.2 | Todo HTML gerado dinamicamente deve ser sanitizado via DOMPurify antes de ser inserido no DOM |
| RNF-03.3 | A extensão deve solicitar apenas as permissões estritamente necessárias |
| RNF-03.4 | Chaves de API (OpenAI, Google) devem ser armazenadas apenas no storage local do navegador |
| RNF-03.5 | A extensão deve estar em conformidade com a LGPD — sem coleta de dados pessoais |

### RNF-04: Manutenibilidade

| ID | Requisito |
|---|---|
| RNF-04.1 | Todo seletor DOM do SEI utilizado pela extensão deve estar registrado em `docs/seletores-sei.md` |
| RNF-04.2 | Código que difere entre versões do SEI deve usar as flags `isSEI_5` e `isNewSEI` |
| RNF-04.3 | Mensagens de commit devem seguir o padrão Conventional Commits |
| RNF-04.4 | Toda nova funcionalidade deve ter documentação de usuário em `pages/` |

### RNF-05: Usabilidade

| ID | Requisito |
|---|---|
| RNF-05.1 | Funcionalidades devem ser acessíveis sem necessidade de treinamento prévio |
| RNF-05.2 | Mensagens de erro devem ser exibidas em português e orientar o usuário sobre como proceder |
| RNF-05.3 | A extensão não deve alterar o fluxo de trabalho nativo do SEI — apenas adicionar opções |

---

## 4. Restrições do Sistema

| ID | Restrição |
|---|---|
| RS-01 | A extensão não pode modificar o servidor SEI — opera exclusivamente no browser |
| RS-02 | A extensão depende da estabilidade dos elementos DOM do SEI; mudanças no sistema host podem quebrar funcionalidades |
| RS-03 | Funcionalidades que requerem backend (Projetos, IA) dependem de serviços externos configurados pelo usuário |
| RS-04 | A extensão não pode acessar o banco de dados do SEI diretamente |
| RS-05 | O Manifest V3 proíbe execução de código remoto — todo JavaScript deve ser empacotado na extensão |

---

## 5. Rastreabilidade de Requisitos

| Requisito | Módulo JS | Documentação |
|---|---|---|
| RF-01.x | `sei-pro-all.js`, `sei-pro.js` | `pages/ESTILOAVANCADO.md` |
| RF-02.x | `sei-pro.js`, `sei-pro-atividades.js` | `pages/FAVORITOS.md`, etc. |
| RF-03.x | `sei-pro-atividades.js` | `pages/PRAZOS.md` |
| RF-04.x | `sei-pro-editor.js` | `pages/NOTARODAPE.md`, etc. |
| RF-05.x | `sei-pro-docs-lote.js`, `sei-pro-arvore.js` | `pages/DUPLICARDOC.md`, etc. |
| RF-06.x | `sei-pro-ai.js` | `pages/FERRAMENTASIA.md` |
| RF-07.x | `sei-legis.js` | `pages/LEGISTICA.md` |
| RF-08.x | `sei-pro-projetos.js` | `pages/PROJETOS.md` |
| RF-09.x | `options.html`, `options.js` | `pages/DESATIVARFUNCOES.md` |
