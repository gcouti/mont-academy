# Prompts de rotina — LLM Wiki

> **Encontro 03 · material de apoio.**
> Prompts prontos para o aluno colar conforme a necessidade do dia a dia. São as
> três operações do padrão — **ingest**, **query**, **lint** — mais a produção de
> peça com marca de verificação. Todos operam sobre o `consultor-wiki` montado no
> [setup](setup-estrutura-llm-wiki.md).

---

## 1. Ingest — depois de toda reunião

Transforma uma fonte bruta de `raw/` em páginas de `wiki/`. Rode logo após salvar
a transcrição.

```text
Faça o ingest de `raw/<arquivo>`. Crie ou atualize as páginas de `wiki/` que esse
material afeta. Cite o arquivo de origem em cada afirmação. Ao final, liste em
uma linha o que ficou de fora e por quê.
```

> Antes de rodar: confira a anonimização do arquivo em `raw/` (régua de PII do
> E01). Transcrição é o documento mais cru que existe.

---

## 2. Query — antes de falar com um cliente

Pergunta à base em vez de confiar no que o modelo "acha".

```text
O que a wiki tem sobre <cliente ou tema>? Responda usando só o que está em
`wiki/` e cite as páginas usadas. Se não houver nada, diga "não consta".
```

---

## 3. Produzir peça — com marca de verificação

Usa um modelo de `padroes/` e só o que está na base. O `[VERIFICAR]` transforma
lacuna em marca visível, em vez de invenção silenciosa.

```text
Escreva a carta mensal seguindo `padroes/carta-mensal.md`, usando só o que está
em `wiki/`. Marque como [VERIFICAR] qualquer coisa que você não achou na base.
Não complete com conhecimento geral.
```

---

## 4. Lint — uma vez por semana

Auditoria de consistência da wiki.

```text
Rode o lint do `wiki/`: aponte contradições, páginas órfãs (sem nenhum link de
entrada) e afirmações sem citação de fonte em `raw/`. Liste cada achado com o
arquivo e a linha.
```

---

## Regra que vale para todos

> **Quando sair uma página genérica, não conserte a página: conserte o
> `CLAUDE.md`.** Corrigir a saída resolve hoje; corrigir a regra resolve todas as
> próximas. Toda correção que vem da revisão volta para o `CLAUDE.md` — é o laço
> que faz o sistema melhorar sozinho.

## Limite rígido — planilha é para ler, nunca para calcular

Se a fonte for planilha, **peça**: descrever composição, apontar concentração,
encontrar posições fora da política, gerar as perguntas que ela levanta.
**Nunca peça**: calcular rentabilidade, projetar cenário, somar posições. Se
precisar de número, peça a fórmula e execute na planilha. Número plausível é
número errado.
