# Diagramas do Sistema

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

> Diagramas em formato texto (ASCII art e Mermaid). Para renderizar os blocos Mermaid, use GitHub, VS Code com extensão Mermaid ou [mermaid.live](https://mermaid.live).

---

## 1. Diagrama de Componentes

```
┌─────────────────────────────────────────────────────────────────────┐
│                        NAVEGADOR DO USUÁRIO                         │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                      ABA DO SEI                              │   │
│  │                                                              │   │
│  │  ┌──────────────────────────────────────────────────────┐   │   │
│  │  │              PÁGINA PRINCIPAL DO SEI                 │   │   │
│  │  │  ┌─────────────┐         ┌───────────────────────┐   │   │   │
│  │  │  │ ifrArvore   │         │  ifrVisualizacao /    │   │   │   │
│  │  │  │ (árvore de  │         │  ifrConteudoVisual.   │   │   │   │
│  │  │  │  documentos)│         │  (conteúdo/editor)    │   │   │   │
│  │  │  └──────┬──────┘         └──────────┬────────────┘   │   │   │
│  │  └─────────┼────────────────────────────┼───────────────┘   │   │
│  │            │                            │                    │   │
│  │            │ injetado por               │ injetado por       │   │
│  │            ▼                            ▼                    │   │
│  │  ┌─────────────────────────────────────────────────────┐    │   │
│  │  │                  SEI PRO (Extensão)                  │    │   │
│  │  │                                                      │    │   │
│  │  │  ┌──────────────┐  ┌─────────────┐  ┌───────────┐   │    │   │
│  │  │  │ background.js│  │ init_all.js │  │ options   │   │    │   │
│  │  │  │ (sw worker)  │  │ (global)    │  │ .html/.js │   │    │   │
│  │  │  └──────────────┘  └──────┬──────┘  └───────────┘   │    │   │
│  │  │                           │                          │    │   │
│  │  │         ┌─────────────────┼─────────────────┐        │    │   │
│  │  │         ▼                 ▼                 ▼        │    │   │
│  │  │  ┌─────────────┐  ┌─────────────┐  ┌────────────┐   │    │   │
│  │  │  │ init.js     │  │init_arvore  │  │init_visual │   │    │   │
│  │  │  │ sei-pro.js  │  │sei-pro-     │  │sei-pro-    │   │    │   │
│  │  │  │ sei-pro-    │  │arvore.js    │  │editor.js   │   │    │   │
│  │  │  │ favoritos.js│  └─────────────┘  └────────────┘   │    │   │
│  │  │  │ sei-pro-    │                                     │    │   │
│  │  │  │ atividades  │  sei-functions-pro.js (shared)      │    │   │
│  │  │  └─────────────┘                                     │    │   │
│  │  └─────────────────────────────────────────────────────┘    │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌──────────────────────┐    ┌──────────────────────────────────┐   │
│  │   chrome.storage     │    │  localStorage / sessionStorage   │   │
│  │   (configurações)    │    │  (favoritos, prazos, histórico)  │   │
│  └──────────────────────┘    └──────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
              │                                │
              ▼                                ▼
    ┌──────────────────┐            ┌───────────────────┐
    │   API OpenAI     │            │  Google Sheets API │
    │  (IA opcional)   │            │  (Projetos opcion.)│
    └──────────────────┘            └───────────────────┘
```

---

## 2. Diagrama de Sequência — Inicialização da Extensão

```mermaid
sequenceDiagram
    participant Browser as Navegador
    participant SEI as Página SEI
    participant Init as init_all.js
    participant Funcs as sei-functions-pro.js
    participant Storage as chrome.storage

    Browser->>SEI: Carrega página do SEI
    SEI-->>Browser: DOM renderizado
    Browser->>Init: Injeta init_all.js (content script)

    Init->>SEI: querySelector('#divInfraSidebarMenu ul#infraMenu')
    SEI-->>Init: elemento encontrado / null
    Init->>Init: define isNewSEI = true/false

    Init->>Storage: sessionStorage.getItem('versaoSei')
    alt versão em cache
        Storage-->>Init: "5.0.3"
    else sem cache
        Init->>SEI: querySelector('img[title*="Versão"]').title
        SEI-->>Init: "Sistema Eletrônico de Informações - Versão 5.0.3"
        Init->>Storage: sessionStorage.setItem('versaoSei', '5.0.3')
    end

    Init->>Init: define isSEI_5 = compareVersionNumbers('5.0.3','5') >= 0

    Init->>Funcs: carrega sei-functions-pro.js
    Init->>Browser: carrega módulos conforme contexto da página
```

---

## 3. Diagrama de Sequência — Uso do Módulo de IA

```mermaid
sequenceDiagram
    participant U as Usuário
    participant EXT as Extensão (sei-pro-ai.js)
    participant Storage as localStorage
    participant OPENAI as API OpenAI

    U->>EXT: Seleciona texto no documento
    U->>EXT: Clica em "Ferramentas IA"
    EXT->>Storage: getItem('seiPro_config').openaiApiKey
    
    alt API Key não configurada
        Storage-->>EXT: ""
        EXT-->>U: "Configure sua chave de API nas opções"
    else API Key configurada
        Storage-->>EXT: "sk-xxxx..."
        EXT-->>U: Exibe menu de prompts
        U->>EXT: Seleciona prompt "Resumir"
        EXT->>EXT: Monta payload {model, messages: [prompt + texto selecionado]}
        EXT->>OPENAI: POST /v1/chat/completions (com API Key do usuário)
        
        alt Sucesso
            OPENAI-->>EXT: {choices: [{message: {content: "Resumo..."}}]}
            EXT-->>U: Exibe resultado no painel lateral
            U->>EXT: Clica "Aplicar"
            EXT->>EXT: Substitui texto selecionado pelo resultado
        else Erro de autenticação
            OPENAI-->>EXT: HTTP 401
            EXT-->>U: "Chave de API inválida. Verifique as configurações."
        else Timeout
            OPENAI-->>EXT: Timeout após 30s
            EXT-->>U: "Tempo esgotado. Tente novamente."
        end
    end
```

---

## 4. Diagrama de Sequência — Detecção de Versão do SEI

```mermaid
sequenceDiagram
    participant CS as Content Script
    participant DOM as DOM do SEI
    participant SS as sessionStorage

    CS->>SS: getItem('versaoSei')
    
    alt Cache hit
        SS-->>CS: "4.1.5"
        CS->>CS: versao = "4.1.5"
    else Cache miss
        SS-->>CS: null
        CS->>DOM: querySelectorAll('img[title*="Sistema Eletrônico"]')
        
        alt Imagem encontrada
            DOM-->>CS: <img title="...Versão 5.0.3">
            CS->>CS: Extrai "5.0.3" via regex
        else Imagem não encontrada (SEI customizado)
            DOM-->>CS: []
            CS->>DOM: querySelector('#spnSeiVersao') ou alternativas
            DOM-->>CS: elemento ou null
        end
        
        CS->>SS: setItem('versaoSei', versao)
    end
    
    CS->>CS: isNewSEI = DOM.querySelector('#divInfraSidebarMenu') !== null
    CS->>CS: isSEI_5 = isNewSEI && compareVersions(versao, '5') >= 0
```

---

## 5. Diagrama de Estados — Prazo

```mermaid
stateDiagram-v2
    [*] --> Pendente : Criado com dataFim futura

    Pendente --> Próximo : dataFim <= hoje + 3 dias
    Próximo --> Vencido : dataFim < hoje
    Pendente --> Vencido : dataFim < hoje (verificação periódica)
    
    Pendente --> Concluído : Usuário marca como concluído
    Próximo --> Concluído : Usuário marca como concluído
    Vencido --> Concluído : Usuário marca como concluído

    Concluído --> [*]
    Vencido --> [*] : Usuário arquiva/remove
```

---

## 6. Fluxo de Dados — Módulo de Favoritos

```
ENTRADA (DOM do SEI)          PROCESSAMENTO               SAÍDA (DOM modificado)
──────────────────────        ──────────────────────       ──────────────────────
Tabela de processos     ──►  Lê números de processo  ──►  Ícone ★ ao lado
renderizada pelo SEI         Consulta localStorage         de cada processo
                             Marca processos já fav.
                                                     ──►  Seção "Favoritos"
localStorage             ──►  Carrega lista de        ──►  no topo da lista
(seiPro_favoritos)            Favorito[]
                              Ordena por 'ordem'
                                                     ──►  Atualiza badge
Evento: clique em ★      ──►  Cria/remove Favorito        no ícone (count)
                              Persiste no localStorage
```

---

## 7. Diagrama de Implantação

```
┌──────────────────────────────────────────────────────────┐
│                  AMBIENTE DO USUÁRIO                     │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │              NAVEGADOR (Chrome/Edge/Firefox)        │  │
│  │                                                    │  │
│  │  ┌─────────────────┐    ┌────────────────────────┐ │  │
│  │  │   SEI Pro       │    │  Perfil do Navegador   │ │  │
│  │  │   (dist/)       │    │  chrome.storage.local  │ │  │
│  │  │                 │    │  localStorage          │ │  │
│  │  │  manifest.json  │    │  sessionStorage        │ │  │
│  │  │  background.js  │    └────────────────────────┘ │  │
│  │  │  js/*.js        │                               │  │
│  │  │  css/*.css      │                               │  │
│  │  │  html/options   │                               │  │
│  │  └─────────────────┘                               │  │
│  │                                                    │  │
│  │  ┌─────────────────────────────────────────────┐   │  │
│  │  │         ABA: instancia.sei.gov.br/sei/      │   │  │
│  │  │                                             │   │  │
│  │  │   Servidor SEI (PHP) ──► HTML renderizado   │   │  │
│  │  │                  ▲                          │   │  │
│  │  │                  │ content scripts injetados│   │  │
│  │  │                  └── pela extensão          │   │  │
│  │  └─────────────────────────────────────────────┘   │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
          │ HTTPS (opcional, sob demanda do usuário)
          ├──────────────► api.openai.com
          └──────────────► sheets.googleapis.com
```

---

## 8. Mapa de Dependências entre Módulos

```
init_all.js
    └──► sei-pro-all.js
              └──► sei-functions-pro.js (base compartilhada)

init.js
    ├──► sei-pro.js
    │        └──► sei-functions-pro.js
    ├──► sei-pro-favoritos.js
    │        └──► sei-functions-pro.js
    ├──► sei-pro-atividades.js   (controle de prazos)
    │        └──► sei-functions-pro.js
    ├──► sei-pro-ai.js
    │        └──► sei-functions-pro.js
    └──► sei-pro-docs-lote.js
             └──► sei-functions-pro.js

init_arvore.js
    └──► sei-pro-arvore.js
             └──► sei-functions-pro.js

init_visualizacao.js / init_visualizacao_html.js
    └──► sei-pro-editor.js
             └──► sei-functions-pro.js

sei-functions-pro.js  ◄── núcleo: todas as funções utilitárias e detecção de versão
    └──► lib/ (jQuery, DOMPurify, Moment.js, etc.)
```
