# Meu Fluxo de Code Review com IA

Apresentação sobre como usar o **Claude Code CLI** para automatizar code reviews com agentes especializados.

👉 **[Ver apresentação](https://willianszwy.github.io/apresentacao-code-review)**

---

## O que está neste repo

### Apresentação
`index.html` — apresentação completa em Reveal.js (abre direto no browser, sem build).

### Arquivos de exemplo prontos para usar

```
.claude/
  commands/
    review.md       — /review: abordagem simples, agente único, qualquer stack
    review-dual.md  — /review-dual: dois agentes em paralelo com veredicto de merge
  agents/
    code-reviewer.md   — revisor técnico multi-stack (correctness, segurança, performance)
    logic-reviewer.md  — revisor de negócio (contratos, edge cases, cavalo de troia)
```

---

## Como usar os arquivos no seu projeto

### Opção 1 — Prompt para o Claude Code

Abra o Claude Code na raiz do seu projeto e cole:

**Para o `/review` simples:**
```
Clone o repositório https://github.com/willianszwy/apresentacao-code-review
em um diretório temporário e copie o arquivo .claude/commands/review.md
para a raiz deste projeto. Crie o diretório .claude/commands/ se não existir.
```

**Para o `/review-dual` com dois agentes:**
```
Clone o repositório https://github.com/willianszwy/apresentacao-code-review
em um diretório temporário e instale neste projeto:
- .claude/agents/code-reviewer.md
- .claude/agents/logic-reviewer.md
- .claude/commands/review-dual.md
Crie os diretórios necessários se não existirem.
```

### Opção 2 — Cópia manual

```bash
# Clonar este repo
git clone https://github.com/willianszwy/apresentacao-code-review /tmp/cr-setup

# Copiar o que quiser pro seu projeto
cp /tmp/cr-setup/.claude/commands/review.md      SEU_PROJETO/.claude/commands/
cp /tmp/cr-setup/.claude/commands/review-dual.md SEU_PROJETO/.claude/commands/
cp /tmp/cr-setup/.claude/agents/code-reviewer.md SEU_PROJETO/.claude/agents/
cp /tmp/cr-setup/.claude/agents/logic-reviewer.md SEU_PROJETO/.claude/agents/
```

---

## Como usar

### `/review` — abordagem simples
```bash
claude /review feature/minha-branch
claude /review 42           # pelo número do PR
```
Um agente analisa o diff focando em contratos, edge cases e regras de negócio.

### `/review-dual` — dois agentes em paralelo
```bash
claude /review-dual feature/minha-branch
claude /review-dual 42      # pelo número do PR
```
Dois agentes rodam simultaneamente sobre o mesmo diff:
- **code-reviewer** — correctness, segurança (OWASP), performance, testes
- **logic-reviewer** — lógica de negócio, cavalo de troia, quebra de contrato

Ao final: dois relatórios separados + tabela de severidade + veredicto de merge.

---

## Pré-requisitos

- [Claude Code CLI](https://claude.ai/code) instalado (`npm install -g @anthropic-ai/claude-code`)
- `gh` CLI instalado (para buscar descrição do PR automaticamente)
