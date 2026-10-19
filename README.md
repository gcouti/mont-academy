# Mont Academy — Workshop Claude

Repositório de materiais do workshop de apresentação do **Claude**.

Este workshop é dividido em **11 encontros**. A branch `main` é mantida limpa — ela
contém apenas esta visão geral. O material de cada encontro é preparado em sua
própria branch e, conforme os encontros acontecem, vai sendo incorporado à `main`.

## Como navegar

Cada encontro tem uma branch dedicada. Para ver o material de um encontro:

```bash
git checkout encontro-01   # troque o número pelo encontro desejado
```

A estrutura de material dentro de cada branch é:

```
materiais/
└── encontro-NN/
    ├── artefatos/   # arquivos, exemplos e entregáveis gerados no encontro
    ├── prompts/     # prompts usados e demonstrados
    └── skills/      # skills criadas ou apresentadas
```

## Encontros

| # | Branch | Tema |
|---|--------|------|
| 01 | `encontro-01` | Fundamentos, Modelo Mental e Segurança |
| 02 | `encontro-02` | Diferenciar Ferramentas e Setup |
| 03 | `encontro-03` | LLM, Wiki e Agente de Código |
| 04 | `encontro-04` | Git e Versionamento |
| 05 | `encontro-05` | Modelagem de Dados e CRM |
| 06 | `encontro-06` | Ingestão e Tratamento de Dados |
| 07 | `encontro-07` | Dashboard, APIs e Alertas |
| 08 | `encontro-08` | Automação e Fluxo de Transcrição |
| 09 | `encontro-09` | Revisão Humana e Reativação de Rede |
| 10 | `encontro-10` | MCP Próprio e Operação |
| 11 | `encontro-11` | Publicação, DNS e Apresentação Final |

## Fluxo de trabalho

- `main` → sempre limpa, com a visão geral e o que já foi apresentado.
- `encontro-NN` → preparação do material de cada dia.
- Ao final de cada encontro, o conteúdo é mesclado na `main`.
