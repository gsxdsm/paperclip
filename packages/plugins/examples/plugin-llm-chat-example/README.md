# LLM Chat Example Plugin

Example Paperclip plugin that demonstrates:

- `dashboardWidget` UI contribution for a self-contained chat widget
- `ctx.llm.providers.list()` to discover direct-session-capable providers
- `ctx.llm.sessions.create()` / `send()` / `close()` for multi-turn chat
- `ctx.streams` / `usePluginStream()` for token streaming into the browser UI

## Slot

| Slot        | Type               | Description                                              |
|-------------|--------------------|----------------------------------------------------------|
| LLM Chat    | `dashboardWidget`  | Dashboard widget with provider/model selection and chat. |

## Capabilities

- `ui.dashboardWidget.register` — render the widget on the dashboard
- `llm.providers.list` — list direct-chat-capable providers
- `llm.sessions.create` — start a direct LLM session
- `llm.sessions.send` — send chat messages and stream chunks
- `llm.sessions.close` — explicitly close a session on reset

## Worker

- `getData "llm.providers"` — returns providers plus their available models
- `performAction "llm.chat.send"` — creates or resumes a session, sends the message, and streams chunks on `llm-chat`
- `performAction "llm.chat.close"` — closes the active direct LLM session

## Local Install (Dev)

From the repo root, build the plugin and install it by local path:

```bash
pnpm --filter @paperclipai/plugin-llm-chat-example build
pnpm paperclipai plugin install ./packages/plugins/examples/plugin-llm-chat-example
```

To uninstall:

```bash
pnpm paperclipai plugin uninstall paperclip.llm-chat-example --force
```

**Local development notes:**

- **Build first.** The host resolves the worker and UI bundle from `package.json#paperclipPlugin`, so `dist/manifest.js`, `dist/worker.js`, and `dist/ui/` must exist before install.
- **Reinstall after pulling.** If a local-path install was created before the server stored `package_path`, uninstall and reinstall so the host can reactivate the plugin cleanly.
- This example only works with providers that support direct LLM sessions. If the provider list is empty, enable a compatible adapter first.

## Structure

- `src/manifest.ts` — manifest with one `dashboardWidget` slot and direct-LLM capabilities
- `src/worker.ts` — provider discovery plus session-based chat actions
- `src/ui/index.tsx` — dashboard chat widget with provider/model selectors and streaming UI
