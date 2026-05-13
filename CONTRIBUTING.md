# Guia de Contribuição

Obrigado por dedicar seu tempo a contribuir com o SEI Pro! Este documento explica como colaborar com o projeto de forma eficaz.

## Índice

- [Código de Conduta](#código-de-conduta)
- [Como posso contribuir?](#como-posso-contribuir)
- [Configurando o ambiente](#configurando-o-ambiente)
- [Fluxo de trabalho com Git](#fluxo-de-trabalho-com-git)
- [Padrões de código](#padrões-de-código)
- [Reportando bugs](#reportando-bugs)
- [Sugerindo melhorias](#sugerindo-melhorias)
- [Pull Requests](#pull-requests)

---

## Código de Conduta

Este projeto adota o [Código de Conduta do Contribuidor (Contributor Covenant)](./CODE_OF_CONDUCT.md). Ao participar, você concorda em respeitar seus termos.

---

## Como posso contribuir?

### Sem precisar programar
- **Reportar bugs** — Abra uma [issue](https://github.com/rafaelfariasbsb/sei-pro/issues) descrevendo o problema
- **Testar** — Instale o fork, use no seu SEI e reporte o que funciona ou não
- **Documentar** — Melhore ou traduza a documentação
- **Divulgar** — Compartilhe o projeto com outros usuários do SEI

### Programando
- Corrigir bugs reportados nas issues
- Implementar compatibilidade com novas versões do SEI
- Refatorar módulos para facilitar manutenção
- Escrever testes

---

## Configurando o ambiente

### Pré-requisitos
- Google Chrome, Microsoft Edge ou Mozilla Firefox
- Editor de código (VS Code recomendado)
- Git

### Passos

1. **Faça um fork** do repositório no GitHub

2. **Clone seu fork**
   ```bash
   git clone git@github.com:SEU-USUARIO/sei-pro.git
   cd sei-pro
   ```

3. **Configure os remotes**
   ```bash
   git remote add upstream git@github.com:rafaelfariasbsb/sei-pro.git
   ```

4. **Carregue a extensão no navegador**

   **Chrome / Edge:**
   - Acesse `chrome://extensions` (ou `edge://extensions`)
   - Ative o **Modo do desenvolvedor**
   - Clique em **Carregar sem compactação**
   - Selecione a pasta `dist/`

   **Firefox:**
   - Acesse `about:debugging`
   - Clique em **Este Firefox** > **Carregar extensão temporária**
   - Selecione o arquivo `dist/manifest.json`

5. **Edite os arquivos em `dist/js/`** e recarregue a extensão para ver as mudanças

> Não há etapa de build — os arquivos em `dist/` são diretamente executados pelo navegador.

---

## Fluxo de trabalho com Git

```bash
# Mantenha seu fork atualizado
git fetch upstream
git checkout main
git merge upstream/main

# Crie um branch para sua contribuição
git checkout -b fix/nome-do-bug
# ou
git checkout -b feat/nome-da-funcionalidade

# Após suas alterações
git add arquivo-modificado.js
git commit -m "fix: corrige detecção de versão no SEI 5"

# Envie para seu fork
git push origin fix/nome-do-bug
```

Então abra um **Pull Request** no GitHub apontando para `rafaelfariasbsb/sei-pro`.

---

## Padrões de código

### Mensagens de commit

Seguimos o padrão [Conventional Commits](https://www.conventionalcommits.org/pt-br):

| Prefixo | Quando usar |
|---------|-------------|
| `fix:` | Correção de bug |
| `feat:` | Nova funcionalidade |
| `docs:` | Apenas documentação |
| `refactor:` | Refatoração sem mudança de comportamento |
| `chore:` | Tarefas de manutenção (dependências, config) |
| `test:` | Adição ou correção de testes |

**Exemplos:**
```
fix: corrige seletor ifrArvore no SEI 5
feat: adiciona suporte ao CKEditor 5
docs: atualiza matriz de compatibilidade
refactor: centraliza seletores SEI em objeto de configuração
```

### JavaScript

- Use `const` e `let` — evite `var` em código novo
- Prefira funções com nomes descritivos a comentários explicativos
- Ao adicionar suporte a uma nova versão do SEI, use o padrão existente:
  ```javascript
  var meuSeletor = isSEI_5 ? '#novo-id-sei5' : '#id-antigo';
  ```
- Documente **por que** o código faz algo não óbvio, não **o que** ele faz

### Seletores DOM

Ao usar um seletor específico do SEI, registre-o em [`docs/seletores-sei.md`](./docs/seletores-sei.md) com a versão correspondente.

---

## Reportando bugs

Antes de abrir uma issue, verifique se ela já foi reportada. Se não, abra uma nova com:

- **Versão do SEI** (visível no canto inferior direito do sistema)
- **Navegador e versão**
- **Versão da extensão**
- **Descrição do comportamento esperado vs. o que aconteceu**
- **Passos para reproduzir**
- **Prints de tela** (se aplicável)
- **Mensagens de erro** do console do navegador (`F12 > Console`)

Use o template de issue disponível no repositório.

---

## Sugerindo melhorias

Abra uma issue com o prefixo `[Sugestão]` no título e descreva:
- O problema que a melhoria resolveria
- Como você imagina a solução
- Referências ou exemplos de outras ferramentas (se houver)

---

## Pull Requests

### Antes de enviar

- [ ] O código funciona no SEI 4.x (não quebrou nada)
- [ ] O código funciona no SEI 5.x (se aplicável)
- [ ] Testado em pelo menos um navegador (Chrome, Edge ou Firefox)
- [ ] Documentação atualizada se necessário
- [ ] Mensagens de commit seguem o padrão Conventional Commits

### Processo de revisão

1. Abra o PR com uma descrição clara do que foi feito e por quê
2. Um mantenedor revisará e poderá pedir ajustes
3. Após aprovação, o PR será incorporado ao branch `main`

---

Dúvidas? Abra uma [issue](https://github.com/rafaelfariasbsb/sei-pro/issues) ou entre em contato.
