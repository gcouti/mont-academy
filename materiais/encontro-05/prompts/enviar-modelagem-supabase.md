# Prompts — Enviar a modelagem para o Supabase

> **Encontro 05 · mão na massa.**
> Sequência de prompts que o aluno cola no Claude Code (com o **Supabase MCP**
> conectado) para sair da aula com **banco modelado, autenticação e RLS ativo**.
> O princípio é sempre o mesmo do E02: **ler → propor → executar**. O agente
> mostra o SQL; você lê e aprova antes de rodar.

---

## 0. Conectar o Supabase (setup, uma vez)

Pré-requisitos: conta no Supabase, **projeto vazio criado na região São Paulo**, e
a estrutura das 5 entidades descrita em `entidades.md` na pasta do curso.

```text
Conecte este projeto ao meu Supabase usando o Supabase MCP. Confirme que você
consegue listar as tabelas do meu projeto (deve estar vazio por enquanto) e me
diga em que região o projeto está. Não crie nada ainda.
```

> Se o MCP pedir credenciais, use o **access token** e a **project ref** do seu
> projeto (Supabase → Settings → API / Access Tokens). Nunca cole chave em
> arquivo versionado — isso é assunto do E10.

---

## 1. Criar as cinco tabelas

```text
Crie no Supabase as cinco tabelas descritas em `entidades.md`, com as colunas do
documento: cliente, interacao, oportunidade, tarefa e posicao.

Regras:
- id é uuid (chave primária, default gen_random_uuid()).
- Use tipos apropriados: text para nome/texto, date/timestamp para datas,
  numeric para valores.
- Relacionamento por cliente_id (uuid) referenciando cliente(id) nas tabelas
  interacao, oportunidade, tarefa e posicao.
- Inclua owner_id uuid em TODAS as cinco tabelas (default auth.uid()).
- faixa_patrimonio é text (faixa, não valor exato). Nada de dados reais ainda.

Mostre o SQL completo antes de executar e espere minha aprovação.
```

**O que conferir no SQL antes de aprovar:**
- [ ] `owner_id uuid` nas cinco tabelas
- [ ] `id uuid` com default, não serial/inteiro
- [ ] `cliente_id` referenciando `cliente(id)` nas quatro tabelas filhas
- [ ] `faixa_patrimonio` é `text`, e não existe coluna `patrimonio` numérica

---

## 2. Ativar a autenticação por e-mail

```text
Ative a autenticação por e-mail (email/senha) neste projeto Supabase. Me explique
em uma frase o que o usuário recebe ao logar (o uid) e confirme que, por
enquanto, qualquer usuário autenticado ainda consegue ver todas as linhas —
porque o RLS ainda não foi ligado.
```

> Lembrete do conceito: **autenticação responde "quem é você?"; autorização
> (RLS) responde "o que você pode ver?"**. Login sem RLS é porta com crachá e sem
> tranca.

---

## 3. Ligar o RLS e criar as políticas (dono do dado)

```text
Ative Row Level Security (RLS) nas cinco tabelas. Em cada uma, crie políticas com
a regra "o dono do dado é quem inseriu": o usuário só pode fazer SELECT, INSERT,
UPDATE e DELETE em linhas onde owner_id = auth.uid().

Garanta também que, no INSERT, o owner_id seja sempre preenchido com auth.uid()
(não confie no cliente mandar o valor certo).

Mostre o SQL de cada policy antes de executar. Espere minha aprovação.
```

**O que conferir antes de aprovar:**
- [ ] RLS **habilitado** nas cinco tabelas (`enable row level security`)
- [ ] Policies cobrindo `select`, `insert`, `update` e `delete` — não só `select`
- [ ] A condição é `owner_id = auth.uid()` (em `using` e em `with check`)

---

## 4. Testar com dois usuários (o teste que não pode ser pulado)

```text
Vamos validar o RLS. Me guie num teste com dois usuários:

1. Crie (ou use) dois usuários de teste, A e B.
2. Como usuário A, insira uma linha fictícia em cliente.
3. Como usuário B, tente ler a tabela cliente.

O resultado esperado é que B NÃO veja a linha de A (deve retornar zero linhas).
Me mostre o resultado dos dois lados e confirme se passou. Se B conseguir ver o
dado de A, aponte qual policy está errada e proponha a correção.
```

> **Critério do encontro:** se esse teste não passar sem vazamento, **dado real
> não entra** até resolver.

---

## 5. (bônus) Estruturar um arquivo desestruturado nos campos

Fecha o ciclo com o E03: pegar um texto cru (uma transcrição) e transformá-lo em
linhas estruturadas — respeitando o RLS que você acabou de criar.

```text
Leia o arquivo `raw/reuniao-2026-11-12.txt` e proponha como inseri-lo no banco:

- Um registro em `cliente` se o cliente ainda não existe.
- Uma linha em `interacao` (data, canal = "reunião", resumo).
- Linhas em `tarefa` e `oportunidade` para o que ficou em aberto na conversa.

Não invente dado que não está no arquivo — se faltar, deixe o campo nulo e me
avise. Mostre o INSERT (com owner_id = auth.uid()) antes de executar e liste, em
uma linha, o que ficou de fora.
```

**O que conferir:** nenhum número foi inventado (ex.: valor exato que não estava
na transcrição), `owner_id` presente no insert, e nada de PII que a régua do E01
manda remover.

---

## Verificação final (antes de sair da aula)

- [ ] Cinco tabelas criadas, com `owner_id` em todas
- [ ] Autenticação por e-mail funcionando (login testado)
- [ ] RLS ativo nas cinco tabelas com a regra "dono do dado"
- [ ] Teste de dois usuários passou **sem vazamento**
