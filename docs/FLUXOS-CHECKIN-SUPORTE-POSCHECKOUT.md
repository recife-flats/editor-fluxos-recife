# Fluxos 04, 05, 06 — Check-in/FNRH, Suporte e Pós-Checkout

> Desenhados a partir do Manual de Operação (setores Hóspedes Ativos,
> Manutenção & Suporte, Pós-Check-out & Feedback). Ainda NÃO existem no n8n —
> são o rascunho para revisão antes de virar workflow.

## 04 — Check-in / Check-out / FNRH (Hóspedes Ativos)

Três gatilhos de entrada:
- 48h antes (apês 804/1006): pede documentos + placa para liberar portaria, grava doc_ok, avisa Lucas.
- 24h antes: confirma horário de chegada (check-in a partir das 15h).
- Hóspede diz "cheguei": dispara a FNRH (obrigatória por lei), aguarda até 2h, se preenchida libera senhas de acesso, senão manda lembrete.

Grava em bookings: fnrh_ok, doc_ok, checkin_time, booking_status.

## 05 — Suporte & Manutenção (Manutenção)

Detecta palavras-chave de problema em QUALQUER setor: vazamento, sem luz,
sem água, trancado, não abre, emergência (prioridade CRÍTICA).
- Dentro do horário: notifica Lucas na hora.
- Fora do horário: pergunta se é emergência; se sim, aciona plantão (Vanessa +55 81 9660-1178); se não, responde no dia seguinte.
- Sempre abre ticket de manutenção e registra a ocorrência.

## 06 — Pós-Checkout & Feedback (Pós-Checkout)

Detecta saída ("saí"/"fui embora"/"checkout") ou roda no checkout:
- Solicita vistoria ao Lucas.
- Sem dano: devolução integral; com dano: calcula desconto e envia foto.
- PIX de devolução em até 24h, grava returned_at, status='returned'.
- Pede avaliação na plataforma.
- Agenda reengajamento (CRM), conversa vai para Resolvida.

## Ponto de atenção
Estes três ainda são rascunho. Antes de virar workflow no n8n, revisar os
textos das mensagens (tom, emojis) e confirmar os gatilhos de tempo.
