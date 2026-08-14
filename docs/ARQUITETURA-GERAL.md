# Arquitetura geral dos fluxos — Recife Flats (Chatwoot + n8n + Supabase)

> Consolidado a partir de: captura do Umbler, Manual de Operação, doc de
> Implementação, e repositório do case técnico (GitHub).

## A espinha: 4 setores = linha do tempo do hóspede

A conversa nunca cai num menu genérico. O n8n verifica o telefone no
Supabase e roteia para o SETOR certo, conforme onde o hóspede está:

```
                    ┌─ VERIFICA TELEFONE no Supabase ─┐
                    │                                  │
   🔵 VENDAS & RESERVAS      🟢 HÓSPEDES ATIVOS    🟠 MANUTENÇÃO    🟣 PÓS-CHECKOUT
   ────────────────────      ──────────────────    ────────────    ──────────────
   • lead novo               • caução pago         • vazamento     • já fez checkout
   • pede info               • em estadia          • sem luz/água  • aguarda devolução
   • reservou (sem caução)   • dúvidas WiFi        • trancado fora   do caução
   • quer estender           • instruções chegada  • eletro quebrado • pedido avaliação
                                                                    • hóspede recorrente
   SAI quando:               SAI quando:           SAI quando:     
   • pagou caução → Ativo    • checkout → Pós      • resolveu →    
   • desistiu → fecha        • problema → Manut.     volta Ativo   
```

## Mapa de fluxos (cada um = uma aba no editor / um workflow no n8n)

| Fluxo | Setor | Nome | Dispara por |
|-------|-------|------|-------------|
| 00 | (entrada) | Início + verificação de banco | mensagem nova |
| 01 | 🔵 | Reservas & Preços | menu / lead |
| 02 | 🔵→🟢 | Caução (PIX, comprovante, devolução) | palavra-chave: caução, pix, pagar |
| 03 | 🟢 | Apartamentos & Dúvidas (WiFi, regras, pet) | menu / palavra-chave |
| 04 | 🟢 | Check-in / Check-out / FNRH | menu / data |
| 05 | 🟠 | Suporte & Manutenção | palavra-chave: urgente, vazamento |
| 06 | 🟣 | Pós-checkout & Avaliação | rotina diária |

## Colunas reais do Supabase (confirmadas no Manual)

- **bookings:** fnrh_ok, doc_ok, form_ok, comprovante_ok, guest_name,
  checkin_time, payment_status, userId (→ users.id)
- **security_deposits:** amount, received_at, receipt_method,
  receipt_proof_url, returned_at, return_proof_url, withheld_amount,
  withhold_reason, damage_photos_urls, status, return_deadline
- **users:** cpf, birth_date, city, profession, how_found_us,
  document_link, accepts_whatsapp, phone

## Valores de caução por apartamento

| Apto | Caução |
|------|--------|
| 105  | R$150  |
| 203  | R$150  |
| 804  | sem caução (só liberação por docs) |
| 1006 | R$200  |

## Webhooks do Chatwoot que alimentam o n8n

| Evento | Quando | Ação no n8n |
|--------|--------|-------------|
| conversation_created | nova conversa | buscar hóspede no Supabase pelo telefone |
| message_created | cada msg recebida | detectar palavra-chave (filtrar message_type=incoming) |
| conversation_status_changed | resolvida/aberta | atualizar booking_status |
| contact_updated | dados mudaram | sincronizar com users |

## Etiquetas (labels) que os fluxos aplicam

lead-novo · booking · direto · caucao-pendente · caucao-pago ·
liberacao-portaria · fnrh-ok · check-in-hoje · em-estadia · emergencia ·
manutencao · check-out-hoje · caucao-devolvido · avaliacao-pedida · finalizado
