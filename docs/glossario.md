# Glossário

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

---

## Termos do Domínio SEI

**SEI — Sistema Eletrônico de Informações**  
Sistema de gestão de processos e documentos administrativos digitais, desenvolvido pelo Tribunal Regional Federal da 4ª Região (TRF4) e distribuído gratuitamente pelo Ministério da Gestão e da Inovação em Serviços Públicos. Utilizado por centenas de órgãos públicos brasileiros. O código-fonte é mantido pelo SERPRO.

**Processo**  
Unidade principal de trabalho no SEI. Um processo agrupa documentos relacionados a uma mesma demanda administrativa. Identificado por um número único no formato `NNNNN.NNNNNN/AAAA-DD`.

**Documento**  
Arquivo digital contido em um processo. Pode ser nato-digital (criado no SEI) ou digitalizado (escaneado de papel). Tipos comuns: ofício, memorando, despacho, relatório, formulário.

**Árvore de Documentos**  
Estrutura hierárquica exibida no painel esquerdo da tela de trabalho do processo, listando todos os documentos e suas ações associadas.

**Unidade**  
Setor organizacional no SEI. Processos e documentos são tramitados entre unidades.

**Marcador**  
Etiqueta colorida que pode ser atribuída a processos para organização visual e filtragem.

**Ponto de Controle**  
Marcação de etapa de acompanhamento que pode ser atribuída a processos.

**Bloco de Assinatura**  
Mecanismo do SEI para coletar assinaturas digitais de múltiplos usuários em um documento.

**Legística**  
Técnica de elaboração de atos normativos (leis, decretos, portarias, etc.) seguindo regras formais de redação, numeração e estruturação. A Legística brasileira é regulada pelo Decreto nº 9.191/2017.

**LGPD — Lei Geral de Proteção de Dados**  
Lei nº 13.709/2018, que estabelece regras sobre coleta, armazenamento, tratamento e compartilhamento de dados pessoais no Brasil.

**Sigilo / Documento Sigiloso**  
Restrição de acesso aplicada a documentos com informações sensíveis, conforme legislação específica. A extensão oferece recursos para marcar e gerar certidões de sigilo.

**ParticiPEN**  
Plataforma de participação da comunidade do Processo Eletrônico Nacional, onde usuários e desenvolvedores do SEI trocam experiências e sugestões.

---

## Termos Técnicos da Extensão

**Extensão de Navegador**  
Programa que adiciona funcionalidades a um navegador web. No contexto do SEI Pro, a extensão injeta JavaScript e CSS nas páginas do SEI para adicionar recursos sem modificar o servidor.

**Manifest V3 (MV3)**  
Versão atual do padrão de desenvolvimento de extensões para Chrome, Edge e Firefox. Define a estrutura, permissões e APIs disponíveis para extensões. Substituiu o Manifest V2, que entrou em processo de deprecação em 2023.

**Content Script**  
Script JavaScript que a extensão injeta nas páginas web visitadas pelo usuário. Opera no contexto da página (acessa o DOM) mas em um ambiente isolado do JavaScript da própria página.

**Service Worker / Background Script**  
Componente da extensão que roda em segundo plano, sem acesso ao DOM. Gerencia o ciclo de vida da extensão, comunicação entre componentes e tarefas em background (ex: verificar contador de processos).

**chrome.storage.local**  
API do navegador para armazenamento persistente da extensão. Similar ao `localStorage`, mas sincronizada entre abas e disponível para o service worker. Usada para configurações globais da extensão.

**localStorage**  
Armazenamento web persistente disponível por domínio. A extensão usa o `localStorage` do domínio do SEI para armazenar dados como favoritos e prazos.

**sessionStorage**  
Armazenamento web temporário (limpo ao fechar a aba). Usado para cache de dados da sessão atual, como a versão detectada do SEI e histórico de processos visitados.

**DOM — Document Object Model**  
Representação em árvore do HTML de uma página, manipulável via JavaScript. A extensão lê e modifica o DOM do SEI para adicionar elementos e funcionalidades.

**iframe**  
Elemento HTML que embute outra página web dentro de uma página. O SEI usa iframes para estruturar sua interface (árvore de documentos, visualizador, editor). Cada iframe tem seu próprio contexto JavaScript.

