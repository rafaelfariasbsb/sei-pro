# Módulo: sei-pro.js

**Tamanho:** ~198 KB  
**Contexto:** Injetado por `init.js` na página principal de trabalho do processo  
**Página SEI:** `?acao=procedimento_trabalhar`  
**Status SEI 5:** 🔲 Não testado  
**Arquivo:** `dist/js/sei-pro.js`

---

## Responsabilidade

Módulo central da página de trabalho do processo. Coordena o carregamento dos submódulos e adiciona funcionalidades à lista de processos e ao painel principal:

- Agrupamento da lista de processos por critérios variados
- Rolagem infinita na pesquisa de processos
- Exportação da lista para CSV
- Marcação de processos como "Não Visualizado"
- Alertas de documentos não assinados
- Cores personalizadas em marcadores
- Exibição de especificação do processo na tabela
- Exibição de nomes de usuários na tabela
- Contador de processos não recebidos (badge no ícone)
- URLs amigáveis
- Substituição de seleção (checkboxes inteligentes)

---

## Dependências

- `sei-functions-pro.js` (base)
- `sei-pro-favoritos.js` (carregado em conjunto)
- `sei-pro-atividades.js` (carregado em conjunto)

---

## Elementos DOM críticos

| Elemento | Seletor SEI 4.x | SEI 5 | Impacto |
|---|---|---|---|
| Tabela de processos | `#divInfraAreaTabela table` | A confirmar | Alto |
| Linha de processo | `tr.infraTrClaro`, `tr.infraTrEscuro` | A confirmar (Bootstrap?) | Alto |
| Checkbox de seleção | `.infraCheckbox` | `.custom-control-input` | Alto |
| Paginação | `#divInfraAreaPaginacao` | A confirmar | Médio |
| Barra de comandos | `#divBotoesControleProcessos` | A confirmar | Médio |
| Coluna de marcadores | `td.infraTdLinkAlt` | A confirmar | Médio |
| Número do processo | `a.processoVisitado` | A confirmar | Alto |

---

## Funcionalidades e status no SEI 5

| Funcionalidade | Dependência crítica | Status SEI 5 |
|---|---|---|
| Agrupamento de lista | Estrutura da tabela de processos | 🔲 |
| Rolagem infinita | Paginação do SEI | 🔲 |
| Exportar CSV | Tabela de processos | 🔲 |
| Marcar "Não Visualizado" | Linha da tabela | 🔲 |
| Alertas documentos | Árvore + editor | 🔲 |
| Cores em marcadores | Coluna de marcadores | 🔲 |
| URLs amigáveis | URL da página | Provavelmente ok |
| Checkboxes inteligentes | `.infraCheckbox` → Bootstrap | 🔲 |
| Badge contador | chrome.storage + background | Provavelmente ok |
