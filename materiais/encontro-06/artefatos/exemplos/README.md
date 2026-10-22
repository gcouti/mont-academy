# Exemplos práticos — Encontro 06

Arquivos fictícios e **já anonimizados** (régua do E01: sem nome, CPF ou e-mail;
contas como alias `CLI-001`) para usar na aula de ingestão de CSV.

## Os três CSVs — de propósito diferentes entre si

Cada um exercita uma "dor" da normalização. São a mesma carteira em três formatos:

| Arquivo | Separador | Decimal / milhar | Data | Nome do ativo | Classe |
|---|---|---|---|---|---|
| `corretora-alfa-2026-11-01.csv` | `;` | vírgula / ponto, com `R$` | `01/11/2026` (coluna) | ticker, com espaço à direita (`ITSA4 `) | MAIÚSCULA (`AÇÃO`) |
| `corretora-beta-2026-11-01.csv` | `,` | ponto, sem símbolo | `2026-11-01` (coluna, ISO) | nome por extenso (`ITAÚSA PN`) | `Ações` |
| `corretora-gama-2026-11-01.csv` | `,` | formato US `1,234.56` entre aspas | `1-Nov-2026` (só no **cabeçalho**) | variante (`ITAUSA4`) | minúscula (`acao`) |

O que a turma deve notar: **é a mesma posição**, mas nenhuma coluna, número ou
sigla bate entre os arquivos. Por isso a normalização existe.

## `ativo-sinonimos.csv`

A tabela de mapeamento (origem → canônico). É o `raw/` do E03 ganhando uso:
cada caso novo que a normalização não pega vira uma linha nova aqui — o laço de
melhoria. Repare que `BOVA11` **não** está completo de propósito: é o caso que
cai no relatório abaixo, para o aluno exercitar "achar o caso e adicionar a regra".

## `relatorio-inconsistencia-exemplo.txt`

Como fica o relatório que o importador mostra **antes de gravar**. 3 linhas lidas,
2 passam, 1 cai (o `BOVA11`). Barreira, não log.

## Como usar em aula

1. Salve um dos CSVs em `raw/` do seu `consultor-mont`.
2. Rode os prompts de `../../prompts/importador-csv-e-n8n.md` (ler → normalizar →
   validar → upsert).
3. Confira que rodar o mesmo arquivo duas vezes **não duplica** em `posicao`.
