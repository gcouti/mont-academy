# CLAUDE.md

> **Artefato do Encontro 03.** Ponto de partida do `consultor-wiki` — o aluno
> recebe este arquivo pronto e edita ao menos três linhas para a própria
> realidade (com atenção à seção "O que nunca entra"). É o mesmo conteúdo gerado
> pelo [prompt de setup](../prompts/setup-estrutura-llm-wiki.md).

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
