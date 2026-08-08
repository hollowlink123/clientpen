# ✦ Clientpen

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![n8n](https://img.shields.io/badge/built%20with-n8n-orange.svg)
![Groq](https://img.shields.io/badge/AI-Groq%20Llama%203.3-purple.svg)
![Status](https://img.shields.io/badge/status-active-brightgreen.svg)

**Six AI-powered automations for the admin work every freelancer dreads: proposals, contracts, invoices, onboarding, updates, and testimonials. Free, self-hosted, and fully yours.**

Fill in a form. AI writes the document. It gets emailed, logged, and tracked automatically. No subscriptions, no SaaS lock-in, no third party holding your client data.

<img width="945" height="909" alt="form" src="https://github.com/user-attachments/assets/d87c8712-e042-4286-94ee-64e4019fa343" />


---

## Table of Contents

- [Overview](#overview)
- [Why Clientpen](#why-clientpen)
- [What's Included](#whats-included)
- [Repository Structure](#repository-structure)
- [Before You Start](#before-you-start)
- [Setup, Step by Step](#setup-step-by-step)
- [Make It Yours](#make-it-yours)
- [Running n8n 24/7](#running-n8n-247)
- [Built With](#built-with)
- [Known Limitations](#known-limitations)
- [Security Notes](#security-notes)
- [Troubleshooting](#troubleshooting)
- [Need Help?](#need-help)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

Clientpen is a set of six n8n workflows plus one simple web form. Each workflow handles a specific piece of freelance admin work: writing a proposal, drafting a contract, chasing an unpaid invoice, onboarding a new client, sending a weekly update, or asking for a testimonial.

You run it yourself, on your own n8n instance, using your own Groq API key and your own Gmail account. Nothing is hosted for you, and no client data passes through anyone else's server.

It's built for freelancers who use, or are willing to try, n8n. If you've never used n8n before, the Setup section below walks through everything.

---

## Why Clientpen

Every freelancer loses hours a week to the same repetitive admin. Writing a proposal from scratch. Drafting a contract. Chasing an unpaid invoice. Welcoming a new client. Writing a status update. Asking for a testimonial without it feeling awkward.

Clientpen turns each of these into a single form submission. You type what you know, AI does the writing, n8n does the sending, and everything runs on infrastructure you control.

---

## What's Included

| Tool | What it does |
|---|---|
| ✦ **Proposal Generator** | Turns a project brief into a tailored proposal PDF and emails it to your client |
| 📋 **Contract Generator** | Drafts a complete freelance contract from your form details and sends it straight to the client |
| 💰 **Invoice Chaser** | Sends the invoice, then follows up automatically with a reminder and a final notice if it goes unpaid |
| 🤝 **Client Onboarding** | Sends a welcome email and project questionnaire the moment a new client signs on |
| 📊 **Weekly Update Bot** | Turns a quick progress note into a polished client update email |
| ⭐ **Testimonial Collector** | Sends a warm, specific request for a testimonial once a project wraps up |

Each tool is a separate, independent n8n workflow with its own webhook. Import only the ones you actually want to use.

**Optional: human-in-the-loop versions.** Proposal Generator and Contract Generator both send straight to the client by default, no review step. If you'd rather approve documents yourself before they go out, `proposal-generator-with-approval.json` and `contract-generator-with-approval.json` add a review step: the drafted PDF is emailed to you first, and only sends to the client after you approve it. Reject it instead, and the client never sees it. These are optional drop-in replacements, same webhook pattern, same form fields, just with an approval gate added.

---

## Repository Structure

```
clientpen/
├── workflows/
│   ├── proposal-generator.json
│   ├── proposal-generator-with-approval.json
│   ├── contract-generator.json
│   ├── contract-generator-with-approval.json
│   ├── invoice-chaser.json
│   ├── client-onboarding.json
│   ├── weekly-update-bot.json
│   └── testimonial-collector.json
├── assets/
│   └── images/
│       ├── form.png
│       └── proposal-example.png
├── clientpen-form.html
├── README.md
├── LICENSE
└── .gitignore
```

---

## Before You Start

You'll need:

- A running [n8n](https://n8n.io) instance, self-hosted or cloud
- A free [Groq API key](https://console.groq.com) for the AI writing
- A Gmail account you can connect inside n8n, for sending emails
- Any modern browser. The form is a single HTML file, nothing to install

---

## Setup, Step by Step

1. **Import a workflow.** In n8n, click *Import from File* and choose the `.json` file for the tool you want, for example `contract-generator.json`
2. **Connect your credentials.** Open the Groq node and every Gmail node inside the workflow, and connect your own API key and Gmail account
3. **Publish the workflow.** Flip it Active, top right of the canvas
4. **Copy the Production Webhook URL.** Click the first (webhook) node and copy the Production URL shown there, not the Test URL
5. **Paste that URL into the matching tool's Connect field** inside `clientpen-form.html`
6. **Repeat for every tool** you want running
7. **Open `clientpen-form.html`** in your browser, fill it in, and hit submit

That's the whole setup. The form itself needs no hosting. It runs entirely in your browser and talks directly to your n8n webhooks.

---

## Make It Yours

Before sending anything to a real client, test each workflow using your own email address first. Submit the form and see exactly what the PDF and emails actually look like.

If you don't like the current look, the layout, colors, and fonts all live in the "Build [X] HTML" Code node for each workflow. Change a color code or font name there. No real coding experience needed.

---

## Running n8n 24/7

If you're self-hosting on a VPS, make sure n8n keeps running after you close your terminal, otherwise your workflows will silently stop responding.

**Docker:** add this to your n8n service in `docker-compose.yml`

```yaml
restart: always
```

**No Docker:** use [PM2](https://pm2.keymetrics.io/)

```bash
pm2 start n8n
pm2 save
pm2 startup
```

---

## Built With

- [n8n](https://n8n.io), workflow automation
- [Groq](https://groq.com) (Llama 3.3 70B), AI writing
- Gmail, email delivery
- HTML2PDF, document generation
- Google Sheets, lightweight logging
- Vanilla HTML, CSS, and JS for the form, zero dependencies

Want to use a different AI provider? Edit the URL and request body in the HTTP Request node that calls Groq. Not a dropdown yet, just a manual swap.

---

## Known Limitations

Being upfront about what this doesn't do yet:

- **No error alerting.** If a workflow fails, an expired credential or an API outage, you won't be notified automatically. Check n8n's Executions tab if something seems off.
- **AI writes a strong first draft, not a finished document.** Review proposals and contracts before sending anything high stakes.
- **No live payment link in Invoice Chaser**, by design. Freelancers state their preferred payment method as plain text instead.
- **Requires basic n8n comfort.** Importing a workflow, connecting a credential. Not fully no-code yet.
- **One line item per invoice or contract.** No multi-item breakdowns for now.

---

## Security Notes

This is a lightweight, self-serve tool. A few things worth knowing before you deploy it publicly:

- **Your webhook URLs are the first layer of access control.** Anyone who has your Production URL can submit data to that workflow. Don't share your real webhook URLs publicly (screenshots, forum posts, commits), and treat them like a lightweight secret.
- **Optional second layer: a shared passphrase, checked with an IF node.** The form has an optional "Webhook passphrase" field in Settings. If you set one, it gets included as a normal field in every submission (not an HTTP header, since that approach breaks in browsers due to CORS preflight restrictions, so don't use n8n's built-in Header Auth here).

  Example of setting this up end to end:
  1. Pick your own secret value, e.g. `myproject_9xQ2` (anything long and hard to guess).
  2. In each n8n workflow, add an **IF node** right after the Webhook node, checking that `{{ $json.body.passphrase }}` equals `myproject_9xQ2`.
  3. Wire the *true* path to continue as normal, and the *false* path to a **Respond to Webhook** node returning something like `{ "status": "unauthorized" }`, so mismatched submissions get rejected cleanly instead of running.
  4. Open `clientpen-form.html` → Settings → paste that same `myproject_9xQ2` into the "Webhook passphrase" field. It saves automatically in your browser and gets sent with every submission from then on.
  5. Repeat the IF node setup (steps 2–3) in each of your 6 workflows, reusing the same passphrase everywhere.
- **No login or authentication system exists on the form itself**, by design, to keep this simple and free. The passphrase above is the closest equivalent: it's a shared secret, not per-user accounts.
- **Never hardcode API keys or secrets into the HTML form.** All credentials (Groq, Gmail, Google Sheets) should only ever be connected inside n8n itself, never inside `clientpen-form.html`.
- **This applies whether you're self-hosted or on n8n Cloud** since both handle credentials, IF nodes, and webhook exposure identically. n8n Cloud runs workflows continuously with no manual steps required, same as a properly configured self-hosted instance; neither hosting type is inherently more or less secure than the other for this setup.

---

## Troubleshooting

**Same test data every time, even after changing the form**
n8n caches the last test payload. Click the webhook node → *Listen for test event* → submit the form fresh → check the output.

**A node shows undefined for fields you filled in**
Check the node right after your webhook. Every field the form sends needs to be listed in that node's output.

**"Referenced node doesn't exist" error**
An HTTP Request node (like the Groq call) between the two nodes can break named references. Fix: use a real Merge node instead, wired directly on the canvas, then read from `$json`.

**Webhook returns "not registered" in production**
Workflow needs to be Active, and you need the Production URL, not the Test URL.

**Form shows a generic failure but nothing shows up in n8n's Executions tab**
The request never reached n8n. Double check the Production URL was actually pasted into the correct tool's field in Settings, and that you're testing the form via a real address (`http://` or `https://`) rather than opening the HTML file directly, since some browsers block requests from a raw `file://` page due to security restrictions.

**Webhook returns a 403 error**
This almost always means Header Auth is still enabled on the Webhook node's Authentication setting. Set it back to **None**. This project uses a body-level passphrase checked with an IF node instead (see Security Notes above), since Header Auth doesn't work reliably from a plain browser form.

**Test URL keeps showing even after activating the workflow**
This is a known n8n editor quirk, not something wrong with your setup. Copy the URL directly from inside the Webhook node right before testing, and confirm the URL text itself contains `/webhook/` rather than `/webhook-test/`. If it's still stuck, deactivate the workflow, reactivate it, and copy the URL fresh, or duplicate the workflow into a new one if the issue persists.

---

## Need Help?

Open an issue on this repo if you run into a problem.

---

## Contributing

MIT licensed. Fork it, make your changes, open a pull request. All PRs get reviewed before merging.

Ideas if you want to contribute:
- An error handling workflow that alerts on failure
- E-signature integration for contracts
- Multi-line-item support for invoices and contracts
- Support for other AI providers
- Translations of the form and email templates

---

## License

MIT, use freely, modify as needed, contribute back if you can. See [LICENSE](./LICENSE).

⭐ If Clientpen saves you time, consider starring the repo.
