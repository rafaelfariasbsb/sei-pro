# ADR-001: Ausência de sistema de build

- **Status:** Aceito (herdado do projeto original)
- **Data:** 2025-05-13
- **Decisores:** Fork comunitário

## Contexto

O projeto original foi desenvolvido sem nenhum sistema de build (webpack, rollup, vite, etc.). Os arquivos JavaScript em `dist/js/` são editados diretamente e servidos ao navegador sem transpilação, bundling ou minificação automática.

## Decisão

Manter a ausência de build system no curto prazo para não criar barreira de entrada para novos contribuidores e não introduzir risco de regressão antes de ter compatibilidade com o SEI 5 estabelecida.

## Consequências

**Positivas:**
- Qualquer pessoa com um editor de texto pode contribuir
- Ciclo de feedback imediato: edita → recarrega extensão → testa
- Sem dependência de Node.js ou npm para desenvolvimento básico

**Negativas:**
- Arquivos JS muito grandes sem minificação (`sei-pro-atividades.js` com 2.1 MB)
- Sem TypeScript — erros de tipo descobertos apenas em runtime
- Dependências gerenciadas manualmente (copiar arquivos para `lib/`)
- Sem tree-shaking — código não utilizado permanece no bundle
- Dificulta modularização real (sem `import/export` nativo com resolução de módulos)

## Alternativas consideradas

**Adicionar Vite como build step:**
- Permitiria TypeScript, imports nativos, minificação automática
- Aumentaria a barreira de entrada para contribuidores
- Risco de regressão durante a migração
- Decisão: **adiar até após compatibilidade SEI 5 estar estável**

## Revisão planejada

Reavaliar após:
1. Compatibilidade com SEI 5 estabelecida
2. Pelo menos 3 contribuidores ativos no projeto
