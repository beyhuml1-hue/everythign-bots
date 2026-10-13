# Everything Bots

Owner workspace: https://bot.lb000.xyz  
Member verification: https://verify.lb000.xyz

The two hostnames share the existing `verify-portal` Worker and `VerificationRegistry` Durable Object named `community`. The member records, encrypted consent tokens, server selection and transfer history stay in their existing namespace. The deployed verification module was preserved before this change. Bot configuration uses additional `bot:` storage keys. There is no member deletion or database replacement migration.

Sign in with the existing owner Discord account. Use **Bot settings** to edit the name, picture and description. **Activate commands** connects Discord interactions and registers slash commands. Existing unrelated application commands are preserved. Connect additional applications you own from Overview; their bot tokens are encrypted at rest and never returned to the browser.

Moderation includes warnings, timeout, kick, ban/unban, role changes, message cleanup, channel lock/unlock, slowmode and announcements. Live Discord permissions and role hierarchy are checked on every action. Admin checker reports actual server permissions. Configure a support role to enable private `/ticket` channels; `/close` archives their access without deleting messages.

Vouchers have a server, code, reward, claim limit and expiry. `/redeem` records one claim per member. Optional Discord role rewards are delivered by the bot. Other rewards are recorded for manual fulfilment; no separate financial wallet is created. Ambiguous role delivery is marked `needs_review` rather than issued again.

## Development and deployment

Node 22+ and Wrangler 4 are required. Run `npm install`, `npm test`, `npm run check`, then `npm run deploy` using an authenticated Cloudflare account. The checked-in configuration targets the existing `verify-portal` service and preserves its bindings and secrets. Never rename or delete its Durable Object class or the `community` object. Do not publish tokens, `.dev.vars`, snapshots or deployment keys.

Existing Cloudflare secrets: `DISCORD_BOT_TOKEN`, `DISCORD_CLIENT_ID`, `DISCORD_CLIENT_SECRET`, `OWNER_DISCORD_ID`, `SESSION_SECRET`, `TOKEN_ENCRYPTION_KEY`, `BOT_ADMIN_KEY`. OAuth continues using the registered callback `https://verify.lb000.xyz/oauth/callback`. Owner login transfers to the bot domain using a short-lived, single-use ticket bound to the browser; cookies remain host-only.

The deployment-only `/api/setup-primary` endpoint requires `BOT_DEPLOY_KEY`. Provision a random temporary key for a first deployment, call it once to connect the primary bot, and remove the key immediately. Normal configuration uses the signed owner session instead.

GitHub contains source code; the Cloudflare worker runs the dashboard and Discord HTTP interactions. Pushes alone do not deploy without a configured deployment workflow.

## Commands

`/help`, `/verify`, `/admincheck`, `/serverinfo`, `/userinfo`, `/ban`, `/unban`, `/kick`, `/timeout`, `/untimeout`, `/warn`, `/warnings`, `/clearwarnings`, `/purge`, `/lock`, `/unlock`, `/slowmode`, `/roleadd`, `/roleremove`, `/announce`, `/redeem`, `/ticket`, `/close`.

Discord may show the bot offline because this integration uses HTTP interactions rather than a gateway connection. Its slash commands still run while the worker is available. Changing online presence requires a separately hosted gateway process.
