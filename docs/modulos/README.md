# Documentação dos Módulos

Esta pasta contém a documentação detalhada de cada módulo JavaScript da extensão SEI Pro.

## Módulos

| Arquivo | Documento | Tamanho | Status SEI 5 |
|---|---|---|---|
| `sei-functions-pro.js` | [Núcleo Compartilhado](./sei-functions-pro.md) | ~732 KB | ⚠️ Parcial |
| `sei-pro.js` | [Página Principal](./sei-pro.md) | ~198 KB | 🔲 A testar |
| `sei-pro-all.js` | [Todas as Páginas](./sei-pro-all.md) | ~54 KB | 🔲 A testar |
| `sei-pro-editor.js` | [Editor de Documentos](./sei-pro-editor.md) | ~440 KB | ❌ Quebrado |
| `sei-pro-arvore.js` | [Árvore do Processo](./sei-pro-arvore.md) | ~173 KB | 🔲 A testar |
| `sei-pro-favoritos.js` | [Favoritos](./sei-pro-favoritos.md) | ~108 KB | 🔲 A testar |
| `sei-pro-atividades.js` | [Controle de Prazos](./sei-pro-atividades.md) | ~2.1 MB | 🔲 A testar |
| `sei-pro-ai.js` | [Inteligência Artificial](./sei-pro-ai.md) | ~114 KB | 🔲 A testar |
| `sei-pro-docs-lote.js` | [Documentos em Lote](./sei-pro-docs-lote.md) | ~67 KB | 🔲 A testar |
| `sei-pro-projetos.js` | [Projetos / Kanban](./sei-pro-projetos.md) | ~136 KB | 🔲 A testar |
| `sei-pro-icons.js` | [Ícones](./sei-pro-icons.md) | ~28 KB | 🔲 A testar |
| `sei-pro-prescricoes.js` | [Prescrições](./sei-pro-prescricoes.md) | ~27 KB | 🔲 A testar |
| `sei-legis.js` | [Legística](./sei-legis.md) | ~35 KB | 🔲 A testar |

**Legenda:**
- ✅ Funcionando no SEI 5
- ⚠️ Parcialmente funcionando
- ❌ Quebrado no SEI 5
- 🔲 Não testado

## Contextos de injeção

```
init_all.js      → sei-pro-all.js          (todas as páginas SEI)
init.js          → sei-pro.js              (lista de processos)
                 → sei-pro-favoritos.js
                 → sei-pro-atividades.js
                 → sei-pro-ai.js
                 → sei-pro-docs-lote.js
init_arvore.js   → sei-pro-arvore.js       (iframe da árvore)
init_visual*.js  → sei-pro-editor.js       (iframe do editor/visualizador)
```

Todos os módulos dependem de `sei-functions-pro.js` como base compartilhada.
