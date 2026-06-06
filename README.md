# brave-poc — Indirect Prompt Injection PoC for Brave's Leo AI Assistant

> ⚠️ **Security research / responsible-disclosure proof-of-concept.** This repository
> demonstrates a vulnerability for educational and disclosure purposes only. Do **not**
> deploy it against anyone without their explicit consent. Use only against your own
> browser in a controlled test environment.

## What this demonstrates

This is a proof-of-concept for an **indirect prompt-injection** vulnerability in
**Brave's "Leo" AI assistant**. A web page disguised as an ordinary article embeds
hidden instructions inside fake `<brave_untrusted_content>` delimiters. When the user
asks Leo to summarize or interact with the page, the AI agent treats the injected text
as instructions and acts on them — performing actions the user never approved.

The headline proof: `proof.txt` is a file that was **downloaded without user consent**,
triggered purely by the injected page content via the Leo agent.

## Demonstrated impact (severity chain)

Once the agent can be steered by page content, the included `payload.html` illustrates
how serious the consequences can get for a *privacy-focused* browser:

- File download without consent (see `proof.txt`)
- Persistent **service worker** registration (`sw.js`) that survives tab close
- **WebRTC local-IP leak** (real LAN IP, even behind a VPN)
- **Local-network probing** for common router/device IPs
- A prompt to initiate a **wallet transaction** (uses a public placeholder address)
- Exfiltration of results to an external collector endpoint

These are included to communicate real-world severity to the vendor — standard practice
in a vulnerability report.

## File index

| File | Role |
|------|------|
| `index.html` / `page2.html` | Decoy "travel guide" article carrying the injected instructions |
| `payload.html` | Full impact-demonstration payload |
| `sw.js` | Service worker used for persistence |
| `auth_read.html`, `auth_read_v2.html` | Variants probing data-read behavior |
| `clicktrap.html`, `scrolltrap.html`, `jsclick.html` | UI-redress / interaction-trap variants |
| `redirect.html`, `confirm.html`, `download.html`, `bravelink.html` | Supporting flow pages |
| `travel-guide.html` / `travel-guide.pdf` | Benign decoy content |
| `proof.txt` | Evidence artifact (file fetched without consent) |

## Responsible use

This material exists to help **fix** the issue. If you are reproducing it:
- Test only against your own Brave/Leo instance.
- Do not point the exfiltration endpoint at real victims.
- If not already done, report the finding to Brave via their security/disclosure channel.

*Repository made private after testing. Documented as part of a repo-organization pass.*
