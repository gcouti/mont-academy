# Prompts — Fluxo de transcrição (n8n e código)

> **Encontro 08 · mão na massa.**
> O fluxo que lê uma transcrição, identifica o cliente, cria a interação, extrai
> tarefas e **para para revisão**. Pode usar a transcrição de exemplo de
> `encontro-03/artefatos/transcricao-exemplo-call-advisor-cliente.txt`.
> Regra de ouro: **o fluxo automatiza a montagem, não a decisão** — ele para
> quando há ambiguidade e antes de qualquer coisa sair.

---

## 1. Construir o fluxo no n8n

```text
Monte no n8n um fluxo que processa transcrições de reunião. Etapas:

GATILHO: arquivo .txt novo na pasta transcricoes/.
1. Ler a transcrição.
2. Identificar o cliente buscando o nome mencionado na tabela `cliente`.
   - Achou um: associa e segue.
   - Não achou / achou mais de um / nome escrito diferente: PARA e pede input.
     Não adivinhe — ambiguidade não escolhe por mim.
3. Criar uma linha em `interacao`: canal = "reunião", data, cliente_id, e um
   resumo em EXATAMENTE 3 bullets, seguindo o tom e as regras do CLAUDE.md (E03).
4. Extrair compromissos ("vou te mandar", "combinamos que", "próximo passo") e
   gerar itens em `tarefa` com prazo sugerido.

Salve TUDO como rascunho — nada é publicado, nada notifica. O fluxo PARA aqui
(ponto de revisão humana do E09). Garanta idempotência: rodar o mesmo arquivo
duas vezes NÃO pode criar duas interações.
```

---

## 2. O mesmo fluxo em código (para comparar)

```text
Escreva uma função que faça o que o fluxo do n8n faz: ler um arquivo de
transcricoes/, buscar o cliente na tabela `cliente`, criar uma `interacao` com
resumo em 3 bullets, extrair tarefas, e salvar tudo como rascunho. Siga as regras
do CLAUDE.md. Não envie nada — só grave o rascunho. Mostre antes de executar.

Depois me ajude a decidir, em 2 linhas no CLAUDE.md, se este fluxo deve viver no
n8n ou em código — com base nos dois critérios: conecta serviços de terceiros
(n8n ganha) × mexe no meu banco e na minha regra (código ganha).
```

---

## 3. Trazer o fluxo para a rede de segurança do git

```text
Exporte o fluxo do n8n como .json e salve em consultor-mont/fluxos/. Quero
commitar esse arquivo toda vez que eu mexer no canvas, para o fluxo entrar no
histórico do git (E04). Me lembre desse passo sempre que eu alterar o fluxo.
```

---

## Verificação

- [ ] Fluxo salvo e ativo; uma execução completa até o ponto de parada
- [ ] A interação aparece como **rascunho**, com `cliente_id` correto
- [ ] Ao menos uma tarefa extraída da transcrição
- [ ] Rodar o mesmo arquivo 2× **não** cria duas interações
- [ ] `.json` do fluxo exportado e **commitado** no repositório
