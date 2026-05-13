# Plano de Testes

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

---

## 1. Objetivos

- Garantir que funcionalidades existentes continuem operando no SEI 4.x após qualquer alteração
- Validar compatibilidade de cada módulo com o SEI 5.x (critério de aceitação do fork)
- Detectar regressões antes de publicar novas releases
- Documentar comportamento esperado como referência para novos contribuidores

---

## 2. Estratégia de Testes

Dado que a extensão não possui suite de testes automatizados, a estratégia atual é baseada em **testes manuais estruturados** com casos de teste documentados. A introdução de testes automatizados é um objetivo futuro (ver [ADR-001](./adr/001-sem-build-system.md)).

### 2.1 Níveis de teste

| Nível | Descrição | Execução |
|---|---|---|
| **Smoke test** | Verificação rápida das funcionalidades críticas após qualquer mudança | A cada PR |
| **Regressão modular** | Teste completo de um módulo alterado | A cada PR que toca o módulo |
| **Regressão completa** | Todos os casos de teste | Antes de cada release |
| **Compatibilidade de versão** | Testes no SEI 4.1 e SEI 5.x | Antes de cada release |

### 2.2 Ambientes de teste

| Ambiente | Versão SEI | Navegador | Obrigatório para release |
|---|---|---|---|
| Produção SEI 4.1 | 4.1.x | Chrome | Sim |
| Produção SEI 5.x | 5.0.x | Chrome | Sim |
| Firefox SEI 5.x | 5.0.x | Firefox | Recomendado |
| Edge SEI 5.x | 5.0.x | Edge | Recomendado |

---

## 3. Critérios de Aceitação para Release

Uma versão está **pronta para release** quando:

- [ ] Todos os casos de smoke test passam no SEI 4.1 (Chrome)
- [ ] Todos os casos de smoke test passam no SEI 5.x (Chrome)
- [ ] Nenhum erro no console do navegador em nenhum módulo testado
- [ ] A funcionalidade alterada ou corrigida passou no caso de teste correspondente
- [ ] CHANGELOG atualizado com as mudanças

---

## 4. Smoke Test — Verificação Rápida

Execute este checklist mínimo antes de qualquer commit em `main`.

### 4.1 Inicialização

| # | Verificação | SEI 4.1 | SEI 5 |
|---|---|---|---|
| S01 | Extensão carrega sem erros no console | | |
| S02 | Versão do SEI detectada corretamente (`sessionStorage.versaoSei`) | | |
| S03 | Ícone da extensão aparece na barra do navegador | | |
| S04 | Página de opções abre sem erros | | |

### 4.2 Layout

| # | Verificação | SEI 4.1 | SEI 5 |
|---|---|---|---|
| S05 | Estilo Avançado aplicado (sem quebra de layout) | | |
| S06 | Modo Noturno funciona | | |

### 4.3 Processos

| # | Verificação | SEI 4.1 | SEI 5 |
|---|---|---|---|
| S07 | Lista de processos carrega normalmente (sem travamento) | | |
| S08 | Ícone de favorito aparece ao lado dos processos | | |
| S09 | Favoritar e desfavoritar um processo funciona | | |

### 4.4 Árvore de Documentos

| # | Verificação | SEI 4.1 | SEI 5 |
|---|---|---|---|
| S10 | Árvore de documentos carrega e exibe normalmente | | |
| S11 | Menu rápido aparece ao passar o mouse sobre documentos | | |

### 4.5 Editor

| # | Verificação | SEI 4.1 | SEI 5 |
|---|---|---|---|
| S12 | Editor de documentos abre sem erros | | |
| S13 | Barra de ferramentas da extensão aparece no editor | | |
| S14 | Autossalvamento ativado (indicador aparece) | | |

---

## 5. Casos de Teste Detalhados

### CT-01: Detecção de versão do SEI

**Pré-condição:** Extensão instalada; nenhuma entrada em `sessionStorage.versaoSei`.  
**Passos:**
1. Abrir o SEI no navegador
2. Abrir DevTools > Console
3. Executar `sessionStorage.getItem('versaoSei')`

**Resultado esperado:** String com a versão do SEI (ex: `"5.0.3"` ou `"4.1.5"`)  
**Resultado SEI 4.1:** `"4.1.x"`  
**Resultado SEI 5:** `"5.0.x"`

---

### CT-02: Favoritar processo

**Pré-condição:** Usuário na lista de controle de processos.  
**Passos:**
1. Clicar no ícone ★ ao lado de um processo
2. Preencher descrição no formulário exibido
3. Clicar em "Salvar"
4. Recarregar a página

