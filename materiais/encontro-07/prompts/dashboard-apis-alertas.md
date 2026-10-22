# Prompts — Dashboard, APIs e Alertas

> **Encontro 07 · mão na massa.**
> Consultas do dashboard, série do Banco Central (SGS) com cache, e o alerta CVM 358.
> Base: a tabela `posicao` populada no E06. Princípio: **o agente escreve a consulta,
> você lê o SQL antes de aprovar** — SQL errado costuma *retornar dado* sem dar erro.

---

## 1. Os quatro indicadores

```text
Escreva as consultas SQL dos quatro indicadores do dashboard, sobre a tabela
`posicao`. Para CADA uma, me mostre o SQL antes de rodar e NÃO altere nada:

1. PL sob gestão: soma de posicao.valor APENAS na data mais recente.
2. Captação líquida do mês: diferença de PL entre a última data e a do mês anterior.
3. Concentração: ranking de clientes por soma em posicao (top 5 + "outros").
4. Alocação: soma agrupada por posicao.classe.

Atenção à armadilha: somar sem filtrar a data mais recente soma todas as
fotografias históricas e infla o PL. Garanta o filtro de data em 1, 3 e 4.
```

> Conferência de ordem de grandeza: você conhece seu PL aproximado. Se vier 3×
> maior, a consulta está errada — não o seu negócio cresceu.

---

## 2. Série do Banco Central (SGS) com cache

```text
Consulte a Selic atual pela API do SGS do Banco Central (série 4189) e exiba na
home do dashboard, ao lado do PL sob gestão. Implemente cache de 1 hora — a Selic
não muda a cada refresh. Mostre na tela "de quando" é o dado (timestamp do cache).
Se a API falhar, mostre o último valor em cache marcado como "dado de ontem".
```

Depois, grave a regra no `CLAUDE.md` para valer nas próximas chamadas:

```text
Adicione ao CLAUDE.md a regra: "Toda API externa tem cache. Nunca bater na API a
cada carregamento; mostrar sempre a data do dado na tela."
```

---

## 3. Alerta CVM 358 (detecção de mudança de estado)

Reaproveita o importador e o fluxo n8n do E06 — outra fonte, mais um nó.

```text
Monte um alerta sobre o Formulário 358 da CVM (negociação de administradores de
companhias abertas), reusando o padrão do importador do E06:

1. Carga diária às 6h no mesmo fluxo n8n, baixando o arquivo do portal de dados
   abertos da CVM.
2. Comparar com a carga anterior e identificar o que apareceu de novo.
3. Cruzar as empresas novas com as empresas que aparecem na posicao dos meus
   clientes (só me interessa o que toca a minha carteira).
4. Se houver cruzamento, gerar notificação; se não, arquivar em silêncio.

Antes de ligar, aplique os três critérios de alerta (ação? hoje? aguenta 20×/semana?)
e nasça com filtro apertado — só dispara no cruzamento do passo 3.
```

---

## 4. Marcar visão do cliente × visão interna

```text
Para cada gráfico e alerta do dashboard, marque no código se é "visão do cliente"
(o cliente pode ver sobre a própria carteira) ou "visão interna" (só o consultor:
concentração agregada, captação, ranking). Crie um modo de apresentação que, ao
ser ligado, esconde tudo que é visão interna — para eu abrir na frente do cliente
sem vazar dado de outros clientes.
```

---

## Verificação

- [ ] Quatro indicadores calculados a partir da `posicao`, com filtro de data
- [ ] Selic (ou outra série SGS) no dashboard, com cache e data visível
- [ ] Cada indicador mostra **de quando é o dado**
- [ ] Um alerta configurado, que passou pelos **três critérios**
- [ ] Cada gráfico marcado como visão do cliente ou interna
