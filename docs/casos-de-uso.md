# Especificação de Casos de Uso

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

---

## 1. Atores

| Ator | Descrição |
|---|---|
| **Usuário SEI** | Servidor público que utiliza o SEI no navegador com a extensão instalada |
| **Administrador da Extensão** | Usuário que configura a extensão (pode ser o mesmo Usuário SEI) |
| **API OpenAI** | Sistema externo de IA (ator secundário) |
| **Google Sheets API** | Sistema externo de armazenamento (ator secundário) |
| **SEI (sistema host)** | O sistema SEI cujo DOM é lido e modificado pela extensão (ator secundário) |

---

## 2. Diagrama de Casos de Uso (visão geral)

```
                        ┌─────────────────────────────────────────┐
                        │              SEI Pro                     │
                        │                                         │
  ┌──────────┐          │  ┌─────────────────────────────────┐    │
  │          │ usa      │  │  UC01 Gerenciar Favoritos        │    │
  │ Usuário  ├──────────┼─►│  UC02 Controlar Prazos           │    │
  │   SEI    │          │  │  UC03 Editar Documento (ext.)    │    │
  │          │          │  │  UC04 Ações em Lote              │    │
  └──────────┘          │  │  UC05 Agrupar Lista de Processos │    │
                        │  │  UC06 Usar IA no Documento       │    │
                        │  │  UC07 Gerenciar Projetos         │    │
  ┌──────────┐          │  └─────────────────────────────────┘    │
  │  Admin.  ├──────────┼─►  UC08 Configurar Extensão             │
  │ Extensão │          │                                         │
  └──────────┘          └─────────────────────────────────────────┘
                                    │           │
                                    ▼           ▼
                              ┌──────────┐  ┌──────────┐
                              │  OpenAI  │  │  Google  │
                              │   API    │  │  Sheets  │
                              └──────────┘  └──────────┘
```

---

## 3. Especificação Detalhada dos Casos de Uso

---

### UC01 — Gerenciar Favoritos

**Objetivo:** Permitir que o usuário marque e acesse rapidamente processos frequentemente utilizados.

**Ator principal:** Usuário SEI  
**Pré-condições:** Usuário está na lista de processos do SEI com a extensão ativa.  
**Pós-condições:** Favorito salvo no `localStorage`; processo exibido na lista de favoritos.

#### Fluxo Principal — Adicionar favorito

1. Usuário visualiza a lista de processos no SEI
2. Extensão exibe ícone de favorito ao lado de cada processo
3. Usuário clica no ícone de favorito de um processo
4. Sistema exibe formulário com campos: descrição (pré-preenchida), cor
5. Usuário confirma
6. Sistema salva o favorito no `localStorage`
7. Sistema exibe o processo na seção de favoritos

#### Fluxo Alternativo — Remover favorito

3a. Usuário clica no ícone de favorito de um processo já favoritado  
3b. Sistema solicita confirmação de remoção  
3c. Usuário confirma  
3d. Sistema remove o favorito do `localStorage`

#### Fluxo Alternativo — Reordenar favoritos

7a. Usuário arrasta e solta favoritos para reordenar  
7b. Sistema persiste a nova ordem no `localStorage`

#### Exceções

- E1: `localStorage` indisponível → exibir mensagem de erro orientando o usuário a verificar as permissões do navegador

---

### UC02 — Controlar Prazos

**Objetivo:** Registrar e acompanhar prazos associados a processos com visualização em Gantt.

**Ator principal:** Usuário SEI  
**Pré-condições:** Usuário está na lista de processos.  
**Pós-condições:** Prazo salvo; gráfico de Gantt atualizado.

#### Fluxo Principal — Cadastrar prazo

1. Usuário acessa o painel de Controle de Prazos (botão injetado pela extensão)
2. Usuário clica em "Novo Prazo"
3. Sistema exibe formulário: processo, título, data início, data fim, cor, observação
4. Usuário preenche e confirma
5. Sistema valida datas (início ≤ fim)
6. Sistema salva o prazo no `localStorage`
7. Sistema atualiza o gráfico de Gantt

#### Fluxo Principal — Visualizar Gantt

1. Usuário acessa o painel de Controle de Prazos
2. Sistema carrega todos os prazos do `localStorage`
3. Sistema renderiza gráfico de Gantt (frappe-gantt)
4. Prazos vencidos exibidos em vermelho; próximos do vencimento em amarelo

#### Exceções

- E1: Data fim anterior à data de início → exibir erro de validação inline
- E2: Processo informado não existe no SEI → exibir aviso (não bloqueia — usuário pode inserir número manualmente)

---

### UC03 — Editar Documento com Recursos Avançados

**Objetivo:** Oferecer ferramentas adicionais no editor de documentos do SEI.

**Ator principal:** Usuário SEI  
**Pré-condições:** Usuário está na tela de edição de um documento no SEI.  
**Pós-condições:** Documento modificado conforme a ferramenta utilizada.

#### Fluxo Principal — Inserir Nota de Rodapé

1. Usuário posiciona cursor no texto onde deseja inserir a nota
2. Usuário clica no botão "Nota de Rodapé" na barra de ferramentas adicionada pela extensão
3. Sistema exibe campo de texto para o conteúdo da nota
4. Usuário digita o conteúdo e confirma
5. Sistema insere marcação de nota no cursor e a nota numerada no rodapé do documento

#### Fluxo Principal — Autossalvamento

1. Usuário inicia edição de um documento
2. Sistema inicia timer de autossalvamento (intervalo configurável, padrão 30s)
3. A cada intervalo, sistema captura o conteúdo atual do editor
4. Sistema salva no `sessionStorage` com chave identificada pelo número do documento
5. Sistema exibe indicador discreto de "Salvo automaticamente às HH:MM"

