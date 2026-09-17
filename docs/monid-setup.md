# Monid CLI — status e próximos passos

## O que já está pronto neste repositório
- `@monid-ai/cli` instalado como devDependency local do projeto (não global) — use sempre via `npx monid ...`. Ver `package.json`.
- O skill `scroll-world` (que usa Monid como backend de vídeo por padrão) está instalado em `.agents/skills/scroll-world` e symlinkado em `.claude/skills/scroll-world`.

## O que NÃO fica salvo entre sessões (por design)
A API key da Monid nunca é commitada no repositório (é um segredo). Ela vive apenas no
credential store local do container (`~/.config/monid`), que é apagado quando a sessão/VM
é reciclada. **Toda sessão nova precisa reconfigurar a chave.**

## Checklist para uma sessão nova (ambiente "Default", com Acesso à rede = Completo)

1. Confirmar que o ambiente desta sessão é o **Default** com **Acesso à rede = Completo**
   (isso já foi ajustado — sessões antigas não herdam a mudança, só as novas).
2. Instalar dependências do projeto (se necessário): `npm install`.
3. Adicionar a API key novamente:
   ```
   npx monid keys add --key <sua-chave-monid_live_...> --label site-escritorio
   ```
4. Verificar conectividade e saldo:
   ```
   npx monid whoami
   npx monid balance
   ```
   Se ambos responderem sem erro de rede/allowlist, a integração está funcionando.
5. (Opcional) Servidor MCP, se for usar via MCP em vez da CLI direta:
   ```
   claude mcp add --transport http monid https://mcp.monid.ai/v1 \
     --header "Authorization: Bearer <sua-chave>" --scope local
   claude mcp list
   ```
   Isso também não persiste entre sessões (fica em `~/.claude.json`, fora do repo).

## Contexto do bloqueio original
As tentativas de alcançar `monid.ai`, `api.monid.ai`, `mcp.monid.ai` e `docs.monid.ai`
falhavam com `403` do proxy de rede porque o ambiente "Default" estava com Acesso à rede
em "Trusted" (allowlist padrão, sem esses domínios). Foi alterado manualmente para
"Completo" (Full) nas configurações do ambiente, no app do Claude Code — essa mudança só
vale para sessões abertas depois da alteração.
