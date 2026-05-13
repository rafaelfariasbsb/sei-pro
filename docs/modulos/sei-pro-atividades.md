# Módulo: sei-pro-atividades.js

**Tamanho:** ~2.1 MB (maior arquivo do projeto)  
**Contexto:** Injetado por `init.js` na página principal  
**Status SEI 5:** 🔲 Não testado  
**Arquivo:** `dist/js/sei-pro-atividades.js`

---

## Responsabilidade

Módulo de **Controle de Prazos e Atividades** — o maior e mais complexo da extensão. Gerencia:

- Cadastro de prazos associados a processos
- Visualização de prazos em gráfico de Gantt (frappe-gantt)
- Reabertura programada de processos
- Controle de atividades e tarefas relacionadas a processos
- Alertas de prazos próximos ou vencidos
- Badge no ícone da extensão com contagem de prazos urgentes

---

## Dependências

- `sei-functions-pro.js` (base)
- `lib/frappe-gantt.js` (gráfico de Gantt)
- `lib/moment.min.js` + plugins (manipulação de datas)
- `lib/chart.min.js` (gráficos auxiliares)
- `chrome.storage.local` (configurações)
- `localStorage` (dados de prazos)

---

## Estrutura de dados

```javascript
// localStorage['seiPro_prazos']
[
  {
    id: "uuid",
    numeroProcesso: "00000.000001/2024-01",
    titulo: "Prazo para manifestação",
    dataInicio: "2024-01-15",
    dataFim: "2024-01-30",
    status: "pendente",  // pendente | proximo | vencido | concluido
    cor: "#3498db",
    observacao: "Aguardar parecer jurídico"
  }
]
```

---

## Problemas de manutenção

O arquivo com 2.1 MB concentra funcionalidades que deveriam estar separadas:
- Lógica de negócio (cálculo de status, validações)
- Persistência (leitura/escrita no storage)
- Renderização (UI do painel, Gantt)
- Integração com SEI (leitura de números de processo do DOM)

**Recomendação:** Refatorar em submódulos na Fase 4. Ver [Roadmap](../roadmap.md).

---

## Status no SEI 5

| Componente | Dependência SEI | Status SEI 5 |
|---|---|---|
| Painel de prazos (modal próprio) | Mínima | 🔲 Provavelmente ok |
| Gantt chart | Nenhuma (lib própria) | 🔲 Provavelmente ok |
| Leitura de números de processo | Tabela de processos do SEI | 🔲 Depende da tabela |
| Badge no ícone | chrome.storage + background | 🔲 Provavelmente ok |
| Reabertura programada | Tabela de processos + ações | 🔲 Depende de seletores |

**Estimativa:** Módulo com maior chance de funcionar parcialmente no SEI 5 sem grandes alterações (UI própria). Principal risco: leitura de dados da tabela de processos.