**Content Script por iframe**  
Scripts injetados especificamente no contexto de um iframe, permitindo à extensão operar dentro de iframes específicos do SEI (ex: `init_arvore.js` opera dentro do iframe da árvore de documentos).

**Vendorização (vendor)**  
Prática de copiar bibliotecas de terceiros diretamente para o repositório do projeto, sem uso de gerenciador de pacotes. Garante que a extensão funcione sem conexão com NPM ou CDN, mas requer atualização manual das dependências.

**DOMPurify**  
Biblioteca de sanitização de HTML que remove código malicioso (XSS) de strings HTML antes de inseri-las no DOM. Usada pela extensão para prevenir injeção de scripts maliciosos.

**isNewSEI**  
Flag booleana interna da extensão que indica se a interface do SEI usa o layout moderno com sidebar menu (versões 4.x+). Detectada pela presença do elemento `#divInfraSidebarMenu ul#infraMenu` no DOM.

**isSEI_5**  
Flag booleana interna da extensão que indica se a versão do SEI é 5.x ou superior. Combinação de `isNewSEI` com comparação do número de versão detectado via DOM.

**Selector / Seletor CSS**  
String que identifica elementos HTML no DOM (ex: `#divInfraAreaTelaE`, `.infraCheckbox`). A extensão usa seletores específicos do SEI que podem variar entre versões.

**CKEditor 5**  
Editor de texto rico (WYSIWYG) de código aberto, adotado pelo SEI 5 como editor de documentos. Substituiu o editor anterior, exigindo adaptação da extensão nas funcionalidades do módulo de edição.

**jQuery**  
Biblioteca JavaScript que simplifica manipulação de DOM, eventos e AJAX. Amplamente usada tanto no SEI quanto na extensão SEI Pro.

**Mermaid**  
Ferramenta de diagramação baseada em texto, com sintaxe semelhante a Markdown. Permite criar diagramas de sequência, fluxo e estados em arquivos `.md`, renderizados automaticamente pelo GitHub.

**ADR — Architecture Decision Record**  
Documento que registra uma decisão arquitetural importante: o contexto, a decisão tomada, as alternativas consideradas e as consequências esperadas. Ver pasta `docs/adr/`.

**Conventional Commits**  
Convenção para mensagens de commit que facilita geração automática de changelogs e comunicação entre contribuidores. Formato: `tipo(escopo): descrição`. Tipos comuns: `fix`, `feat`, `docs`, `refactor`, `chore`.

**Keep a Changelog**  
Convenção para manutenção de arquivos CHANGELOG.md, com seções padronizadas: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.

**OCR — Optical Character Recognition**  
Tecnologia de reconhecimento de caracteres em imagens. A extensão usa Tesseract.js (implementação JavaScript) para extrair texto de imagens inseridas em documentos do SEI.

**Gantt**  
Tipo de gráfico de barras horizontais usado para representar cronogramas. A extensão usa a biblioteca `frappe-gantt` para visualizar prazos e cronogramas de projetos.

**Kanban**  
Método de gestão visual de trabalho usando quadro com colunas (ex: "A fazer", "Em andamento", "Concluído"). A extensão usa a biblioteca `jKanban` para o módulo de Projetos.

---

## Siglas

| Sigla | Significado |
|---|---|
| SEI | Sistema Eletrônico de Informações |
| SIP | Sistema de Inteligência de Pessoas (integrado ao SEI) |
| SERPRO | Serviço Federal de Processamento de Dados |
| TRF4 | Tribunal Regional Federal da 4ª Região |
| LGPD | Lei Geral de Proteção de Dados (Lei nº 13.709/2018) |
| MV3 | Manifest Version 3 (padrão de extensões de navegador) |
| DOM | Document Object Model |
| API | Application Programming Interface |
| XSS | Cross-Site Scripting (tipo de vulnerabilidade web) |
| CVE | Common Vulnerabilities and Exposures |
| ADR | Architecture Decision Record |
| ERS | Especificação de Requisitos de Software |
| OCR | Optical Character Recognition |
| CSV | Comma-Separated Values (formato de planilha texto) |
| UUID | Universally Unique Identifier |
