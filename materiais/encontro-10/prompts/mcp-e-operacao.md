# Prompts — MCP próprio e operação

> **Encontro 10 · mão na massa.**
> Tornar o sistema operável: proteger segredos, testar o backup, ligar
> monitoramento e expor suas ferramentas via MCP.
>
> ⚠️ A nota-fonte do E10 não estava disponível nas anotações no momento da
> geração; estes prompts foram reconstruídos a partir das referências ao E10 nos
> demais encontros. Ajuste quando a nota original voltar.

---

## 1. Proteger segredos (`.gitignore` + varredura)

```text
Crie/ajuste o .gitignore para garantir que .env, .env.local, chaves (*.pem) e
node_modules/ nunca sejam commitados. Depois, faça uma varredura no repositório
(inclusive no histórico) procurando qualquer segredo que já tenha vazado: chaves
de API, tokens, senha de banco, service role do Supabase. Liste o que encontrar e
me diga quais eu preciso ROTACIONAR (apagar o commit não basta — o histórico guarda).
```

> A chave anon (pública) do Supabase pode ir ao navegador — é o RLS do E05 que a
> torna segura. A service role **nunca** vai ao navegador nem ao repositório.

---

## 2. Testar a restauração do backup

```text
Me guie para testar a restauração do backup do Supabase num projeto SEPARADO (nunca
no de produção). Quero confirmar que os dados voltam inteiros. Ajude-me também a
definir, e anotar no CLAUDE.md, a retenção (mínimo 5 anos para o que fundamentou
recomendação — art. 23 CVM 19) e a frequência com que eu testo a restauração.
```

---

## 3. Monitoramento das automações

```text
Revise os fluxos agendados (E06/E08/E09) e garanta que cada um:
- registra a execução (quantas linhas, quando);
- em caso de falha, me manda um aviso (e-mail) e não falha em silêncio.
Sugira também um monitor de disponibilidade (uptime) para o site do E11. Os alertas
de operação seguem os três critérios do E07 — filtro apertado, só o que exige ação.
```

---

## 4. Expor uma ferramenta via MCP

```text
Quero expor minhas ferramentas ao agente via MCP, para não reexplicar o processo a
cada conversa. Comece por uma: `buscar_cliente(nome)` que consulta a tabela
`cliente` respeitando o RLS (E05). Mostre como registrar o MCP e me explique como
o agente passa a chamá-la. A ferramenta herda as regras do sistema — nada de
atalho para a IA decidir (revisão do E09 continua valendo).
```

E, se for usar o MCP da Mont:

```text
Me ajude a conectar o MCP da Mont (catálogo, Solidez Bancária, Wealth Plan) ao meu
agente, e confirme quais ferramentas ele passa a oferecer.
```

---

## Verificação

- [ ] `.env` no `.gitignore`; varredura feita; chaves vazadas rotacionadas
- [ ] Restauração de backup testada num projeto separado; retenção definida
- [ ] Cada fluxo agendado avisa quando falha; uptime do site monitorado
- [ ] Ao menos uma ferramenta exposta via MCP e usada pelo agente
