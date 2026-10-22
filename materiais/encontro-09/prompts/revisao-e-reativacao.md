# Prompts — Revisão humana e reativação de rede

> **Encontro 09 · mão na massa.**
> Fechar o fluxo do E08 com revisão só no caminho crítico, dar origem a cada
> registro, e montar o fluxo de reativação. Regra que governa tudo:
> **IA transforma e sugere; o banco calcula; você decide.**

---

## 1. Ponto de revisão humana (só no crítico)

```text
No fluxo de transcrição do E08, adicione um nó de revisão humana APENAS no
caminho crítico. Critério: é crítico se, saindo errado, alguém de fora percebe
ou eu assumo um compromisso que não fiz.

- Crítico (PARA e espera minha aprovação): qualquer coisa que vai ao cliente;
  tarefa com prazo que vira compromisso; o que embasa recomendação/adequação;
  edição ou exclusão de registro histórico.
- Não crítico (salva direto, rotulado "gerado por IA"): resumo interno em 3
  bullets, classificação por assunto, sugestão que só eu vejo.

Nunca use aprovação automática por tempo. Se eu não olhar, não sai.
```

---

## 2. Registro de origem em todo registro criado por fluxo

```text
Adicione uma coluna `origem` nas tabelas que recebem dados de fluxo e preencha
em TODO registro criado automaticamente, crítico ou não, no formato
`e09-transcricao-AAAA-MM-DD`. Essa marcação é rastro interno (auditoria,
depuração, art. 23 CVM 19) — NÃO deve aparecer em nada que vai ao cliente.
```

---

## 3. Fluxo de reativação de rede

```text
Monte um fluxo de reativação de relacionamento adormecido:

GATILHO: domingo, 20h.
1. Buscar clientes sem nenhuma `interacao` há mais de 60 dias.
2. Para cada um, montar contexto: última interação registrada + posição atual +
   padrão de marca (padroes/). Essa etapa é o que separa reativação de spam —
   sem ela vira "oi, tudo bem?" em escala.
3. Gerar uma sugestão de mensagem personalizada.
4. PARA: eu leio antes de enviar (mensagem a cliente é sempre crítica).
5. Após aprovação, enviar e registrar como `interacao` com `origem` (o cliente
   sai da lista dos 60 dias).

Varie o horário de envio dentro da janela e não use o mesmo gancho para todos.
Se eu não conseguir explicar por que esta mensagem vai para esta pessoa hoje, o
fluxo não deve mandar.
```

> **WhatsApp — limite honesto:** exportar conversa e tratar como arquivo (ok) ou
> API oficial do WhatsApp Business (ok, com custo). Ler a conta pessoal
> automaticamente ou automação por navegador: **não** (viola termos / risco de
> banimento). A saída padrão é gerar a mensagem para você colar.

---

## 4. Gravar a regra-mãe no CLAUDE.md

```text
Adicione ao CLAUDE.md: "IA transforma e sugere; o banco calcula; você decide."
Inclua um exemplo de cada: transforma (resumo de interações), sugere (próximo
passo), calcula no banco (PL do dashboard), decide (adequação de produto).
Toda chamada de IA fica no servidor, nunca no navegador.
```

---

## Verificação

- [ ] Nó de revisão ativo só no caminho crítico; uma execução de item crítico aprovada
- [ ] Coluna `origem` preenchida em todos os registros de fluxo
- [ ] Fluxo de reativação salvo, com revisão; ao menos uma sugestão gerada e lida
- [ ] Regra "IA transforma e sugere; o banco calcula; você decide" no `CLAUDE.md`
