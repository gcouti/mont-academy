# Prompts — Importador de CSV de corretora + agendamento no n8n

> **Encontro 06 · mão na massa.**
> Sequência para construir o importador que lê CSV de corretora, normaliza, valida,
> grava em `posicao` sem duplicar — e depois roda sozinho no n8n.
> Princípio-base do encontro: **código gerado por IA, execução sem IA.** O agente
> escreve o código que normaliza; a execução de 3.000 linhas não passa pelo modelo
> (é determinística, tem que dar o mesmo resultado sempre).
>
> Exemplos para testar em `../artefatos/exemplos/`.

---

## 1. Ler o CSV e inferir o esquema (sem alterar nada)

```text
Leia o arquivo `raw/corretora-alfa-2026-11-01.csv` e me diga:
- quais colunas existem;
- quais parecem identificar o ATIVO (código ou nome);
- quais parecem ser VALOR monetário (quantidade, preço, total);
- qual é a DATA de referência, e se ela está na linha ou só no cabeçalho;
- qual o separador e o encoding do arquivo.

Não altere nada e não grave nada ainda. Só me descreva o que viu.
```

> Se aparecer `AÇÃO` virando `AÃÃO` ou `A�O`, é **encoding** (arquivo salvo em
> Latin-1/Windows-1252, lido como UTF-8). Não é corrupção, é tradução errada —
> cole o sintoma no agente que ele resolve.

---

## 2. Normalizar (escrever o código do tratamento)

```text
Escreva um importador (script) que normaliza as linhas deste CSV para o esquema
da tabela `posicao` do curso. Quatro tratamentos:

1. ATIVO: use a tabela `raw/ativo-sinonimos.csv` (origem → canonico) para
   converter qualquer sigla/nome para o ticker canônico. Remova espaços.
2. DATA: converta qualquer formato (dd/mm/aaaa, ISO, "1-Nov-2026", cabeçalho)
   para ISO `aaaa-mm-dd`.
3. VALOR: converta "R$ 1.234,56", "1234.56" e "1,234.56" todos para número
   1234.56 (ponto decimal, sem símbolo nem separador de milhar).
4. CLASSE: normalize "AÇÃO"/"ação"/"Ações" → `acao`; "FII"/"Fundo Imobiliário" →
   `fii`; etc., usando a classe canônica da tabela de sinônimos.

O código faz a execução; você NÃO normaliza linha por linha no chat. Mostre o
script antes de rodar e explique onde ele lê a tabela de sinônimos.
```

---

## 3. Validar e gerar o relatório de inconsistência (barreira antes de gravar)

```text
Antes de gravar qualquer coisa, o importador deve gerar um relatório e NÃO gravar
automaticamente. O relatório mostra:

- quantas linhas foram lidas;
- quantas passaram na validação;
- quantas caíram e por quê (ex.: ativo sem correspondência na tabela de
  sinônimos, valor vazio onde é obrigatório, data impossível).

Rode em modo simulação (dry-run) primeiro: nada é gravado até eu ler o relatório
e aprovar. Veja o formato esperado em
`../artefatos/exemplos/relatorio-inconsistencia-exemplo.txt`.
```

---

## 4. Carga sem duplicar (upsert idempotente)

```text
Implemente a gravação em `posicao` como UPSERT, usando como chave única a
combinação (cliente_id, ativo, data):
- se a linha já existe, atualiza;
- se não existe, insere.

Garanta que rodar o MESMO arquivo duas vezes não cria linhas duplicadas.
Preencha owner_id com auth.uid() (RLS do E05). Mostre o SQL/código antes de rodar.
```

**Teste de idempotência (faça na verificação):** importe, conte as linhas de
`posicao`, importe o mesmo arquivo de novo, conte outra vez. **Número igual = passou.**

---

## 5. Agendar no n8n (fazer rodar sozinha)

Hoje o fluxo é uma **linha reta** — três conceitos só: **gatilho**, **nó**,
**tratamento de erro**. Condicional e IA no fluxo são o E08.

```text
Quero montar no n8n um fluxo que roda o importador sozinho. Monte assim:

- GATILHO: agendamento (cron) — todo dia útil às 6h.
- NÓ 1: rodar o importador (ler + normalizar + validar).
- NÓ 2: se passou na validação, fazer o upsert em `posicao`.
- NÓ 3: registrar a execução (quantas linhas, quando).
- TRATAMENTO DE ERRO: se qualquer nó falhar OU a validação reprovar, PARAR e me
  mandar um e-mail com o relatório. Nunca falhar em silêncio.

Me explique cada nó enquanto monta. Depois rode o fluxo manualmente uma vez para
testar, e só então deixe o gatilho de horário ativo.
```

> **Commit o fluxo:** exporte o `.json` do n8n e salve em
> `consultor-mont/fluxos/` (versionado no git, como manda o E04/E06).

---

## Verificação antes de sair

- [ ] Um CSV importado em `posicao`; rodar de novo **não duplica**
- [ ] O relatório de inconsistência aparece **antes** de qualquer gravação
- [ ] Fluxo n8n salvo, ativo, agendado — e rodou ao menos uma vez com sucesso
- [ ] Regra de idempotência de `posicao` anotada no `CLAUDE.md`

> *Automação não conserta processo ruim. Ela multiplica o processo que você tem.*
