---
name: helpalive
description: Install and configure HelpAlive, an AI agent that runs inside a web app, answers users' questions from the app's docs, and does tasks for them or shows them how. Use this skill to add the HelpAlive script, call identify() and reset(), verify users with a server-signed token, allow HelpAlive in a Content Security Policy, or troubleshoot an install that shows nothing.
metadata:
    mintlify-proj: helpalive
    version: "2.0"
---

# HelpAlive

HelpAlive is an AI agent inside your web app. It answers your users' questions from your published docs, and does tasks for them in the app or shows them how. It reads the page's structure, not screenshots. Installing it takes one script tag, one `identify()` call for the signed-in user, and one `reset()` call at logout. Nothing in the app needs tagging.

Full docs: https://docs.helpalive.com. Append `.md` to any page address for Markdown. Every page: https://docs.helpalive.com/llms-full.txt.

## When to use

- Adding HelpAlive to a web app for the first time.
- Passing the signed-in user and their company to HelpAlive.
- Making users verifiable with a token signed on the server.
- Allowing HelpAlive in a Content Security Policy, or installing through Google Tag Manager.
- Finding out why the agent doesn't appear.

Training the agent, choosing who gets it, and billing are done by an admin in the HelpAlive dashboard (https://app.helpalive.com), not in code.

## Install

1. Put this tag in the `<head>` of the layout every page uses. The project key is public and is on **Settings → Setup & API Key**.

```html
<script src="https://cdn.helpalive.com/sdk/helpalive.js" data-api-key="YOUR_PROJECT_KEY" async></script>
```

2. Wherever the app knows the signed-in user, right after login and on every page load, add the queue lines and then `identify()`:

```javascript
// Lets the calls below run before the script has loaded. Keep it first.
window.HelpAlive = window.HelpAlive || { q: [] };
["identify", "reset", "setConsent", "track"].forEach((m) => {
  if (!HelpAlive[m]) HelpAlive[m] = (...args) => HelpAlive.q.push([m, args]);
});

// B2B: users belong to tenants
HelpAlive.identify({
  userId: String(user.id),      // required: your own id for this person
  tenantId: String(company.id), // required: their company or workspace
  tenantName: company.name,     // required: the company's name, shown in your dashboard
  displayName: user.name,       // required: the person's name (or their email), shown in your dashboard
  email: user.email,            // optional
  role: user.role,              // optional: e.g. "admin", "editor", "viewer"
  plan: company.plan,           // optional: e.g. "free", "pro", "enterprise"
  createdAt: user.createdAt,    // optional: signup date, Unix time in seconds
});
```

For a B2C app, where users have no company or workspace, leave out `tenantId` and `tenantName`; the dashboard groups those users as B2C customers. `userId` and `displayName` are always required.

```javascript
// B2C: users have no tenant
HelpAlive.identify({
  userId: String(user.id),      // required: your own id for this person
  displayName: user.name,       // required: the person's name (or their email), shown in your dashboard
  email: user.email,            // optional
  plan: user.plan,              // optional: e.g. "free", "pro"
  createdAt: user.createdAt,    // optional: signup date, Unix time in seconds
});
```

3. In the logout code, apart from `identify()`, call `HelpAlive.reset()`.

Details: https://docs.helpalive.com/installation.md

## Rules that fail silently

- No `data-api-key` on the tag: nothing starts and nothing is logged.
- `identify()` never runs or has an empty `userId`: the agent never appears. It shows only for signed-in users.
- A Content Security Policy must allow `script-src https://cdn.helpalive.com` and `connect-src https://api.helpalive.com https://chat.helpalive.com`. With a nonce, put the nonce on the HelpAlive tag. Never add `unsafe-inline` for HelpAlive.
- The agent appears only after an admin publishes a source on **Train** and switches the agent on for the user on **Controls**.
- `setConsent()` does nothing. It stays in the queue list so older pages don't break.
- Add `data-debug` to the tag while testing to see the reasons in the browser console. Remove it before shipping.

## Verify users (optional, recommended)

Sign a token on the server and pass it as `userToken` in place of `userId` and `tenantId`:

- HS256, keyed on the signing secret from **Settings → Setup & API Key**.
- Claims: `sub` (the user id, as a string) and `tenant` (the company id, as a string; leave it out if the app has none). No other claims.
- Never put the signing secret in the browser, the repository, or a prompt.

Details and server code in Node, Python, Ruby, PHP, Java and Go: https://docs.helpalive.com/sdk/verify-users.md

## Check it works

On **Settings → Setup & API Key**, the panel says **Everything is working.** when the required checks are green; the last one turns green once a test from **Test it on your app** has run. Then turn the agent on for users on **Controls**. Sign in to the app as a user who gets the agent and open the chat button.

## More

- Troubleshooting: https://docs.helpalive.com/sdk/troubleshooting.md
- Content Security Policy: https://docs.helpalive.com/sdk/csp.md
- JavaScript reference: https://docs.helpalive.com/sdk/reference.md
- Support: support@helpalive.com
