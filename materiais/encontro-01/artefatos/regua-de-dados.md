# Régua de dados — uma página (Encontro 01)

> Para lembrar às 11 da noite, com pressa, antes de colar alguma coisa.
> *Ela é boa em transformar informação que você deu, e ruim em fornecer informação que você não tem.*

## Antes de colar, responda 3 perguntas sobre a ferramenta

1. **Onde o dado fica?** — país, servidor, contrato. Plano gratuito ≠ corporativo.
2. **Quem pode acessar?** — só você? a equipe do provedor? treina o modelo?
3. **Por quanto tempo?** — há retenção? dá para apagar de verdade?

Sem resposta às três, **não está aprovada** para dado de cliente.

## Árvore de classificação do dado

1. Tem **nome, CPF, e-mail, telefone, conta ou contrato**? → **PESSOAL (identificado)** · ❌ não entra
2. A **combinação** dos campos restantes chega a uma pessoa só? → **PESSOAL (identificável)** · ❌ não entra
3. Existe **chave** que desfaz o que você fez (planilha do "Cliente 47")? → **PSEUDONIMIZADO** (ainda é pessoal) · ❌ não entra
4. Nenhum dos acima, de verdade → **ANONIMIZADO** · ✅ pode usar

> "Não sei" cai no mesmo lugar que "sim". **Incerteza é resposta negativa.**

## Pode entrar ✅

Base fictícia da Mont · dado público de mercado · texto próprio sem identificação · norma e regulamento · pergunta conceitual sobre produto.

## Não pode entrar ❌

Nome, CPF, e-mail, telefone · posição, extrato, movimentação · contrato, suitability, proposta · print de conversa · transcrição sem tratamento.

## Anonimizar em 3 níveis

- **Original:** "João Pereira, CPF …, R$ 847.320,15 no fundo XP Multiestratégia, resgatar R$ 200 mil até 15/03."
- **Pseudonimizado (ainda é pessoal):** "Cliente 47 tem R$ 847.320,15 no fundo XP… " — se existe a tabela do 47, nada foi resolvido.
- **Anonimizado:** "Cliente PF tem entre R$ 800–900 mil num fundo multimercado e quer resgatar ~25% no próximo mês."

> Cuidado com o **identificável por combinação**: "dentista · Maringá · R$ 2,3 mi · resgatou em março" identifica sem nome nenhum.

## Lembretes legais (orientação técnica, não parecer jurídico)

- **Você é o controlador** — responde perante a ANPD e o cliente.
- **Art. 17 CVM 19:** automação **não mitiga** a sua responsabilidade. "Foi a IA" não é defesa.
- **Patrimônio/posição não são "sensíveis"** na LGPD (lista fechada), mas são dado pessoal sob sigilo.

## Se vazar: Pare → Registre → Avise → Contenha

**Pare** (não cole mais nada, feche) · **Registre** (o quê, quando, qual ferramenta, quantos titulares) · **Avise** (canal da Mont) · **Contenha** (apague o que der, revogue acesso).

> Prazo legal: **3 dias úteis** (LGPD art. 48 + Res. CD/ANPD 15/2024). Por isso "registre" vem antes de "contenha".
