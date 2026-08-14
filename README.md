# Editor de Fluxos — Recife Flats WhatsApp

Editor visual dos fluxos de atendimento do WhatsApp (Chatwoot + n8n + Supabase).
Permite desenhar, revisar e editar os fluxos como caixinhas ligadas, antes de
implementar no n8n.

## Como usar

Abra o arquivo `index.html` no navegador (duplo clique). Precisa de internet na
primeira vez, porque carrega a biblioteca Drawflow e as fontes.

- **Menu lateral:** as abas de cada fluxo (Início, Reservas, Caução, etc.)
- **Paleta no topo:** arraste blocos para o canvas
- **Clique num bloco:** abre o painel de edição (texto, opções)
- **Barra de ações:** Editar · Copiar · Apagar · Desfazer
- **Baixar JSON:** salva o fluxo aberto em arquivo

## Estrutura

- `index.html` — o editor
- `fluxos-json/` — os fluxos exportados em JSON
- `docs/` — documentação da arquitetura e de cada fluxo

## Atalhos

- `Ctrl+Z` desfazer · `Delete` apagar · `Ctrl+D` duplicar · `Esc` fechar painel

---
Projeto de Vanessa · Recife Flats Temporada