**Resultado esperado:**
- Processo aparece na seção "Favoritos" no topo da lista
- Após recarregar, favorito persiste
- `localStorage.getItem('seiPro_favoritos')` contém o processo favoritado

---

### CT-03: Nota de rodapé no editor

**Pré-condição:** Usuário editando um documento no SEI.  
**Passos:**
1. Posicionar cursor no meio de um parágrafo
2. Clicar em "Nota de Rodapé" na barra da extensão
3. Digitar o texto da nota
4. Confirmar

**Resultado esperado:**
- Número sobrescrito inserido na posição do cursor
- Nota aparece no rodapé do documento com numeração correspondente
- Documento salvo preserva a nota

---

### CT-04: Autossalvamento

**Pré-condição:** Usuário editando um documento; autossalvamento ativo nas configurações.  
**Passos:**
1. Abrir documento para edição
2. Digitar qualquer texto
3. Aguardar o intervalo configurado (padrão: 30s)

**Resultado esperado:**
- Indicador "Salvo automaticamente às HH:MM" aparece
- `sessionStorage` contém o rascunho com chave referente ao número do documento

---

### CT-05: Controle de Prazos — Cadastro e visualização

**Pré-condição:** Usuário na lista de processos.  
**Passos:**
1. Abrir o painel de Controle de Prazos
2. Clicar em "Novo Prazo"
3. Preencher: processo, título, data início (hoje), data fim (próxima semana)
4. Salvar
5. Verificar visualização no Gantt

**Resultado esperado:**
- Barra do prazo aparece no gráfico de Gantt na posição correta
- Status exibido como "Pendente"
- Prazo persiste após recarregar a página

---

### CT-06: Ações em Lote

**Pré-condição:** Usuário na lista de processos com pelo menos 3 processos visíveis.  
**Passos:**
1. Ativar modo de seleção múltipla
2. Selecionar 3 processos
3. Escolher ação "Marcar como visto"
4. Confirmar

**Resultado esperado:**
- Ação executada nos 3 processos
- Relatório exibe "3 de 3 com sucesso"
- Processos refletem a mudança visualmente

---

### CT-07: Integração com IA

**Pré-condição:** Chave de API OpenAI configurada nas opções.  
**Passos:**
1. Abrir um documento no visualizador
2. Selecionar um trecho de texto
3. Clicar em "Ferramentas IA" > "Resumir"

**Resultado esperado:**
- Painel lateral exibe resumo gerado pela API
- Opções "Aplicar", "Copiar" e "Descartar" funcionam

---

### CT-08: Compatibilidade de seletores SEI 5

**Pré-condição:** SEI 5.x; extensão instalada.  
**Passos:**
1. Abrir DevTools > Console no SEI 5
2. Executar verificações de seletores críticos:
   ```javascript
   // Verificar seletores — adaptar conforme mapeamento em docs/seletores-sei.md
   console.log('ifrArvore:', document.getElementById('ifrArvore'));
   console.log('Painel esq:', document.getElementById('divInfraAreaTelaE'));
   ```

**Resultado esperado:** Elementos encontrados com valores não-nulos.

---

## 6. Testes de Regressão por Módulo

Para cada PR que alterar um módulo, executar os casos correspondentes:

| Módulo alterado | Casos de teste a executar |
|---|---|
| `sei-functions-pro.js` | CT-01, S01–S14 (smoke completo) |
| `sei-pro-favoritos.js` | CT-02, S08, S09 |
| `sei-pro-editor.js` | CT-03, CT-04, S12, S13, S14 |
| `sei-pro-atividades.js` | CT-05 |
| `sei-pro-docs-lote.js` | CT-06 |
| `sei-pro-ai.js` | CT-07 |
| `sei-pro-arvore.js` | S10, S11 |

---

## 7. Registro de Resultados

Para cada release, preencher o template abaixo e incluir como artefato na release do GitHub:

```markdown
## Relatório de Testes — vX.Y.Z

**Data:** AAAA-MM-DD  
**Testado por:** @usuario  

### Ambiente
- SEI versão: X.X.X
- Navegador: Chrome/Edge/Firefox X.X
- OS: Linux/Windows/macOS

### Smoke Test
| Caso | Resultado | Observação |
|------|-----------|------------|
| S01  | ✅ / ❌   |            |
...

### Casos específicos
| Caso | Resultado | Observação |
|------|-----------|------------|
| CT-0x | ✅ / ❌  |            |

### Problemas encontrados
- [ ] Issue #XX: descrição
```
