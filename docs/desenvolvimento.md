# Guia de Desenvolvimento

## Pré-requisitos

- Google Chrome, Microsoft Edge ou Mozilla Firefox
- Editor de código (VS Code recomendado)
- Git
- Acesso a uma instância do SEI para testes (versão 4.x e/ou 5.x)

---

## Configurando o ambiente

### 1. Clone o repositório

```bash
git clone git@github.com:rafaelfariasbsb/sei-pro.git
cd sei-pro
```

### 2. Carregue a extensão no navegador

**Chrome / Edge:**
1. Acesse `chrome://extensions` (ou `edge://extensions`)
2. Ative o **Modo do desenvolvedor** (canto superior direito)
3. Clique em **Carregar sem compactação**
4. Selecione a pasta `dist/`

**Firefox:**
1. Acesse `about:debugging`
2. Clique em **Este Firefox**
3. Clique em **Carregar extensão temporária**
4. Selecione o arquivo `dist/manifest.json`

### 3. Recarregando após edições

- **Chrome/Edge:** Na página de extensões, clique no ícone de atualização (↺) da extensão
- **Firefox:** Na página de debugging, clique em **Recarregar**
- Recarregue a aba do SEI após recarregar a extensão

> Não há etapa de build. Os arquivos em `dist/` são executados diretamente pelo navegador.

---

## Estrutura de diretórios

```
sei-pro/
├── dist/                     ← Extensão pronta para carregar no browser
│   ├── manifest.json         ← Configuração da extensão (MV3)
│   ├── background.js         ← Service worker
│   ├── config_hosts.json     ← Domínios excluídos da injeção
│   ├── js/
│   │   ├── init*.js          ← Entry points por contexto de página
│   │   ├── sei-functions-pro.js  ← Funções utilitárias compartilhadas
│   │   ├── sei-pro*.js       ← Módulos de funcionalidades
│   │   ├── sei-legis.js      ← Ferramentas de Legística
│   │   └── lib/              ← Dependências de terceiros (vendorizadas)
│   ├── css/                  ← Estilos da extensão
│   ├── html/                 ← Página de opções
│   ├── icons/                ← Ícones da extensão
│   └── webfonts/             ← FontAwesome fonts
├── docs/                     ← Documentação técnica
│   ├── adr/                  ← Architecture Decision Records
│   ├── arquitetura.md
│   ├── desenvolvimento.md    ← Este arquivo
│   ├── seletores-sei.md
│   └── matriz-compatibilidade.md
├── img/                      ← Imagens para documentação
├── pages/                    ← Documentação de funcionalidades para usuários
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── CHANGELOG.md
├── SECURITY.md
└── README.md
```

---

## Como encontrar o código de uma funcionalidade

### Por contexto de página

| Estou vendo... | Arquivo relevante |
|---|---|
| Lista de processos | `sei-pro.js`, `sei-pro-favoritos.js`, `sei-pro-atividades.js` |
| Editor de documentos | `sei-pro-editor.js` |
| Árvore do processo | `sei-pro-arvore.js` |
| Todas as páginas | `sei-pro-all.js` |
| Projetos / Kanban | `sei-pro-projetos.js` |
| IA / ChatGPT | `sei-pro-ai.js` |
| Ações em lote | `sei-pro-docs-lote.js` |

### Por funcionalidade

Use a busca do seu editor em `dist/js/` com o nome da funcionalidade em português ou inglês. As funções geralmente têm nomes descritivos como `adicionarFavorito()`, `gerarQRCode()`, `inserirNota()`.

---

## Debugando

### Console do navegador

Cada iframe do SEI tem seu próprio contexto de JavaScript. Para inspecionar:

**Chrome:**
1. Abra o DevTools (`F12`)
2. No canto superior esquerdo do Console, selecione o contexto desejado no dropdown (lista os iframes)
3. Contextos relevantes: `ifrArvore`, `ifrVisualizacao`, página principal

**Firefox:**
1. Abra o DevTools (`F12`)
2. Na aba Console, use o seletor de frame no topo

### Logs da extensão (background script)

- **Chrome:** `chrome://extensions` > SEI Pro > **Detalhes** > **Exibir visualizações: service worker**
- **Firefox:** `about:debugging` > Este Firefox > SEI Pro > **Inspecionar**

---

## Testando compatibilidade com o SEI

### Versões do SEI para testar

| Versão | Prioridade |
|---|---|
| SEI 5.x | Alta — foco atual do fork |
| SEI 4.1.x | Alta — base de usuários existente |
| SEI 4.0.x | Média |

### O que testar após uma mudança

1. A funcionalidade alterada funciona conforme esperado
2. Nenhuma mensagem de erro no console JavaScript
3. As funcionalidades adjacentes não foram quebradas
4. Funciona tanto no SEI 4.x quanto no SEI 5.x (se possível)

---

## Dependências

Todas as dependências ficam em `dist/js/lib/` e são carregadas diretamente — sem npm.

| Biblioteca | Versão | Uso |
|---|---|---|
| jQuery | 3.4.1 | Manipulação DOM |
| jQuery UI | — | Widgets de UI |
| DOMPurify | — | Sanitização XSS |
| PDF.js | — | Renderização de PDF |
| Moment.js | — | Datas e horas |
| frappe-gantt | — | Gráfico de Gantt |
| jKanban | — | Quadro Kanban |
| Chart.js | — | Gráficos |
| Tesseract.js | — | OCR |
| diff2html | — | Diff de documentos |
| Mammoth.js | — | DOCX → HTML |
| CKEditor | — | Editor de texto rico |
| CryptoJS | — | Utilitários criptográficos |
| JSZip | — | Arquivos ZIP |
| PapaParse | — | Parse de CSV |
| html2canvas | — | Screenshots |

> Para atualizar uma dependência, substitua o arquivo correspondente em `dist/js/lib/` pela nova versão.

---

## Adicionando uma nova funcionalidade

1. Identifique em qual módulo a feature se encaixa (ver tabela acima)
2. Adicione a lógica no arquivo correspondente
3. Se a feature depende de seletores DOM do SEI, registre-os em [`docs/seletores-sei.md`](./seletores-sei.md)
4. Se a feature se comporta diferente no SEI 5, use:
   ```javascript
   if (isSEI_5) {
       // comportamento para SEI 5
   } else {
       // comportamento para SEI 4.x
   }
   ```
5. Adicione a feature na página de opções (`dist/html/options.html`) se precisar de configuração
6. Documente para o usuário final em `pages/NOMEDA FEATURE.md`
7. Atualize o `CHANGELOG.md`
