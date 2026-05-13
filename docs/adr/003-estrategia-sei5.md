# ADR-003: Estratégia de compatibilidade com SEI 5

- **Status:** Aceito
- **Data:** 2025-05-13
- **Decisores:** Fork comunitário

## Contexto

O SEI 5 introduziu mudanças no frontend (Bootstrap 4/5, CKEditor 5, SVG no lugar de GIFs, ajustes no DOM) que quebraram diversas funcionalidades da extensão. O projeto original foi abandonado antes de concluir a adaptação — existem 74 referências a `isSEI_5` no código mas a maioria sem implementação completa.

## Decisão

Adotar uma estratégia de compatibilidade **incremental e não destrutiva**:

1. **Manter suporte simultâneo ao SEI 4.x e 5.x** — não abandonar usuários em versões anteriores
2. **Usar branches condicionais** com as flags existentes (`isSEI_5`, `isNewSEI`) em vez de criar versões separadas da extensão
3. **Centralizar seletores DOM** em `docs/seletores-sei.md` antes de alterar código — mapear primeiro, codar depois
4. **Priorizar por impacto** — corrigir primeiro o que afeta mais funcionalidades (árvore, editor, layout)
5. **Usar o código-fonte do SEI 5** para mapear os novos seletores sem depender de uma instância em produção

## Padrão de implementação

```javascript
// Em vez de código espalhado, centralizar no início do módulo:
var SEI = {
    arvoreFrame:   isSEI_5 ? 'novoIdSei5'   : 'ifrArvore',
    visualizacao:  isSEI_5 ? 'novoIdVis5'   : 'ifrConteudoVisualizacao',
    painelEsq:     isSEI_5 ? '#novoPainel5' : '#divInfraAreaTelaE',
    // ...
};
```

## Consequências

**Positivas:**
- Usuários de SEI 4.x não são impactados
- Abordagem incremental — cada módulo pode ser corrigido independentemente
- Código existente (4.2 MB de JS) é aproveitado
- Reduz o tempo até a primeira versão funcional no SEI 5

**Negativas:**
- Acúmulo de condicionais ao longo do tempo (técnica de dívida)
- Testes manuais necessários em duas versões do SEI
- Sem testes automatizados para detectar regressões

## Alternativas consideradas

**Reescrita completa:**
- Stack moderna (TypeScript, Vite, React/Solid)
- Tempo estimado: 3-6 meses para paridade de funcionalidades
- Risco: produto sem usuários durante a reescrita
- Decisão: **descartada** — custo muito alto para a fase atual

**Versão separada para SEI 5:**
- Manter dois repositórios ou dois branches principais
- Duplicação de código e esforço de manutenção dobrado
- Decisão: **descartada** — inviável para um projeto comunitário pequeno

## Revisão planejada

Reavaliar arquitetura (possível reescrita modular) após:
1. SEI 5 ter ≥ 80% das funcionalidades funcionando
2. Base de contribuidores estabelecida
3. Testes de regressão automatizados em vigor
