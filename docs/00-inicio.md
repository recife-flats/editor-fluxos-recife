# Fluxo 00 — Início (verificação + roteamento por setor)

> Diferença central para o Umbler: antes de qualquer menu, o n8n consulta o
> Supabase pelo telefone e roteia para o **setor certo** conforme onde o
> hóspede está na linha do tempo.

## A lógica

```
Mensagem chega (webhook conversation_created)
  │
  └─▶ VERIFICA TELEFONE no Supabase
        │
        ├── ACHOU → roteia por etapa da reserva:
        │      • caução pago + em estadia → 🟢 Hóspedes Ativos → Menu Hóspede
        │      • reservou, caução pendente → 🔵 Vendas → Menu Caução Pendente
        │      • já fez checkout          → 🟣 Pós-Checkout → Menu Pós
        │
        └── NÃO ACHOU → "Você já tem reserva conosco?"
               ├── NÃO → é LEAD → 🔵 Vendas → Menu Comercial (etiqueta lead-novo)
               └── SIM → pede número da reserva → busca no banco
                     ├── ACHOU → vincula telefone novo → roteia por etapa
                     └── NÃO ACHOU → reserva não cadastrada → atendente humano
```

## Por que roteia por setor (e não menu fixo)

Os 4 setores são a linha do tempo do hóspede. O mesmo "oi" recebe resposta
diferente conforme a etapa: um lead ouve preços; um hóspede em estadia ouve
"precisa de WiFi?"; quem fez checkout ouve sobre devolução de caução. É isso
que o Umbler não fazia — lá todo mundo entrava no mesmo menu.

## Pendências (só você resolve)

- Textos das saudações por horário — onde encaixam no fluxo novo?
- Nome exato das colunas: número da reserva, e phone_normalized em users.
- Valores de payment_status ('pago'? 'paid'?).
- Aceite de política de privacidade — integrar ao primeiro contato do lead.
