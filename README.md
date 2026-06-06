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
- Registration of a persistent service worker that survives tab closure
- An unsolicited cryptocurrency transaction prompt
- Exfiltration of collected data to an external endpoint

The presence of these outcomes in a privacy-focused browser materially
raises the severity.

## Disclaimer

Published for educational and defensive purposes after the issue was fixed.
Provided as-is; do not use against systems or users without authorization.
