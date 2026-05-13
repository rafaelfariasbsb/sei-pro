# Gestão de Riscos

**Projeto:** SEI Pro — Fork Comunitário  
**Versão:** 1.0  
**Data:** 2025-05-13

---

## 1. Metodologia

Riscos são avaliados por:
- **Probabilidade:** Baixa (1) / Média (2) / Alta (3)
- **Impacto:** Baixo (1) / Médio (2) / Alto (3)
- **Exposição = Probabilidade × Impacto** (1–9)

Prioridade de resposta:
- 7–9: **Crítico** — ação imediata necessária
- 4–6: **Alto** — plano de mitigação obrigatório
- 1–3: **Médio/Baixo** — monitorar

---

## 2. Registro de Riscos

### R01 — Mudanças no DOM do SEI quebram a extensão

| Campo | Valor |
|---|---|
| **Categoria** | Técnico |
| **Probabilidade** | Alta (3) |
| **Impacto** | Alto (3) |
| **Exposição** | 9 — Crítico |
| **Descrição** | O SEI é atualizado pelo SERPRO/TIC. Qualquer alteração em IDs, classes ou estrutura de iframes pode quebrar funcionalidades sem aviso prévio. Já ocorreu na transição 4.0→4.1 e na adoção do Bootstrap no SEI 5. |
| **Mitigação** | Centralizar todos os seletores em `docs/seletores-sei.md`; usar variáveis de configuração por versão em vez de strings literais espalhadas no código; monitorar releases do SEI. |
| **Plano de contingência** | Manter lista de issues abertas por versão do SEI; publicar hotfix em até 48h após identificar quebra em produção. |
| **Responsável** | Mantenedores do fork |

---

### R02 — Projeto sem contribuidores ativos

| Campo | Valor |
|---|---|
| **Categoria** | Projeto |
| **Probabilidade** | Média (2) |
| **Impacto** | Alto (3) |
| **Exposição** | 6 — Alto |
| **Descrição** | O projeto original foi abandonado pelo autor. Este fork pode ter o mesmo destino se não houver engajamento da comunidade. Com um único mantenedor, qualquer ausência paralisa o projeto. |
| **Mitigação** | Documentação de alta qualidade para reduzir barreira de entrada; label "good first issue" em issues simples; engajamento ativo no ParticiPEN e comunidades SEI. |
| **Plano de contingência** | Se inativo por > 6 meses, adicionar aviso no README indicando o status; transferir repositório para organização comunitária se houver interessados. |
| **Responsável** | @rafaelfariasbsb |

---

### R03 — Mudança na política do Manifest V3 (MV3)

| Campo | Valor |
|---|---|
| **Categoria** | Técnico / Regulatório |
| **Probabilidade** | Baixa (1) |
| **Impacto** | Alto (3) |
| **Exposição** | 3 — Médio |
| **Descrição** | O Google pode alterar ou restringir permissões do MV3 (como fez ao deprecar MV2), exigindo refatoração da extensão. O Firefox tem política diferente do Chrome para algumas APIs. |
| **Mitigação** | Manter o uso de APIs padrão do WebExtensions; evitar APIs proprietárias do Chrome quando houver equivalente padrão; monitorar anúncios do Chromium e Mozilla. |
| **Plano de contingência** | Avaliar impacto de cada mudança de política; manter compatibilidade Firefox como alternativa ao Chrome. |
| **Responsável** | Mantenedores |

---

### R04 — API OpenAI ou Google Sheets muda / descontinua

| Campo | Valor |
|---|---|
| **Categoria** | Dependência externa |
| **Probabilidade** | Média (2) |
| **Impacto** | Médio (2) |
| **Exposição** | 4 — Alto |
| **Descrição** | A integração com IA depende da API da OpenAI (sujeita a mudanças de preço, autenticação e endpoints). O módulo de Projetos usa Google Sheets API, igualmente sujeita a breaking changes. |
| **Mitigação** | Isolar chamadas de API em funções específicas para facilitar substituição; documentar claramente que estas são funcionalidades opcionais. |
| **Plano de contingência** | Para IA: suportar outros provedores (Anthropic, Google Gemini) via configuração. Para Google Sheets: manter modo local como padrão e backend remoto como opcional. |
| **Responsável** | Mantenedores |

---

### R05 — Vulnerabilidade de segurança na extensão

