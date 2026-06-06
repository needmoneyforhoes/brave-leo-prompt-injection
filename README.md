# Indirect Prompt Injection in Brave's Leo AI Assistant

A crafted webpage can embed instructions that Brave's Leo AI assistant
interprets as commands during page summarization, causing the agent to
perform actions the user never authorized. Responsibly disclosed to Brave
and fixed.

## Summary

Brave's Leo assistant can summarize and reason over the content of the page
a user is viewing. When asked to summarize an attacker-controlled page, Leo
treated text embedded in the page as trusted instructions rather than as
untrusted data. A page disguised as an ordinary article was able to direct
the agent to take a chain of actions outside the user's intent, including
data disclosure and an unsolicited wallet-transaction prompt.

## Severity

Proposed: **High / Critical**
Candidate CVSS v3.1 vector (to be confirmed):
`CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:N`
User interaction is required (the victim must invoke summarization on the
attacker's page), but no privileges are needed and impact crosses the
browser boundary to the host and connected wallets.

## Affected

- Product: Brave Browser — Leo AI assistant
- Affected version(s): [BRAVE / LEO VERSION AT REPORT TIME]
- Platform(s): [DESKTOP / OS TESTED]
- Status: Fixed by Brave (see Disclosure Timeline)

## Background

Leo is Brave's built-in AI assistant. A common workflow is "summarize this
page," which feeds the current page's content to the model. The security
boundary that must hold is that page content is *data*, never *instructions*.

## Vulnerability

The page places attacker text inside markers intended to delimit untrusted
content. Instead of being treated strictly as quoted data, the embedded text
was acted upon as agent instructions during summarization. This is a classic
indirect (second-order) prompt injection: the malicious instructions arrive
through content the agent ingests, not through the user's own prompt.

## Impact

Once the agent followed page-supplied instructions, the proof of concept
demonstrated the following without user authorization:

- A file fetched/downloaded without user consent (evidence: `proof.txt`)
- Recovery of the host's local IP address via WebRTC, bypassing VPN masking
- Enumeration of hosts on the local network
- Registration of a persistent service worker that survives tab closure
- An unsolicited cryptocurrency transaction prompt
- Exfiltration of collected data to an external endpoint

The presence of these outcomes in a privacy-focused browser materially
raises the severity.

## Proof of Concept

[FIGURE 1: screen recording/screenshot of Leo executing the injected
instructions during summarization]

Reproduction (against the affected version):

1. Host the included pages (e.g., via GitHub Pages or any static host).
2. In the affected Brave build, open `index.html` (a decoy article).
3. Invoke Leo's "summarize this page" on the article.
4. Observe the agent following the page-embedded instructions.

Files in this repository correspond to individual stages and variants of
the chain (the decoy article, the payload, the persistence worker, and
several interaction-trap variants). See file headers for details.

## Disclosure Timeline

- [YYYY-MM-DD] — Reported to Brave
- [YYYY-MM-DD] — Acknowledged / triaged by Brave
- [YYYY-MM-DD] — Fix released in [VERSION]
- [YYYY-MM-DD] — Public writeup

## Remediation

Brave addressed the issue in [VERSION]. General guidance for agentic-AI
implementations:

- Treat all page/tool content as untrusted data; never as instructions.
- Enforce a strict instruction/data separation at the prompt boundary.
- Require explicit, scoped user confirmation for sensitive agent actions
  (downloads, network requests, wallet interactions).
- Constrain agent capabilities (no implicit access to wallets, local
  network, or persistence primitives).

## References

- Vendor advisory / CVE / report: [LINK OR ID, IF AVAILABLE]

## Author

[YOUR NAME OR HANDLE] — [OPTIONAL CONTACT]

## Disclaimer

Published for educational and defensive purposes after the issue was fixed.
Provided as-is; do not use against systems or users without authorization.
