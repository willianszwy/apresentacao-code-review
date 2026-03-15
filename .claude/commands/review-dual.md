# /review-2 — Code review paralelo com dois agentes especializados

Você vai fazer um code review completo usando dois agentes especializados rodando em paralelo.

## Passo 1 — Obter o diff

### 1a. Determinar a fonte do diff

O argumento `$ARGUMENTS` pode ser:
- Um nome de branch (ex: `feature/login`)
- Um número de PR (ex: `42`)
- Um link de PR do GitHub (ex: `https://github.com/org/repo/pull/42`)
- Vazio — nesse caso use `HEAD~1..HEAD` (último commit)

### 1b. Obter a descrição do PR — tente as opções abaixo em ordem

**Opção 1 — MCP GitHub** (verifique se está disponível antes de usar):
```
Se o MCP do GitHub estiver disponível na sessão, use-o para buscar o PR pelo número ou URL fornecido.
```

**Opção 2 — GitHub CLI**:
```bash
gh pr view $ARGUMENTS --json title,body,additions,deletions,changedFiles,url 2>/dev/null
```

**Opção 3 — Link direto**:
Se `$ARGUMENTS` for uma URL de PR do GitHub, extraia o número e use o `gh`:
```bash
# extrair número do link e rodar gh pr view <número>
```

**Opção 4 — Perguntar ao usuário**:
Se nenhuma das opções acima funcionar, pergunte:
> "Não consegui obter a descrição do PR automaticamente. Você pode colar o link ou a descrição aqui? (ou pressione Enter para continuar só com o diff)"

### 1c. Obter o diff

Execute em paralelo com a busca do PR:

```bash
git fetch origin 2>/dev/null; git diff origin/main...$ARGUMENTS 2>/dev/null || git diff HEAD~1..HEAD
```

### 1d. Resumo obrigatório

Antes de acionar os agentes, mostre:
```
Arquivos alterados: N
Linhas adicionadas: +N
Linhas removidas: -N
Descrição do PR: [obtida / não disponível]
Fonte do diff: [branch X / PR #N / último commit]
```

---

## Passo 2 — Acionar os dois agentes em paralelo

Dispare **simultaneamente** os dois agentes abaixo, passando o diff completo e a descrição do PR (se disponível):

**Agente 1 — `code-reviewer`**
Prompt:
```
Analise o diff abaixo focando em problemas técnicos: correctness, segurança, performance e qualidade de testes. Ignore lógica de negócio.

[descrição do PR se disponível]

[diff completo]
```

**Agente 2 — `logic-reviewer`**
Prompt:
```
Analise o diff abaixo focando em lógica de negócio, edge cases, quebra de contrato e código não relacionado embutido na mudança (cavalo de troia). Compare o comportamento implementado com a intenção descrita no PR.

[descrição do PR se disponível — se ausente, informe ao agente]

[diff completo]
```

---

## Passo 3 — Apresentar os resultados

Apresente os dois relatórios **separados e identificados**, nesta ordem:

1. Cabeçalho com resumo do diff (arquivos, linhas, fonte)
2. Relatório completo do `code-reviewer` (exatamente como retornado)
3. Relatório completo do `logic-reviewer` (exatamente como retornado)
4. Tabela consolidada de severidade:

| Severidade | code-reviewer | logic-reviewer | Total |
|------------|:-------------:|:--------------:|:-----:|
| 🔴 Crítico  | N | N | N |
| 🟡 Médio    | N | N | N |
| 🟢 Baixo    | N | N | N |

5. Veredicto final: **PODE FAZER MERGE** ou **NÃO FAZER MERGE** (não fazer merge se houver qualquer 🔴 crítico).

**Não mescle os relatórios dos dois agentes. Mantenha-os separados.**
