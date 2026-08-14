# Inventário do n8n — o que já existe (13/08/2026)

> Descoberto ao acessar https://n8n.recifeflatstemporada.com/home/workflows
> Importante: os workflows vivem no servidor (n8n), não no computador.
> Formatar o PC NÃO apaga — só apaga cópias locais.

## Workflows salvos (4)

| Workflow | Status | Criado | Observação |
|----------|--------|--------|------------|
| InfinitePay - Confirmação Caução | 🟢 Published | 1 ago | Integração de pagamento do caução — JÁ EXISTE |
| Sprint 1B - MVP Saudação | 🟢 Published | 4 jul | Bot de saudação (já na memória do projeto) |
| My workflow | rascunho | 6 jul | Provável teste |
| Sprint 1A - WiFi Auto-resposta | — | 28 jun | Resposta automática de WiFi por palavra-chave |

## Credenciais configuradas (4)

| Credencial | Tipo | Observação |
|-----------|------|------------|
| Postgres account | Postgres | Acesso direto ao banco |
| Groq account | Groq | LLM (respostas com IA?) |
| Chatwoot API | Header Auth | Conexão com o Chatwoot |
| Supabase account | Supabase API | Acesso ao Supabase |

## Métricas atuais do n8n
- Execuções em produção: 3 · Falhas: 0 · Taxa de falha: 0% · Tempo médio: 4.32s
- n8n versão 2.27.4

## O que isso significa para o projeto
- O caução com InfinitePay JÁ está montado e publicado — não perdemos nada.
- O bot de saudação e o de WiFi também já existem no n8n.
- Próximo passo: exportar os JSON desses workflows e refletir no editor visual,
  para o desenho bater com o que já está rodando de verdade.

## Endereços confirmados
- n8n:      https://n8n.recifeflatstemporada.com
- Chatwoot: https://chat.recifeflatstemporada.com
- VPS:      srv1710017.hstgr.cloud (Hostinger)

## Detalhes técnicos reais (extraídos dos JSON exportados)

### Caução (2 workflows conectados por webhook)
- **Criação do link** (dentro do Sprint 1B): POST api.checkout.infinitepay.io/links
  - handle: "recifeflats"
  - price: valor × 100 (centavos)
  - order_nsu: securityDepositId
  - webhook_url: https://n8n.recifeflatstemporada.com/webhook/infinitepay-caucao
  - grava em security_deposits com status 'pending' (upsert ON CONFLICT booking_id)
- **Confirmação** (workflow InfinitePay): recebe webhook → RE-VALIDA em
  api.checkout.infinitepay.io/payment_check → se success+paid, UPDATE
  security_deposits status='received', paid_at, infinite_payment_id,
  receipt_proof_url; e bookings.comprovante_ok=true → confirma no Chatwoot.

### Colunas reais de security_deposits (confirmadas)
status, paid_at, infinite_payment_id, infinite_reference, receipt_proof_url,
checkout_url, booking_id, property_id, amount, booking_code, guest_name,
guest_phone, metadata (jsonb com conversation_id e account_id)

### WiFi (Sprint 1A)
- Palavras-chave (regex): wifi | wi-fi | senha | internet | rede
- Busca em v_guest_active_stay pelo phone_normalized
- Ipê (105/203): rede Hospedagem105_vivo / senha BemVindo105
- Forte (804): rede Rosemary / senha megue6049
- ⚠️ VERIFICAR: 105 e 203 estão com a mesma senha no workflow

### Normalização de telefone (real, no nó "Extrai dados")
Se tem 12 dígitos e começa com 55, insere o 9 após o DDD (posição 4).

### Padrão de resposta ao Chatwoot
POST chat.recifeflatstemporada.com/api/v1/accounts/{account_id}/conversations/{conversation_id}/messages
com message_type: outgoing, private: false, auth via Header (Chatwoot API).
