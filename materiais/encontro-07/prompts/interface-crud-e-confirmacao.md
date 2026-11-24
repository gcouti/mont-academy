# Prompts — Interface de Dados (CRUD) e Confirmação de Reunião

> **Encontro 07 · mão na massa.**
> Parte da tela do advisor que já existe desde o E05 (login + leitura das cinco
> tabelas com RLS). Hoje ela ganha criar/editar/excluir e uma tela inicial com
> resumo do dia e um botão que prepara a confirmação de reunião. **Todos os
> dados vêm do Supabase** — nenhum prompt abaixo pede dado inventado ou
> fixo no código. Princípio de sempre: **o agente mostra o código, você lê,
> você aprova, só então ele aplica.**

---

## 1. Adicionar criar, editar e excluir

```text
Os dados estão no Supabase (o mesmo projeto do E05) — leia e grave de lá,
não invente dado de exemplo nem deixe nada fixo no código.

Na tela do advisor que já existe, adicione criar, editar e excluir para as
cinco tabelas (cliente, interacao, oportunidade, tarefa, posicao).

Regras:
- O formulário de edição vem preenchido com os valores atuais do registro,
  lidos do Supabase — nunca em branco.
- Excluir sempre pede confirmação explícita ("Excluir o cliente X? Isso não
  apaga interações, tarefas ou posições ligadas a ele.") — nunca um clique só.
- owner_id NUNCA é um campo do formulário. Continua vindo de auth.uid(),
  automaticamente, igual ao E05.
- Após cada ação (criar, editar, excluir), mostre uma mensagem de sucesso ou
  de erro — nunca falhe em silêncio.

Mostre o código antes de aplicar e espere minha aprovação.
```

**O que conferir antes de aprovar:**
- [ ] Editar abre com os valores atuais preenchidos
- [ ] Excluir tem uma confirmação explícita, não um clique só
- [ ] `owner_id` não aparece em nenhum formulário — é sempre automático
- [ ] Mensagem de sucesso/erro aparece após cada ação

---

## 2. Reteste o RLS com escrita (não só leitura)

O teste de dois usuários do E05 garantiu que **B não lê** o dado de A. Agora
garanta que **B também não escreve**.

```text
Os dados e as políticas de RLS estão no Supabase (o mesmo projeto do E05) —
teste direto lá, não simule localmente.

Repita o teste de isolamento do E05, agora cobrindo escrita:

1. Como usuário A, crie um cliente de teste.
2. Como usuário B, tente editar esse cliente (mudar o nome).
3. Como usuário B, tente excluir esse cliente.

O resultado esperado é que B NÃO consiga editar nem excluir (deve dar erro de
permissão, não "zero linhas afetadas" silencioso). Se B conseguir, aponte qual
policy está errada e proponha a correção.
```

> **Critério do encontro:** se esse teste não passar sem vazamento de escrita,
> a tela não sai do ambiente de teste.

---

## 3. Tela inicial: resumo do dia + botão "Pedir confirmação de reunião"

```text
Os dados estão no Supabase (o mesmo projeto do E05/E06, já conectado) — leia
de lá, não invente dado de exemplo.

Crie uma tela inicial (home) para o advisor. Ao abrir, ela busca no Supabase
e mostra um resumo do dia: quantas reuniões hoje, quantas tarefas com prazo
essa semana, e a lista das próximas reuniões (tarefa ou interacao com data
futura, do cliente deste advisor).

Em cada reunião da lista, adicione um botão "Pedir confirmação". Ao clicar:

1. Monte uma mensagem curta e educada pedindo confirmação de presença, usando
   só os dados que já estão no registro: nome do cliente, data, horário e
   local/link da reunião.
2. Se faltar algum desses dados, NÃO invente — deixe uma nota "faltou <campo>"
   no lugar.
3. Mostre o rascunho na tela com um botão "copiar". Nenhuma chamada a
   WhatsApp, e-mail ou qualquer API de envio — é só texto para eu copiar e
   mandar pelo canal que eu já uso.
4. Depois de gerar o rascunho, registre uma linha em interacao com
   origem = "e07-confirmacao-reuniao".

Mostre o código antes de aplicar e espere minha aprovação.
```

**O que conferir antes de aprovar:**
- [ ] O resumo do dia reflete o Supabase — não é dado fixo no código
- [ ] O botão só aparece em reuniões **futuras**
- [ ] A mensagem não inventa dado que não está no registro
- [ ] **Nada é enviado automaticamente** — só copiado
- [ ] Fica registrada uma linha em `interacao` com a `origem` correta

---

## 4. (Se você fez o Caminho B do E05 — Lovable)

As mesmas três tarefas acima valem pedindo ao Lovable, não ao Claude Code. A
diferença é **onde você revisa**: antes de aceitar qualquer sugestão de
migração do Supabase, confira se ela não remove `owner_id`, não desativa RLS
e não muda a chave de deduplicação — mesmo que venha descrita como "melhoria".

```text
Os dados estão no Supabase (o mesmo projeto conectado desde o E05) — leia e
grave de lá, não invente dado de exemplo.

Proponha a migração e as mudanças de interface para criar, editar e excluir
nas cinco tabelas, com as mesmas regras: formulário de edição preenchido com
os valores atuais do Supabase, confirmação explícita para excluir, owner_id
sempre automático. Mostre a migração SQL completa antes de aplicar.
```

---

## Verificação final

- [ ] Criar, editar e excluir funcionando nas cinco tabelas
- [ ] Teste de dois usuários passou **para leitura e para escrita**
- [ ] Botão de confirmação gera rascunho correto, sem inventar dado
- [ ] Nenhum envio automático de mensagem — só rascunho + copiar
- [ ] Rascunho fica registrado em `interacao` com `origem`
