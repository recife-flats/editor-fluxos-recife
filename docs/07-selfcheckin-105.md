# Fluxo 07 — Self-Checkin 105 (extraído do Umbler + otimizado)

> Reconstruído dos prints do fluxo "Self-Checkin | 105" do Umbler Talk.
> Serve de MOLDE para os outros apês (203/804/1006) — o que é específico do
> 105 está marcado para trocar depois.

## Sequência
1. Início (gatilho) → menu: Iniciar Self Check-in / Ver Localização
2. Boas-vindas + Localização (Delikata / Edifício Ipê / Av. Engenheiro)
3. Acesso ao prédio: cadeado cofre perto da caixa de correios (senha do cofre)
4. "Tudo certo?" → Continuar Check-in / Estacionamento
5. Avisos: chuveiro + ar-condicionado não juntos; descarte de lixo; boas-vindas
6. Menu: Wi-Fi / Estacionamento / Localização / Falar com atendente / Finalizar
7. Finaliza → grava check-in no banco

## Dados específicos do 105 (confirmados por Vanessa)
- WiFi: rede Hospedagem105_vivo / senha BemVindo105
- Acesso: cadeado cofre perto da caixa de correios
- Estacionamento: rotativo, ao lado da Delikata
- Atendente: Gabriela

## Otimizações aplicadas (vs. Umbler original)
- Gatilho consulta o banco pelo telefone para já saber quem é (pular perguntas).
- Menu único de opções ao invés de vários "enviar mensagem" soltos.
- Ao finalizar, grava checkin_time e booking_status no Supabase + etiqueta.
- Volta ao menu centralizada (um caminho de retorno, não repetido).

## Para virar molde dos outros apês
Trocar: senha WiFi, senha do cofre, forma de acesso (804/1006 têm portaria),
localização e nome do edifício. A estrutura permanece igual.