#### Fluxo Alternativo — Recuperar rascunho após queda

3a. Usuário abre documento que possui rascunho salvo  
3b. Sistema detecta rascunho no `sessionStorage`  
3c. Sistema pergunta se deseja recuperar o rascunho  
3d. Usuário confirma → sistema restaura conteúdo salvo

#### Exceções

- E1: Editor do SEI não encontrado no DOM (versão incompatível) → exibir aviso de incompatibilidade no console; não injetar barra de ferramentas

---

### UC04 — Executar Ações em Lote

**Objetivo:** Realizar ações sobre múltiplos documentos ou processos simultaneamente.

**Ator principal:** Usuário SEI  
**Pré-condições:** Usuário está na lista de processos ou árvore de documentos.  
**Pós-condições:** Ação executada em todos os itens selecionados.

#### Fluxo Principal

1. Usuário ativa o modo de seleção múltipla (botão ou atalho da extensão)
2. Sistema habilita checkboxes em cada item da lista
3. Usuário seleciona os itens desejados
4. Sistema exibe menu de ações disponíveis (enviar, concluir, marcar, exportar, etc.)
5. Usuário escolhe a ação
6. Sistema exibe confirmação com resumo (quantidade de itens afetados)
7. Usuário confirma
8. Sistema executa a ação via interação com a interface do SEI (simulação de cliques)
9. Sistema exibe relatório de sucesso/falha por item

#### Exceções

- E1: SEI bloqueia a ação por falta de permissão → item marcado como falha no relatório, demais prosseguem

---

### UC05 — Agrupar Lista de Processos

**Objetivo:** Reorganizar visualmente a lista de processos por diferentes critérios.

**Ator principal:** Usuário SEI  
**Pré-condições:** Usuário está na tela de controle de processos do SEI.  
**Pós-condições:** Lista reorganizada visualmente (sem alterar dados no servidor).

#### Fluxo Principal

1. Usuário seleciona critério de agrupamento no menu adicionado pela extensão (data, tipo, responsável, marcador, etc.)
2. Sistema lê os dados da tabela de processos já renderizada pelo SEI
3. Sistema reorganiza as linhas da tabela por agrupamento
4. Sistema insere linhas de cabeçalho de grupo com contador de itens
5. Usuário pode expandir/colapsar grupos

---

### UC06 — Usar IA no Documento

**Objetivo:** Aplicar IA (ChatGPT) sobre o texto do documento para auxiliar a redação.

**Ator principal:** Usuário SEI  
**Atores secundários:** API OpenAI  
**Pré-condições:** Usuário está no editor ou visualizador de um documento; chave de API OpenAI configurada.  
**Pós-condições:** Resposta da IA exibida ao usuário para revisão e aplicação opcional.

#### Fluxo Principal

1. Usuário seleciona um trecho de texto no documento
2. Usuário clica no ícone de IA na barra de ferramentas da extensão
3. Sistema exibe menu de prompts disponíveis (resumir, reescrever, pesquisar legislação, traduzir, etc.)
4. Usuário escolhe um prompt ou digita um personalizado
5. Sistema monta requisição: prompt selecionado + texto selecionado
6. Sistema envia requisição para a API OpenAI (usando chave do usuário)
7. Sistema exibe resultado em painel lateral com opções: "Aplicar", "Copiar", "Descartar"
8. Usuário escolhe a ação

#### Exceções

- E1: Chave de API não configurada → exibir mensagem orientando o usuário a configurar nas opções
- E2: Erro de autenticação na API → exibir mensagem de erro com link para as configurações
- E3: Timeout na requisição → exibir mensagem de erro e opção de tentar novamente
- E4: Nenhum texto selecionado → exibir aviso solicitando seleção de texto

---

### UC07 — Gerenciar Projetos

**Objetivo:** Criar e acompanhar projetos com quadro Kanban e Gantt.

**Ator principal:** Usuário SEI  
**Ator secundário:** Google Sheets API (opcional)  
**Pré-condições:** Usuário está no SEI com a extensão ativa.

#### Fluxo Principal — Criar projeto local

1. Usuário acessa o módulo de Projetos (painel da extensão)
2. Usuário clica em "Novo Projeto"
3. Sistema exibe formulário: nome, descrição, colunas iniciais
4. Usuário preenche e confirma
5. Sistema cria projeto no `localStorage`
6. Sistema exibe quadro Kanban do projeto recém-criado

#### Fluxo Alternativo — Sincronizar com Google Sheets

5a. Usuário ativa opção "Compartilhar via Google Sheets" e fornece credenciais  
5b. Sistema cria aba na planilha configurada  
5c. Sistema sincroniza criação e edição de cartões em tempo real

---

### UC08 — Configurar Extensão

**Objetivo:** Permitir que o usuário personalize o comportamento da extensão.

**Ator principal:** Administrador da Extensão  
**Pré-condições:** Extensão instalada no navegador.  
**Pós-condições:** Configurações salvas no `chrome.storage.local`.

#### Fluxo Principal

1. Usuário acessa a página de opções da extensão (ícone na barra do navegador → Opções)
2. Sistema carrega configurações atuais do `chrome.storage.local`
3. Sistema exibe seções: Funcionalidades (ativar/desativar), Aparência, IA, Projetos, Conta
4. Usuário modifica as configurações desejadas
5. Usuário clica em "Salvar"
6. Sistema valida e persiste as configurações
7. Sistema exibe confirmação de salvamento

#### Exceções

- E1: Chave de API inválida → exibir erro de validação antes de salvar
