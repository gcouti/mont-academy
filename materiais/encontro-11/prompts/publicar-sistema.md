# Prompts & checklist — Publicar o sistema

> **Encontro 11 · mão na massa.**
> Levar o sistema do `localhost` para `https://seudominio.com.br`. Três atores:
> GitHub (guarda o código), plataforma gerenciada (executa 24h), registrador de
> domínio (diz quem é dono do endereço). Você não configura servidor.

---

## 1. Subir o código para o GitHub

```text
Confirme que meu repositório está limpo e que o .env está no .gitignore (segredo
nunca sobe). Depois me guie para dar push da main para o GitHub e confirmar que
a versão no GitHub é a mesma que roda localmente.
```

---

## 2. Conectar a plataforma e rodar o build

```text
Vou conectar minha plataforma de publicação (Vercel/Netlify/Render) ao repositório
do GitHub. Me explique o que configurar para o primeiro build e, quando ele rodar,
me ajude a ler o log. Se falhar, vamos ler → copiar o erro → corrigir (ex.:
dependência não declarada no package.json) e tentar de novo.
```

> Build quebrado não derruba o que já estava no ar — a versão anterior continua.
> `main` é o que está publicado: trabalhe em branch e leve para `main` só o que funciona.

---

## 3. Cadastrar as variáveis de ambiente (a falha nº 1)

```text
A plataforma não tem minhas chaves porque o .env não sobe. Liste quais variáveis
de ambiente eu preciso cadastrar à mão nas configurações do projeto na plataforma:
URL do Supabase, chave anon do Supabase, webhook do n8n, token do MCP.

IMPORTANTE: só a chave anon (pública) vai para produção/navegador — ela é segura
por causa do RLS (E05). A service role NUNCA vai para a plataforma nem o repositório.
```

> Sintoma clássico: o site sobe mas a tela de clientes fica vazia / erro de conexão.
> Não é o banco — são as variáveis faltando.

---

## 4. Apontar o domínio (DNS + SSL)

```text
Minha plataforma me deu um endereço `nome.vercel.app`. Me guie para apontar meu
domínio próprio para ele no registrador, usando um registro CNAME (preferível ao A,
porque se a plataforma trocar de IP eu não preciso mexer). Confirme que o SSL
(cadeado https) será emitido automaticamente pela plataforma.
```

> Propagação de DNS leva de minutos a horas — "no celular abre e no PC não" é
> cache, não erro. Registre o domínio **5 dias antes** da aula.

---

## Verificação (estar no ar de verdade)

- [ ] Código no GitHub, com push da versão atual
- [ ] Sistema no ar no endereço temporário da plataforma
- [ ] Variáveis cadastradas — a tela de clientes **carrega em produção**
- [ ] DNS (CNAME) apontando o domínio próprio para a plataforma
- [ ] `https://seudominio.com.br` abre com cadeado (pode propagar em casa)

---

## Apresentação final (5 min, formato fixo)

1. Uma tela do sistema — agora no ar, no seu endereço.
2. O antes/depois cronometrado (quanto tempo a tarefa levava × leva hoje).
3. Uma coisa que surpreendeu.
