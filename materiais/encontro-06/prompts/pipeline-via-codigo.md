# Prompt — Pipeline de ingestão via código (alternativa ao n8n)

> **Encontro 06 · ferramentas de automação.**
> O curso demonstra o n8n (canvas visual), mas o mesmo pipeline — ler, mapear,
> normalizar, validar, upsert — pode ser **código puro**, versionado no git e
> agendado por cron ou CI/CD. Use este prompt se preferir tudo em código em vez
> do canvas visual. Vale a mesma regra do resto do encontro: **código gerado
> por IA, execução sem IA** — o agente escreve o script; quem roda é o cron,
> não o modelo.

---

## O prompt

```text
Quero montar o pipeline de ingestão do CSV de corretora inteiramente em código,
sem usar n8n. Crie um script (na linguagem que já é usada no projeto) que faça:

1. Ler o(s) arquivo(s) novos em raw/, detectando separador e encoding.
2. Mapear as colunas para o esquema de posicao, usando raw/ativo-sinonimos.csv
   para resolver o ativo.
3. Normalizar ativo, data, valor e classe (as mesmas quatro regras do E06).
4. Validar linha por linha e gerar um relatório de inconsistência. Se houver
   qualquer linha reprovada, NÃO grava nada — só mostra o relatório.
5. Se passou, fazer upsert em posicao usando a chave única
   (cliente_id, ativo, data). Preencha owner_id com um uid fixo de
   configuração — não existe sessão de usuário logado rodando por cron.
6. Registrar um log de cada execução: quando rodou, quantas linhas lidas,
   quantas gravadas, resultado.

Regras:
- Determinístico: a mesma execução sobre o mesmo arquivo dá sempre o mesmo
  resultado.
- Nenhuma chamada a modelo de IA durante a execução — só na escrita do código.
- Trate erro de leitura ou gravação parando e registrando no log; nunca falhe
  em silêncio.
- Agende via cron do sistema ou GitHub Actions, para rodar todo dia útil às 6h.
  Inclua o arquivo de agendamento (crontab ou workflow .yml) versionado.

Mostre o código completo antes de criar os arquivos e espere minha aprovação.
```

---

## O que conferir antes de aprovar

- [ ] O script **não grava nada** se alguma linha for reprovada na validação
- [ ] `owner_id` vem de uma **variável de configuração**, não de `auth.uid()` — não há sessão de usuário num cron
- [ ] A chave de upsert é `(cliente_id, ativo, data)`
- [ ] Existe **log de cada execução** — não é "rodou e sumiu"
- [ ] O agendamento (cron ou GitHub Actions) está **versionado no repositório**, igual ao `.json` do n8n

---

## Código vs n8n — quando escolher qual

| Código | n8n |
|---|---|
| Lógica complexa, precisa de testes automatizados | Fluxo simples, poucos nós |
| Você já programa e quer tudo revisável no git, em diff | Prefere ver e editar o fluxo visualmente |
| Já existe CI/CD no projeto para fazer o deploy | Não quer gerenciar infraestrutura de execução |

> Nenhuma opção é mais "certa" — é a mesma pergunta do **espectro de controle**
> do E04: quanto você quer operar diretamente versus delegar a uma ferramenta
> pronta. O pipeline (ler → mapear → normalizar → validar → upsert) é o mesmo
> dos dois jeitos.
