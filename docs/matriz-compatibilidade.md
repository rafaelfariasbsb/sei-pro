# Matriz de Compatibilidade

Status de cada funcionalidade por versão do SEI e navegador.

**Legenda:**
- ✅ Funciona
- ⚠️ Parcial (limitações conhecidas)
- ❌ Quebrado
- 🔲 Não testado
- — Não aplicável

---

## Por versão do SEI

| Funcionalidade | SEI 4.0 | SEI 4.1 | SEI 5.x |
|---|---|---|---|
| **Layout** | | | |
| Estilo Avançado / Modo Noturno | ✅ | ✅ | 🔲 |
| Menu Suspenso | ✅ | ✅ | 🔲 |
| URL Amigável | ✅ | ✅ | 🔲 |
| Redimensionar árvore automaticamente | ✅ | ✅ | 🔲 |
| Dividir informações da árvore em duas linhas | ✅ | ✅ | 🔲 |
| **Processos** | | | |
| Favoritos | ✅ | ✅ | 🔲 |
| Controle de Prazos (Gantt) | ✅ | ✅ | 🔲 |
| Agrupar lista de processos | ✅ | ✅ | 🔲 |
| Histórico de processos visitados | ✅ | ✅ | 🔲 |
| Exportar lista de processos (CSV) | ✅ | ✅ | 🔲 |
| Rolagem infinita na pesquisa | ✅ | ✅ | 🔲 |
| Ações em Lote | ✅ | ✅ | 🔲 |
| Alertar documentos não assinados | ✅ | ✅ | 🔲 |
| Cores personalizadas em Marcadores | ✅ | ✅ | 🔲 |
| **Editor de Documentos** | | | |
| Nota de rodapé | ✅ | ✅ | 🔲 |
| Sumário automático | ✅ | ✅ | 🔲 |
| Quebra de página | ✅ | ✅ | 🔲 |
| Inserir equações (LaTeX) | ✅ | ✅ | 🔲 |
| Tabela rápida | ✅ | ✅ | 🔲 |
| Copiar formatação de texto | ✅ | ✅ | 🔲 |
| Aumentar/reduzir fonte | ✅ | ✅ | 🔲 |
| Alinhar texto | ✅ | ✅ | 🔲 |
| Abrir/editar/remover hiperlinks | ✅ | ✅ | 🔲 |
| Dados do processo no documento | ✅ | ✅ | 🔲 |
| Marca d'água de minuta | ✅ | ✅ | 🔲 |
| Sigilo / tarjas LGPD | ✅ | ✅ | 🔲 |
| Autossalvamento | ✅ | ✅ | 🔲 |
| Ditado por voz | ✅ | ✅ | 🔲 |
| Parágrafos numerados | ✅ | ✅ | 🔲 |
| Referências internas | ✅ | ✅ | 🔲 |
| Teclas de atalho | ✅ | ✅ | 🔲 |
| **Documentos** | | | |
| Duplicar documento | ✅ | ✅ | 🔲 |
| Enviar múltiplos documentos externos | ✅ | ✅ | 🔲 |
| Inserir documento HTML / Google Docs | ✅ | ✅ | 🔲 |
| Editor de imagens | ✅ | ✅ | 🔲 |
| Redimensionar imagens | ✅ | ✅ | 🔲 |
| Reduzir qualidade de imagens | ✅ | ✅ | 🔲 |
| OCR (Tesseract) | ✅ | ✅ | 🔲 |
| Comparar documentos (diff) | ✅ | ✅ | 🔲 |
| Certidão de Sigilo (LGPD) | ✅ | ✅ | 🔲 |
| QR Code | ✅ | ✅ | 🔲 |
| Link curto (TinyURL) | ✅ | ✅ | 🔲 |
| Verificar integridade (Hashcode) | ✅ | ✅ | 🔲 |
| Menu rápido na árvore | ✅ | ✅ | 🔲 |
| Informações adicionais na árvore | ✅ | ✅ | 🔲 |
| **IA** | | | |
| Integração ChatGPT / OpenAI | ✅ | ✅ | 🔲 |
| **Jurídico / Normativo** | | | |
| Legística (enumerar normas) | ✅ | ✅ | 🔲 |
| Link de legislação | ✅ | ✅ | 🔲 |
| **Projetos** | | | |
| Kanban | ✅ | ✅ | 🔲 |
| Gantt de projetos | ✅ | ✅ | 🔲 |
| Backend Google Sheets | ✅ | ✅ | 🔲 |
| **Utilitários** | | | |
| Preenchimento automático de senha | ✅ | ✅ | 🔲 |
| Primeira letra maiúscula (PT-BR) | ✅ | ✅ | 🔲 |
| Estilo de tabela | ✅ | ✅ | 🔲 |
| Ordenar tabelas | ✅ | ✅ | 🔲 |

---

## Por navegador (SEI 4.1)

| Funcionalidade | Chrome | Edge | Firefox |
|---|---|---|---|
| Todas as funcionalidades core | ✅ | ✅ | ✅ |
| Ditado por voz | ✅ | ✅ | ⚠️ |
| OCR (Tesseract) | ✅ | ✅ | 🔲 |

---

## Roadmap de compatibilidade SEI 5

> Priorização baseada no impacto para os usuários.

| Prioridade | Funcionalidade | Motivo |
|---|---|---|
| 🔴 Alta | Árvore do processo | Base de quase todas as outras features |
| 🔴 Alta | Editor de documentos | Maior parte das features de editor |
| 🔴 Alta | Layout / painéis | Afeta todas as páginas |
| 🟡 Média | Favoritos | Muito utilizado |
| 🟡 Média | Controle de Prazos | Muito utilizado |
| 🟡 Média | Ações em Lote | Muito utilizado |
| 🟢 Baixa | Projetos / Kanban | Uso mais restrito |
| 🟢 Baixa | Integração IA | Depende de config externa |

---

> Este documento é atualizado à medida que as funcionalidades são testadas e corrigidas para o SEI 5.
> Contribuições com resultados de testes são bem-vindas — veja [CONTRIBUTING.md](../CONTRIBUTING.md).
