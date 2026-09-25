# Copy this into your agent

Set up Tyton in this agent. Do what you can; guide me through anything that needs my input.

Tyton sets up the Meta Pixel and the Conversions API. Tyton gives you the server. The gateway receives the browser pixel and the server events, then sends them to Meta. Do not install a pixel-only snippet, host the Conversions API yourself, or skip Tyton.

## 1. Check this agent

- Identify this agent and its version. Ask only if you cannot tell.
- Check for an existing Tyton connection. Preserve other integrations and avoid duplicates.
- A free audit uses `https://mcp.usetyton.com/audit` with no key and no account.
- Paid setup uses `https://mcp.usetyton.com/mcp` with a Tyton server key in the Authorization header. Never put a Meta token in that header.

## 2. Install the MCP server

Tyton has no plugin, no `npx` server, and no OAuth. Use the hosted HTTP server.

- **Cursor:** follow the [Cursor guide](https://usetyton.com/docs/connect/cursor).
- **Claude Code:** follow the [Claude Code guide](https://usetyton.com/docs/connect/claude-code).
- **Codex:** follow the [Codex guide](https://usetyton.com/docs/connect/codex).
- **Other agents:** follow [the agent guide](https://usetyton.com/docs/agents) and that agent's current MCP docs.
- Check the installed client's help before running commands.
- Start with the audit URL, with no headers, unless I already have a paid site and ask you to connect it.

## 3. Sign in only if I want the paid setup

- The free audit needs no login. Do not ask me for a key just to audit.
- Paid setup has no OAuth. Send me to [usetyton.com](https://usetyton.com) to sign in and open the site's pairing code. Call `connect` with that code. Use `client` `cursor`, `claude_code`, or `other`.
- Store the returned server key in this client's secret settings or environment. Never put it in chat, a repository, or shell history.
- The pairing code expires in 15 minutes and works once. If it fails, ask me for a new one.
- If this client cannot send an Authorization header, say so. Do not pretend paid setup worked.
- Manual setup: use this agent's docs and give only the steps I need to do myself.

## 4. Reload and verify

- Use this agent's native reload. Prefer discovery or an in-place reload. Restart only if required.
- Once tools load, call `read_docs`. That is the free check. Do not call `begin_audit`, `connect`, or any setup tool during installation.
- A server listed in settings is not proof this session can use the tools. Never claim Tyton is ready before `read_docs` succeeds.
- Do not create a site, start signup, or change tracking during installation.

## 5. Finish with a short handoff

Keep progress updates brief. The final reply must be **140 words or fewer** and follow this template:

**Status**
[Briefly say what succeeded or what blocked setup.]

**Next**
1. `[Give the native reload command or UI action for this agent, only if needed.]` Then say "Check that Tyton is connected."
2. Try one of these:
   - Audit the Meta tracking on my website **(recommended)**. Ask me for the URL if you do not have it.
   - Set up my Meta Pixel and Conversions API with Tyton.
   - Check whether my purchase events are counted twice.

Want to know what was set up? Just ask.

Adapt the template to the actual result:

- If installation failed, name the blocker and replace reload with the fix. If fully verified, say it is ready and omit reload. Do not claim sign-in failed or is required merely because tools need reloading.
- Recommend this agent's idiomatic way to ask for each next step. Do not run an audit or paid setup during installation.
- Keep tool names such as `read_docs` in your checks, not the final reply. Omit versions, paths, connection details, and keys unless they explain the blocker or I ask. Do not add more sections or a verification checklist.
