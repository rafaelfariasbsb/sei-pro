# ADR-002: Google Sheets como backend remoto

- **Status:** Aceito (herdado do projeto original)
- **Data:** 2025-05-13
- **Decisores:** Fork comunitário

## Contexto

O módulo de Projetos (Kanban + Gantt) permite armazenar dados de forma colaborativa entre usuários da mesma equipe. Foi necessário escolher um backend remoto que não exigisse infraestrutura própria.

## Decisão

Usar a API do Google Sheets como banco de dados remoto opcional. Cada usuário/equipe configura sua própria planilha e chave de API no Google Cloud.

## Consequências

**Positivas:**
- Zero custo de infraestrutura para o projeto
- Usuários têm controle total dos seus dados (LGPD)
- Familiaridade — servidores públicos já usam Google Workspace
- A extensão não precisa de servidor próprio

**Negativas:**
- Configuração complexa para o usuário final (Google Cloud Console, chave de API)
- Dependência de serviço externo (Google)
- Performance limitada comparada a um banco de dados real
- Sem autenticação — qualquer pessoa com a chave pode acessar os dados
- Funcionalidade documentada como "momentaneamente descontinuada" no projeto original

## Alternativas consideradas

**Backend próprio (API REST):**
- Maior controle e performance
- Exigiria servidor, banco de dados, autenticação — custo e complexidade altos
- Fora do escopo de uma extensão de browser comunitária

**Supabase / Firebase:**
- Mais simples que Google Sheets para o desenvolvedor
- Ainda exigiria conta e configuração do usuário
- Dependência de plataforma proprietária

## Revisão planejada

Reavaliar se/quando o projeto tiver recursos para manter infraestrutura própria ou encontrar um parceiro de hospedagem.
