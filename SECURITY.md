# Política de Segurança

## Versões com suporte

| Versão | Suportada |
|--------|-----------|
| Fork comunitário (≥ 1.6.1) | Sim |
| Versões originais (≤ 1.6.1) | Não — use este fork |

## Reportando uma vulnerabilidade

**Não abra uma issue pública para vulnerabilidades de segurança.**

Se você descobriu uma vulnerabilidade, envie um e-mail para [rfariasbsb@gmail.com](mailto:rfariasbsb@gmail.com) com:

- Descrição da vulnerabilidade
- Passos para reproduzir
- Impacto potencial
- Sugestão de correção (se houver)

Você receberá uma resposta em até **72 horas**. Trabalharemos com você para entender e corrigir o problema antes de qualquer divulgação pública.

## Considerações de segurança da extensão

Esta extensão:

- **Não coleta dados do usuário** — nenhuma informação é enviada a servidores externos
- **Não armazena credenciais** — o preenchimento automático de senha usa apenas o armazenamento local do navegador
- **Usa DOMPurify** para sanitização de HTML gerado dinamicamente
- **Requer apenas a permissão `storage`** do navegador

A funcionalidade de integração com Google Sheets (Projetos) requer que o usuário configure sua própria chave de API do Google Cloud — nenhuma chave compartilhada é usada.

## Escopo

Vulnerabilidades relevantes incluem:

- XSS (Cross-Site Scripting) injetado por meio da extensão
- Vazamento de dados de documentos do SEI para terceiros
- Escalada de privilégios no contexto do browser
- Injeção de código malicioso via campos de configuração

Fora do escopo:

- Vulnerabilidades no próprio sistema SEI (reporte ao SERPRO)
- Vulnerabilidades em bibliotecas de terceiros já divulgadas publicamente
