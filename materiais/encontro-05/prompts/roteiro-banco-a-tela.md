# Roteiro de aula: do banco à tela do advisor e do cliente

Cada passo tem **o prompt** para colar no Claude Code, **o que esperar** e **o que conferir antes de aprovar**.
Regra de ouro da aula: o agente **mostra o SQL, você lê, você aprova, só então ele executa**.

Resultado final: um banco Supabase com cinco tabelas e RLS, uma tela do **advisor** (vê todos os seus clientes) e uma tela do **cliente** (vê só o próprio cadastro).

---

## Passo 1. Criar as cinco tabelas

```text
Crie no Supabase as cinco tabelas do modelo de dados (cliente, interacao,
oportunidade, tarefa e posicao), com as colunas do documento de entidades.

Regras:
- id é uuid (chave primária, default gen_random_uuid()).
- Use tipos apropriados: text para nome/texto, date para datas, numeric para valores.
- Relacionamento por cliente_id (uuid) referenciando cliente(id) nas tabelas
  interacao, oportunidade, tarefa e posicao.
- Inclua owner_id uuid em TODAS as cinco tabelas (default auth.uid()).
- faixa_patrimonio é text (faixa, não valor exato). Nada de dados reais ainda.

Mostre o SQL completo antes de executar e espere minha aprovação.
```

**Conferir:** `owner_id` nas cinco · `id` uuid com default · `cliente_id` com FK nas quatro filhas · `faixa_patrimonio` é `text`.

---

## Passo 2. Ativar o login por e-mail e senha

```text
Ative a autenticação por e-mail (email/senha) neste projeto Supabase. Me explique
em uma frase o que o usuário recebe ao logar (o uid) e confirme que, por
enquanto, qualquer pessoa ainda consegue ver todas as linhas, porque o RLS
ainda não foi ligado.
```

**Ideia para explicar:** autenticação responde "quem é você?"; autorização (RLS) responde "o que você pode ver?". O usuário logado recebe um **uid**, e é ele que vai preencher `owner_id`.

**Armadilha:** sem RLS, nem o login é necessário. Quem tem a chave pública lê e altera tudo.

---

## Passo 3. Criar o seu usuário e inserir clientes de exemplo

Antes de pedir os dados: **cadastre-se** (ou crie o usuário no painel em Authentication → Users). O `owner_id` é `not null`, e o agente insere como administrador, sem sessão, então `auth.uid()` volta nulo. O insert precisa do uid de um usuário real.

```text
Meu e-mail de advisor é <SEU-EMAIL>. Busque o meu uid em auth.users.
Gere três clientes FICTÍCIOS (nomes e documentos inventados, faixa_patrimonio
como texto) e mostre o INSERT antes de executar. Use o meu uid como owner_id.
```

**Conferir:** documentos claramente falsos · `owner_id` é o seu uid · nada de dados reais.

---

## Passo 4. Ligar o RLS (advisor vê o que é dele)

```text
Ative Row Level Security nas cinco tabelas. Em cada uma, crie uma política para
o advisor: ele só pode fazer SELECT, INSERT, UPDATE e DELETE em linhas onde
owner_id = auth.uid(). Mostre o SQL completo antes de executar. Depois confirme
que a chave pública, sem login, recebe lista vazia.
```

**Conferir:** `enable row level security` nas cinco · uma policy por tabela · `using` **e** `with check`.

---

## Passo 5. Tela do advisor

```text
Faça uma página HTML simples (sem build) para eu consultar os dados. Tela de
login com e-mail e senha do Supabase. Depois de logar, abas para as cinco
tabelas, com campo para filtrar linhas. Nas tabelas filhas, mostre o nome do
cliente em vez do uuid. Use a chave publishable do projeto, nunca a service_role.
```

**Conferir:** a chave no HTML é a **publishable** · não aparece `service_role` em lugar nenhum.

---

## Passo 6. Dar ao cliente o próprio login

Decisão de negócio, não de tecnologia: **o que o cliente pode ver?** Na aula: só o próprio cadastro. Anotações (`interacao`) e funil (`oportunidade`) são internos.

```text
Quero uma tela onde o cliente entra com o login dele e vê só o próprio cadastro.
Proponha o SQL e espere minha aprovação. Ele deve: (1) adicionar em cliente uma
coluna user_id uuid único referenciando auth.users(id), que liga o cliente à
conta dele; (2) criar uma política de SELECT em cliente com user_id = auth.uid(),
somente leitura; (3) não dar ao cliente nenhum acesso às outras tabelas.
```

**Conferir:** o cliente tem **só SELECT** · nenhuma policy de cliente nas tabelas filhas.

---

## Passo 7. Tela do cliente e conta de teste

```text
Crie a página cliente.html: login com e-mail e senha e, depois de logar, mostra
nome, documento, faixa de patrimônio e perfil do próprio cliente, em somente
leitura. Se a conta não estiver ligada a nenhum cadastro, avise para falar com
o advisor. Guarde a sessão com uma chave de storage diferente da tela do advisor.

Depois crie uma conta de teste para o cliente (outro e-mail meu, por exemplo
meuemail+cliente@...), ligue ao cliente <NOME> pelo user_id e me diga a senha.
```

---

## Passo 8. Provar o isolamento (o momento "uau" da aula)

1. Logue na tela do **advisor**: aparecem os três clientes.
2. Logue na tela do **cliente** com a conta de teste: aparece **só um** cadastro.
3. Sem login, consulte a API com a chave pública: volta `[]`.

Se o cliente enxergar outro nome, o RLS está errado. Pare e revise as policies.

---

## Armadilhas que apareceram de verdade

| Sintoma | Causa | Como resolver |
|---|---|---|
| Após confirmar o e-mail, o navegador vai para `localhost:3000/#access_token=...` | A **Site URL** do Supabase é `localhost:3000` por padrão | Servir a página nessa porta ou ajustar a URL em Authentication → URL Configuration |
| Os dados "sumiram" depois de recriar a conta | `owner_id` guarda o **uid**, não o e-mail; conta nova = uid novo | Atualizar o `owner_id` para o uid atual (e evitar apagar a conta) |
| Insert falha com `owner_id` nulo | `auth.uid()` é nulo fora de uma sessão de usuário | Informar o uid explicitamente |
| Tela funciona sem login | RLS desligado | Passo 4 |

## Segurança: o que **não** fazer na aula

- Não cole URLs com `#access_token=...` nem tokens em chats: dão acesso à conta.
- Não coloque senhas em prompts nem em repositórios.
- Nunca ponha a chave `service_role` em HTML: ela ignora todo o RLS.
- Só dados fictícios até o RLS estar ligado e testado.
