# Prompt — Conectar a uma API externa para buscar dados

> **Encontro 06 · outra fonte de dados.**
> O mesmo pipeline (ler → mapear → normalizar → validar → upsert) vale quando a
> fonte não é um CSV de corretora, mas uma **API** — de um provedor de cotações,
> de outro sistema interno, de uma corretora que oferece integração direta.
> Muda só o passo 1 (ler virar "buscar"); os passos 2 a 5 são os mesmos do
> resto do encontro.

---

## O prompt

```text
Quero conectar a API de <NOME DO PROVEDOR> para buscar <O QUE BUSCAR — ex.:
posição da carteira, cotações, extrato>. Antes de gravar qualquer coisa:

1. Leia a documentação da API (endpoint, autenticação, paginação, limite de
   requisições) e me explique em poucas linhas como funciona.
2. Monte uma chamada de teste, só leitura, e me mostre a resposta crua (sem
   gravar nada ainda).
3. Proponha o mapeamento: qual campo da resposta vira ativo, valor, data e
   classe no esquema de posicao — igual ao que fizemos com o CSV.

A chave/token da API NUNCA aparece no código nem neste chat: leia de uma
variável de ambiente (ex. API_KEY) que eu já configurei. Se precisar dela para
testar, me diga o nome da variável, não peça o valor.

Não grave nada no banco neste passo — só confirme que a leitura funciona e me
mostre o mapeamento proposto.
```

**O que conferir antes de aprovar:**
- [ ] Nenhuma chave, token ou segredo aparece no código ou na conversa
- [ ] A chamada de teste é **só leitura** (GET), nada foi gravado ainda
- [ ] O mapeamento proposto cobre os mesmos campos do esquema de `posicao` (ativo, valor, data, classe)
- [ ] Ficou claro o limite de requisições da API (rate limit) — importa para o passo de agendamento

---

## Depois da leitura: o pipeline continua igual

```text
Agora trate a resposta da API como tratamos o CSV: normalize ativo, data, valor
e classe (mesmas quatro regras), valide linha por linha com relatório antes de
gravar, e faça upsert em posicao pela chave (cliente_id, ativo, data). Mostre o
código antes de rodar.
```

> Dados de API costumam vir "mais limpos" que CSV de corretora, mas **nunca
> assuma isso** — a mesma dor de identidade instável (um ticker com nome
> diferente do seu `ativo-sinonimos.csv`) acontece entre provedores de API
> também. Rode a validação do mesmo jeito.

---

## Armadilhas comuns com API

| Sintoma | Causa | Como resolver |
|---|---|---|
| Chamada funciona no teste e falha no agendamento | Token expirou (muitas APIs usam token de curta duração) | Implementar renovação automática do token, nunca hardcoded |
| Resposta vem paginada e só a primeira página é lida | Paginação não tratada | Percorrer todas as páginas antes de considerar a leitura completa |
| Importação dobra os dados em horário de pico | Rate limit derruba a chamada no meio, script tenta de novo do zero | Checar se já leu aquela página/lote antes de repetir; manter a idempotência do upsert |
| Segredo vazou em log ou print de debug | `API_KEY` impressa "só para depurar" | Nunca logar o valor da variável de ambiente, nem em debug |