| Campo | Valor |
|---|---|
| **Categoria** | Segurança |
| **Probabilidade** | Baixa (1) |
| **Impacto** | Alto (3) |
| **Exposição** | 3 — Médio |
| **Descrição** | Uma vulnerabilidade XSS na extensão poderia ser explorada para exfiltrar conteúdo de documentos do SEI (potencialmente sigilosos). A extensão tem acesso ao DOM completo do SEI, incluindo conteúdo de documentos. |
| **Mitigação** | Uso obrigatório de DOMPurify para qualquer HTML dinamicamente gerado; revisão de segurança em PRs que manipulam HTML; nunca usar `innerHTML` sem sanitização. |
| **Plano de contingência** | Processo de divulgação responsável via `SECURITY.md`; publicar patch em até 24h para vulnerabilidades críticas; comunicar usuários via release notes. |
| **Responsável** | Mantenedores |

---

### R06 — Dependências de terceiros desatualizadas com vulnerabilidades

| Campo | Valor |
|---|---|
| **Categoria** | Segurança / Manutenção |
| **Probabilidade** | Alta (3) |
| **Impacto** | Médio (2) |
| **Exposição** | 6 — Alto |
| **Descrição** | As bibliotecas em `dist/js/lib/` são todas vendorizadas e atualizadas manualmente. jQuery 3.4.1 e outras já possuem versões mais recentes com correções de segurança. Sem automação, CVEs passam despercebidos. |
| **Mitigação** | Criar issue de rastreamento para atualização de dependências a cada release; verificar CVEs nas bibliotecas principais no [osv.dev](https://osv.dev). |
| **Plano de contingência** | Priorizar atualização de bibliotecas com CVEs de severidade Alta ou Crítica antes de qualquer release. |
| **Responsável** | Mantenedores |

---

### R07 — Conflito com políticas de TI das organizações usuárias

| Campo | Valor |
|---|---|
| **Categoria** | Regulatório / Organizacional |
| **Probabilidade** | Média (2) |
| **Impacto** | Médio (2) |
| **Exposição** | 4 — Alto |
| **Descrição** | Órgãos públicos com políticas rígidas de segurança da informação podem bloquear o uso de extensões não homologadas. Alguns instalam o SEI via portal corporativo que restringe extensões. |
| **Mitigação** | Documentar claramente que a extensão não coleta dados (LGPD); manter política de privacidade atualizada; disponibilizar instalação manual (sem necessidade de conta na Chrome Store). |
| **Plano de contingência** | Oferecer documentação para que o setor de TI do órgão possa avaliar e homologar a extensão. |
| **Responsável** | Comunidade / @rafaelfariasbsb |

---

### R08 — SEI 5 altera arquitetura frontend radicalmente em versão futura

| Campo | Valor |
|---|---|
| **Categoria** | Técnico |
| **Probabilidade** | Baixa (1) |
| **Impacto** | Alto (3) |
| **Exposição** | 3 — Médio |
| **Descrição** | Se o SEI migrar para SPA (React, Vue, Angular) em versão futura, a abordagem de extensão baseada em DOM imperativo se torna muito mais complexa ou inviável. |
| **Mitigação** | Monitorar roadmap público do SEI; manter contato com a comunidade ParticiPEN onde atualizações são anunciadas. |
| **Plano de contingência** | Avaliar reescrita da extensão usando APIs mais estáveis (ex: MutationObserver, shadow DOM) se arquitetura do SEI mudar significativamente. |
| **Responsável** | Mantenedores |

---

## 3. Dashboard de Riscos

```
IMPACTO
  │
3 │  R01(9)          R05(3)  R02(6)
  │  Crítico         Médio   Alto
  │
2 │          R04(4)  R07(4)  R06(6)
  │          Alto    Alto    Alto
  │
1 │  R08(3)  R03(3)
  │  Médio   Médio
  │
  └──────────────────────────────── PROBABILIDADE
       1        2        3
     Baixa    Média    Alta

(número entre parênteses = exposição)
```

---

## 4. Revisão de Riscos

| Evento gatilho | Ação |
|---|---|
| Nova release do SEI anunciada | Revisar R01; testar smoke tests na versão beta/RC |
| Novo contribuidor ativo | Reavaliar R02 (probabilidade cai) |
| CVE publicado em biblioteca usada | Acionar R06; avaliar urgência de atualização |
| Relato de bug de segurança | Acionar processo de SECURITY.md (R05) |
| Mudança de política do Chrome/Firefox | Revisar R03 |
