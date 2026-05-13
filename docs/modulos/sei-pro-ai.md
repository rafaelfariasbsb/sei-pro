# Módulo: sei-pro-ai.js

**Tamanho:** ~114 KB  
**Contexto:** Injetado por `init.js` na página principal  
**Status SEI 5:** 🔲 Não testado (núcleo de API provavelmente funciona; UI depende do editor)  
**Arquivo:** `dist/js/sei-pro-ai.js`

---

## Responsabilidade

Integra a extensão com serviços de IA (prioritariamente OpenAI/ChatGPT) para auxiliar a redação de documentos no SEI:

- Menu de prompts predefinidos (resumir, reescrever, traduzir, pesquisar legislação)
- Prompts personalizados criados pelo usuário
- Painel lateral com resposta da IA
- Aplicar resposta ao documento (substituir seleção)
- Histórico de interações da sessão

---

## Dependências

- `sei-functions-pro.js` (base)
- `sei-pro-editor.js` (para inserção do resultado no documento)
- API OpenAI (external, via `fetch`)

---

## Fluxo de dados

```
Usuário seleciona texto
       │
       ▼
Usuário escolhe prompt
       │
       ▼
sei-pro-ai.js monta payload:
  {
    model: "gpt-4o" (configurável),
    messages: [
      { role: "system", content: promptSistema },
      { role: "user",   content: prompt + "\n\n" + textoSelecionado }
    ]
  }
       │
       ▼
fetch("https://api.openai.com/v1/chat/completions", {
  headers: { Authorization: "Bearer " + apiKey }
})
       │
       ▼
Resposta exibida no painel lateral
       │
       ▼
Usuário: Aplicar / Copiar / Descartar
```

---

## Configuração

| Config | Chave storage | Descrição |
|---|---|---|
| Chave de API | `seiPro_config.openaiApiKey` | Chave de API do usuário no OpenAI |
| Modelo | `seiPro_config.openaiModelo` | Modelo a usar (padrão: `gpt-4o`) |
| Prompts customizados | `seiPro_promptsIA` | Array de PromptIA salvo pelo usuário |

---

## Prompts predefinidos

| Nome | Função |
|---|---|
| Resumir | Condensa o texto em pontos principais |
| Reescrever | Melhora clareza e objetividade mantendo o conteúdo |
| Traduzir | Traduz para português formal |
| Pesquisa de legislação | Identifica legislação relevante ao texto |
| Revisar | Corrige gramática e adequação à linguagem administrativa |
| Escrita interativa | Modo de coedição com a IA |

---

## Status no SEI 5

A chamada à API OpenAI é independente do DOM do SEI — provavelmente funciona no SEI 5 sem alterações. O problema principal está na **inserção do resultado no documento**, que depende do editor.

| Componente | Dependência | Status SEI 5 |
|---|---|---|
| Chamada à API OpenAI | Apenas `fetch` | ✅ Deve funcionar |
| Exibição do painel lateral | DOM geral da página | 🔲 |
| Captura do texto selecionado | Editor do SEI | ❌ Depende do editor CK5 |
| Inserção do resultado | Editor do SEI | ❌ Depende do editor CK5 |

**Conclusão:** Testar após Fase 2 (adaptação do editor para CKEditor 5).

---

## Considerações de segurança

- A chave de API é armazenada em `localStorage` — visível para qualquer script no mesmo domínio (incluindo o SEI)
- O texto enviado à API é o texto **selecionado pelo usuário** — nunca o documento inteiro automaticamente
- Considerar migrar para `chrome.storage.local` (mais seguro, não acessível pelo SEI) em versão futura
- Documentar claramente que textos enviados à OpenAI saem do perímetro do órgão
