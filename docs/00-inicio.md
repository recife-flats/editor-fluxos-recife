# Fluxo 00 — Início (versão completa com pendências por apartamento)

## Lógica
Mensagem recebida → verifica no Supabase pelo telefone:

**Hóspede conhecido:**
- Saudação por horário (Recife): bom dia (7-12h), boa tarde (12-18h),
  boa noite (18-24h), + versões de almoço e pós-expediente.
- Tem pendências? Se sim, cobra conforme o apartamento; se não, menu hóspede.

**Lead (não encontrado):**
- Saudação por horário → "Já tem reserva conosco?"
  - Sim → digite o número → busca no banco → achou: "Encontrei sua reserva,
    {nome}!" (vai pra checagem de pendências); não achou → notifica atendente.
  - Não → conectando a colaborador → menu comercial (Reservas, Ver apês,
    Dúvidas, Já sou hóspede, Outro assunto).

## Pendências por apartamento (confirmado por Vanessa)

| Item                    | 105 | 203 | 804 | 1006 |
|-------------------------|-----|-----|-----|------|
| FNRH (obrigatório)      |  ✅  |  ✅  |  ✅  |  ✅   |
| Caução 100% reembolsável|  ✅  |  ✅  |  ❌  |  ✅   |
| Documentação hóspedes   |  ✅  |  ✅  |  ✅  |  ✅   |
| Dados do veículo        |  —  |  —  |  ✅  |  ✅*  |

- 804: sem caução, mas sempre pede dados do veículo (garagem compacta) +
  info da garagem enviada + formulário para a direção.
- 1006*: dados do veículo só se for usar o estacionamento.

## Mensagem de cobrança de pendências
"Para confirmar sua reserva, ainda precisamos que você finalize o preenchimento
da FNRH, o envio da documentação dos hóspedes que terão acesso ao apartamento
[+ caução reembolsável, se 105/203/1006] [+ dados do veículo, se 804/1006]."

## Horário (do print do Umbler + textos da Vanessa)
- Bom dia / Boa tarde / Boa noite (normais)
- Almoço: "Nossa equipe está em horário de almoço. Assim que possível um
  colaborador vai te atender. Enquanto isso, posso ajudar? Qual seu nome?"
- Pós-expediente: "Nossa equipe já encerrou o atendimento de hoje. Sua mensagem
  foi registrada e amanhã pela manhã um colaborador responde. Se for urgente,
  digite Urgente."
