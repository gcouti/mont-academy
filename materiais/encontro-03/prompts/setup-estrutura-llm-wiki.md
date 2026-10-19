# Prompt de setup — estrutura do LLM Wiki (`consultor-wiki`)

> **Encontro 03 · Bloco 4 (Construção guiada).**
> Este é o prompt que o aluno cola no Claude Code, dentro da pasta do curso, para
> o agente montar a estrutura do repositório `consultor-wiki`. Depois de rodar,
> cada aluno **edita ao menos três linhas** do `CLAUDE.md` para a realidade dele
> (principalmente a seção "O que nunca entra").

---

## Como usar

1. Abra o Claude Code na pasta do curso (a mesma do E02).
2. Cole o prompt abaixo (tudo dentro do bloco).
3. Revise o que o agente propôs antes de aprovar a criação dos arquivos.
4. Edite o `CLAUDE.md` para a sua realidade.

---

## O prompt

```text
Você vai montar a estrutura inicial de uma base de conhecimento pessoal chamada
"consultor-wiki", seguindo o padrão LLM Wiki. Crie tudo dentro de uma pasta
`consultor-wiki/` na raiz deste diretório.

Contexto: sou consultor de valores mobiliários. A informação entra bruta, é
organizada numa wiki, e depois é consultada para produzir peças de comunicação
com cliente. O mesmo arquivo que eu leio para trabalhar é o que você lê para
executar.

Crie a seguinte estrutura de pastas, cada uma com um arquivo README.md curto
explicando seu propósito e quem a edita:

- `raw/`      — fontes originais como chegaram (transcrição de reunião, lâmina de
                fundo, regulamento, ata, planilha). NUNCA é modificada pelo
                agente, só lida. Quem edita: eu.
- `wiki/`     — conhecimento organizado, uma página markdown por assunto,
                interligadas com [[wikilinks]]. Quem escreve: você (o agente),
                sob minha revisão.
- `padroes/`  — modelos das peças recorrentes (carta-mensal.md, follow-up.md,
                resumo-reuniao.md). Definem estrutura e tom. Quem edita: eu.
- `archive/`  — o que não é mais usado no dia a dia, mantido para referência.

Mantenha cada pasta versionável mesmo vazia (use um .gitkeep onde não houver
conteúdo ainda).

Depois, crie na raiz de `consultor-wiki/` um arquivo `CLAUDE.md` com este
conteúdo de partida — é a instrução persistente que você vai seguir em toda
conversa dentro desta pasta:

---
# CLAUDE.md

Este arquivo orienta o agente de código que trabalha nesta pasta.

## O que é este repositório

Base de conhecimento de um consultor de valores mobiliários. A informação
entra bruta, é organizada, e é consultada depois para produzir peças de
comunicação com cliente.

## Estrutura de pastas

- `raw/` — material capturado como chegou: transcrição de reunião, lâmina de
  fundo, regulamento, relatório, ata. **Nunca modifique nada aqui.**
- `wiki/` — conhecimento organizado, uma página por assunto, interligado com
  `[[links]]`. É o que você escreve.
- `padroes/` — modelos das peças recorrentes (`carta-mensal.md`,
  `follow-up.md`, `resumo-reuniao.md`). Definem estrutura e tom.
- `archive/` — o que não é mais usado no dia a dia, mantido para referência.

## Como escrever em `wiki/`

- Uma página por assunto. Título em `#`, nome do arquivo igual ao título.
- **Toda afirmação cita a fonte em `raw/` de onde veio.** Sem exceção.
- Se a informação não está em `raw/`, escreva "não consta" — não complete
  com conhecimento geral.
- Português do Brasil, linguagem de cliente leigo, sem jargão não explicado.
- Ligue páginas relacionadas com `[[wikilink]]`.

## O que nunca entra

- Nome, CPF, e-mail, telefone ou conta de cliente.
- Valor exato de patrimônio — use faixa.
- Recomendação de produto a cliente específico. Isso é decisão minha.

## Operações

- **ingest** — leio um arquivo de `raw/`, crio ou atualizo as páginas de
  `wiki/` que ele afeta, e listo o que mudou.
- **query** — respondo com base no `wiki/`, citando as páginas usadas.
- **lint** — reviso `wiki/` procurando contradições, páginas órfãs e
  afirmações sem citação.

## Regras de trabalho

- Proponha antes de alterar arquivo existente. Criar arquivo novo pode.
- Ao terminar uma ingestão, diga em uma linha o que ficou de fora e por quê.
---

Antes de criar qualquer coisa, me mostre a árvore de pastas e arquivos que você
vai gerar e aguarde minha aprovação. Ao final, liste em uma linha o que criou.
```

---

## Depois de rodar (checklist do aluno)

- [ ] As quatro pastas existem: `raw/`, `wiki/`, `padroes/`, `archive/`.
- [ ] O `CLAUDE.md` está na raiz do `consultor-wiki/`.
- [ ] Editei **pelo menos três linhas** do `CLAUDE.md` para a minha realidade —
      com atenção à seção **"O que nunca entra"**, que é a que mais varia por aluno.
- [ ] A estrutura está pronta para receber a primeira transcrição em `raw/`
      (bloco 5 — ingestão ao vivo).

> **Regra de condução:** edite o `CLAUDE.md` em aula, não só leia. Um `CLAUDE.md`
> copiado sem edição não foi testado contra nada e vira ficção.
