# Modelo de Domínio e Dicionário de Dados

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

---

## 1. Visão Geral do Domínio

O SEI Pro opera sobre o domínio do SEI (Sistema Eletrônico de Informações). As entidades abaixo representam os conceitos manipulados pela extensão, seja lendo do DOM do SEI ou armazenando dados próprios.

```
┌─────────────────────────────────────────────────────────────┐
│                    DOMÍNIO DO SEI (host)                    │
│                                                             │
│   ┌──────────┐    contém    ┌──────────┐                    │
│   │ Processo ├─────────────►│Documento │                    │
│   └──────────┘  1..*       └──────────┘                    │
│        │                        │                           │
│        │ pertence a             │ tem tipo                  │
│        ▼                        ▼                           │
│   ┌──────────┐            ┌──────────┐                      │
│   │ Unidade  │            │TipoDoc   │                      │
│   └──────────┘            └──────────┘                      │
│        │                                                     │
│        │ gerenciada por                                      │
│        ▼                                                     │
│   ┌──────────┐                                              │
│   │ Usuário  │                                              │
│   └──────────┘                                              │
└─────────────────────────────────────────────────────────────┘
         │
         │ estende com
         ▼
┌─────────────────────────────────────────────────────────────┐
│                   DOMÍNIO DO SEI PRO                        │
│                                                             │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌──────────┐   │
│  │Favorito │   │  Prazo  │   │ Projeto │   │ Atividade│   │
│  └─────────┘   └─────────┘   └─────────┘   └──────────┘   │
│                                                             │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐                   │
│  │Histórico│   │Configur.│   │PromptIA │                   │
│  └─────────┘   └─────────┘   └─────────┘                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Entidades do Domínio SEI (lidas via DOM)

Estas entidades existem no SEI e são **lidas** pela extensão através do DOM — a extensão não as persiste nem as modifica no servidor.

### 2.1 Processo

| Atributo | Tipo | Descrição |
|---|---|---|
| `numero` | String | Número único do processo (ex: `00000.000001/2024-01`) |
| `tipo` | String | Tipo/assunto do processo |
| `especificacao` | String | Descrição/especificação do processo |
| `situacao` | Enum | `aberto`, `sobrestado`, `concluído` |
| `unidadeAtual` | String | Unidade onde o processo está atualmente |
| `marcadores` | String[] | Lista de marcadores atribuídos |
| `dataUltimaModificacao` | Date | Data da última modificação |
| `responsavel` | String | Nome do responsável atual |
| `documentos` | Documento[] | Lista de documentos contidos |

### 2.2 Documento

| Atributo | Tipo | Descrição |
|---|---|---|
| `numero` | String | Número sequencial do documento no processo |
| `tipo` | String | Tipo do documento (ofício, memorando, despacho, etc.) |
| `dataElaboracao` | Date | Data de elaboração |
| `assinado` | Boolean | Se o documento está assinado digitalmente |
| `sigiloso` | Boolean | Se o documento tem restrição de acesso (LGPD) |
| `formato` | Enum | `nato-digital`, `digitalizado` |
| `conteudo` | HTML | Conteúdo HTML do documento (acessível no iframe do editor) |

### 2.3 Usuário (contexto de sessão)

| Atributo | Tipo | Descrição |
|---|---|---|
| `siglaUnidade` | String | Sigla da unidade atual do usuário logado |
| `nomeUnidade` | String | Nome completo da unidade |
| `nomeUsuario` | String | Nome do usuário logado |

---

## 3. Entidades do Domínio SEI Pro (persistidas pela extensão)

Estas entidades são **criadas e gerenciadas** pela extensão, armazenadas no `localStorage`, `sessionStorage` ou `chrome.storage.local`.

### 3.1 Favorito

Representa um processo marcado como favorito pelo usuário.

| Atributo | Tipo | Armazenamento | Descrição |
|---|---|---|---|
| `id` | String | localStorage | UUID gerado pela extensão |
| `numeroProcesso` | String | localStorage | Número do processo no SEI |
| `descricao` | String | localStorage | Rótulo personalizado pelo usuário |
| `cor` | String | localStorage | Cor do marcador (hex) |
| `ordem` | Integer | localStorage | Posição na lista de favoritos |
| `dataCriacao` | ISO8601 | localStorage | Data em que foi favoritado |

### 3.2 Prazo

Representa um prazo associado a um processo no módulo de Controle de Prazos.

| Atributo | Tipo | Armazenamento | Descrição |
|---|---|---|---|
| `id` | String | localStorage | UUID gerado pela extensão |
| `numeroProcesso` | String | localStorage | Número do processo associado |
| `titulo` | String | localStorage | Título descritivo do prazo |
| `dataInicio` | ISO8601 | localStorage | Data de início do prazo |
| `dataFim` | ISO8601 | localStorage | Data limite do prazo |
| `status` | Enum | localStorage | `pendente`, `concluido`, `vencido` |
| `cor` | String | localStorage | Cor de exibição no Gantt (hex) |
| `observacao` | String | localStorage | Observações adicionais |

### 3.3 Projeto

Representa um projeto gerenciado no módulo Kanban/Gantt.

| Atributo | Tipo | Armazenamento | Descrição |
|---|---|---|---|
| `id` | String | localStorage / Google Sheets | UUID do projeto |
| `nome` | String | localStorage / Google Sheets | Nome do projeto |
| `descricao` | String | localStorage / Google Sheets | Descrição do projeto |
| `colunas` | Coluna[] | localStorage / Google Sheets | Colunas do quadro Kanban |
| `dataCriacao` | ISO8601 | localStorage / Google Sheets | Data de criação |
| `compartilhado` | Boolean | Google Sheets | Se usa backend remoto |

### 3.4 Coluna (Kanban)

| Atributo | Tipo | Descrição |
|---|---|---|
| `id` | String | UUID da coluna |
| `titulo` | String | Nome da coluna (ex: "A fazer", "Em andamento") |
| `cartoes` | Cartao[] | Lista de cartões na coluna |
| `ordem` | Integer | Posição da coluna no quadro |

### 3.5 Cartão (Kanban)

| Atributo | Tipo | Descrição |
|---|---|---|
| `id` | String | UUID do cartão |
| `titulo` | String | Título do cartão |
| `descricao` | String | Descrição detalhada |
| `numeroProcesso` | String | Processo SEI associado (opcional) |
| `responsavel` | String | Responsável pelo cartão |
| `prazo` | ISO8601 | Data limite |
| `cor` | String | Cor de destaque (hex) |

### 3.6 HistoricoProcesso

Registro de processos visitados recentemente.

| Atributo | Tipo | Armazenamento | Descrição |
|---|---|---|---|
| `numeroProcesso` | String | sessionStorage | Número do processo |
| `descricao` | String | sessionStorage | Tipo/especificação (capturado do DOM) |
| `url` | String | sessionStorage | URL completa da página |
| `dataAcesso` | ISO8601 | sessionStorage | Timestamp do acesso |

### 3.7 ConfiguracaoExtensao

Configurações gerais da extensão, definidas pelo usuário na página de opções.

| Atributo | Tipo | Padrão | Descrição |
|---|---|---|---|
| `estiloAvancado` | Boolean | `true` | Ativa tema visual alternativo |
| `modoNoturno` | Boolean | `false` | Ativa dark mode |
| `autosave` | Boolean | `true` | Ativa autossalvamento do editor |
| `autosaveIntervalo` | Integer | `30` | Intervalo de autossave em segundos |
| `openaiApiKey` | String | `""` | Chave de API da OpenAI |
| `googleSheetsId` | String | `""` | ID da planilha Google Sheets |
| `googleApiKey` | String | `""` | Chave de API do Google Cloud |
| `funcionalidadesAtivas` | Object | `{}` | Mapa de `{nomeFuncionalidade: boolean}` |
| `senhaLogin` | String (criptografada) | `""` | Senha para preenchimento automático |

### 3.8 PromptIA

Prompt personalizado salvo pelo usuário para o módulo de IA.

| Atributo | Tipo | Armazenamento | Descrição |
|---|---|---|---|
| `id` | String | localStorage | UUID do prompt |
| `nome` | String | localStorage | Nome exibido no menu |
| `conteudo` | String | localStorage | Texto do prompt |
| `categoria` | String | localStorage | Categoria para agrupamento |

---

## 4. Dicionário de Dados — Chaves de Storage

### localStorage

| Chave | Tipo do Valor | Módulo | Descrição |
|---|---|---|---|
| `seiPro_favoritos` | JSON (Favorito[]) | Favoritos | Lista de processos favoritos |
| `seiPro_prazos` | JSON (Prazo[]) | Prazos | Lista de prazos cadastrados |
| `seiPro_projetos` | JSON (Projeto[]) | Projetos | Lista de projetos (modo local) |
| `seiPro_promptsIA` | JSON (PromptIA[]) | IA | Prompts personalizados |
| `seiPro_config` | JSON (ConfiguracaoExtensao) | Opções | Configurações da extensão |

### sessionStorage

| Chave | Tipo do Valor | Módulo | Descrição |
|---|---|---|---|
| `versaoSei` | String | Global | Versão detectada do SEI (ex: `"5.0.3"`) |
| `seiPro_historico` | JSON (HistoricoProcesso[]) | Histórico | Processos visitados na sessão |
| `seiPro_arvoreCache` | JSON | Árvore | Cache da estrutura da árvore de documentos |

### chrome.storage.local

| Chave | Tipo do Valor | Descrição |
|---|---|---|
| `config` | JSON (ConfiguracaoExtensao) | Configurações sincronizadas entre abas |
| `badgeCount` | Integer | Contador exibido no ícone da extensão |

---

## 5. Regras de Negócio

| ID | Regra |
|---|---|
| RN-01 | Um processo pode ter no máximo um registro de favorito por usuário |
| RN-02 | Prazos com `dataFim` anterior à data atual devem ter status `vencido` automaticamente |
| RN-03 | O autossalvamento só é ativado quando o usuário inicia a edição de um documento |
| RN-04 | O preenchimento automático de senha só é executado se a funcionalidade estiver ativa nas configurações |
| RN-05 | Funcionalidades desativadas pelo usuário não devem injetar código no DOM do SEI |
| RN-06 | O histórico de processos visitados é limitado a 50 entradas por sessão |
| RN-07 | Dados enviados à API da OpenAI são o texto selecionado pelo usuário — nunca dados do processo inteiro automaticamente |
| RN-08 | O backend Google Sheets é sempre opcional — o módulo de Projetos deve funcionar em modo local sem configuração de API |
