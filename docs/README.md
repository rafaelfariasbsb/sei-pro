# Documentação Técnica — SEI Pro

Bem-vindo à documentação técnica do SEI Pro. Esta pasta contém toda a documentação de engenharia de software e análise de sistema do projeto.

---

## Índice

### Engenharia de Software

| Documento | Descrição |
|---|---|
| [Especificação de Requisitos (ERS)](./requisitos.md) | Requisitos funcionais e não-funcionais completos, organizados por módulo |
| [Casos de Uso](./casos-de-uso.md) | Especificação detalhada dos principais casos de uso com fluxos e exceções |
| [Plano de Testes](./plano-testes.md) | Estratégia de testes, smoke tests, casos de teste detalhados e critérios de release |
| [Gestão de Riscos](./gestao-riscos.md) | Registro de riscos, análise de probabilidade/impacto e planos de mitigação |

### Análise de Sistema

| Documento | Descrição |
|---|---|
| [Arquitetura](./arquitetura.md) | Como a extensão funciona: componentes, contextos de injeção, fluxo de inicialização |
| [Modelo de Domínio](./modelo-dominio.md) | Entidades do sistema, dicionário de dados e regras de negócio |
| [Diagramas](./diagramas.md) | Diagramas de componentes, sequência, estados e fluxo de dados |
| [Seletores DOM do SEI](./seletores-sei.md) | Registro centralizado de seletores DOM por versão do SEI (referência para SEI 5) |
| [Matriz de Compatibilidade](./matriz-compatibilidade.md) | Status de cada funcionalidade por versão do SEI e navegador |

### Referência

| Documento | Descrição |
|---|---|
| [Glossário](./glossario.md) | Definições de termos do domínio SEI e termos técnicos da extensão |
| [Guia de Desenvolvimento](./desenvolvimento.md) | Como montar o ambiente, depurar e adicionar funcionalidades |

### Decisões Arquiteturais (ADR)

| Documento | Decisão |
|---|---|
| [ADR-001](./adr/001-sem-build-system.md) | Ausência de sistema de build |
| [ADR-002](./adr/002-google-sheets-backend.md) | Google Sheets como backend remoto |
| [ADR-003](./adr/003-estrategia-sei5.md) | Estratégia de compatibilidade com SEI 5 |

---

## Como contribuir com a documentação

- Erros ou desatualizações: abra uma [issue](https://github.com/rafaelfariasbsb/sei-pro/issues) com o label `docs`
- Ao alterar seletores DOM no código, atualize [`seletores-sei.md`](./seletores-sei.md)
- Ao testar uma funcionalidade no SEI 5, atualize [`matriz-compatibilidade.md`](./matriz-compatibilidade.md)
- Ao tomar uma decisão arquitetural relevante, crie um novo ADR em `adr/NNN-titulo.md`
