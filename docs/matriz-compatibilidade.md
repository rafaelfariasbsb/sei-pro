# Matriz de Compatibilidade

Status de cada funcionalidade por versão do SEI e navegador.

> **Como esta matriz foi preenchida (11/08/2026).** A coluna SEI 5.x foi verificada em instância **SEI 5.0.3** (homologação) e em ambiente local **SEI 5.0.0**; a coluna SEI 4.1, em instância **4.1** em produção. Só recebem ✅ os itens **efetivamente exercitados** — o restante permanece 🔲 (não testado), e não deve ser lido como "quebrado". Contribuições de teste são bem-vindas: veja [como contribuir](../CONTRIBUTING.md).

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
| Redimensionar árvore automaticamente | ✅ | ✅ | ✅ |
| Dividir informações da árvore em duas linhas | ✅ | ✅ | 🔲 |
| **Processos** | | | |
| Favoritos | ⚠️ | ⚠️ | ⚠️ |
| Controle de Prazos (Gantt) | ⚠️ | ⚠️ | ⚠️ |
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
| Informações adicionais na árvore | ✅ | ✅ | ✅ |
| **IA** | | | |
| Integração ChatGPT / OpenAI | ✅ | ✅ | 🔲 |
| **Jurídico / Normativo** | | | |
| Legística (enumerar normas) | ❌ | ❌ | ❌ |
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

## Notas sobre os itens marcados ⚠️ e ❌

**Favoritos — ⚠️ em todas as versões.** Favoritar, listar e organizar funcionam **localmente** (os dados ficam em `localStorage`). O que não funciona é a **sincronização entre dispositivos**, que depende do backend do projeto original.

**Controle de Prazos — ⚠️ em todas as versões.** O recurso funciona, mas a **descoberta automática do servidor** dependia do domínio `seipro.app`, hoje desativado. Quem já tem perfil configurado segue usando normalmente; para configurar do zero é preciso informar manualmente **URL do Servidor** e **Chave de Acesso** nas opções da extensão.

**Legística — ❌ em todas as versões.** Dependia inteiramente de `seipro.app/legis/`, que não responde mais. Não é uma incompatibilidade com o SEI 5: quebra igual no SEI 4.

**Editor de documentos — a coluna SEI 5.x merece uma ressalva.** O SEI 5 mantém **CKEditor 4 e CKEditor 5**, escolhidos por documento (`documento.sta_editor`) ou por unidade (parâmetro `SEI_NOVO_EDITOR_UNIDADES`). Onde o CK4 estiver em uso, as funcionalidades de editor tendem a funcionar como no SEI 4. Onde o CK5 estiver ativo, **não funcionam** — a adaptação está prevista e é o maior item em aberto do projeto. Detalhes técnicos em [especificacao-sei5.md](./especificacao-sei5.md).

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
