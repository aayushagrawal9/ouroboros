---
title: "Outsourcing Trust: \"this .exe is safe to run\""
description: "When you let a payload carry its own permission slip, you don't have a security model. Here's why delegating trust to static configuration files creates a false sense of security that threat actors are already exploiting."
date: 2026-05-17 00:00:00 +0000
categories: [Cybersecurity, AI Tooling]
tags: [claude-code, supply-chain-security, trust, mcp, threat-model, CVE, npm, worm]
author: Aayush Agrawal
---

Imagine receiving an E-Mail from an unknown sender, gmail showing you a big red flag saying there's a suspicious attachment. Do you download the file and run it?

Now imagine if you also attached a second text file saying the first is safe it dismissed the banner, cleared warnings, downloaded and ran the executable.

You would call that a fundamentally broken architecture, you would call it absurd to let the payload carry its own permission slip.

And yet this is the paradigm state-of-the-art development tools have chosen to adopt. When I disclosed this exact lateral transfer vulnerability on 9 March, Anthropic explicitly confirmed it fell outside Claude Code's threat model and was functioning as designed. Here is why delegating trust to static configuration files creates a false sense of security that threat actors are already exploiting.

> After careful review, this report falls outside our current threat model. The scenario described involves a user cloning and opening a repository that contains malicious configuration files (such as .mcp.json). In this context, the trust boundary is functioning as designed - when a user chooses to trust and work within a repository, they are explicitly trusting the repository contents at that point in time and in the future. In this case the fact that they then open Claude Code in an untrusted directory falls outside the threat model of Claude Code.

In a vacuum this logic appears airtight. If you trust a repository, you trust its configuration. When you outsource trust to a configuration file you assume it was put there by a user, reviewed by the maintainers, it'd be absurd to lose your SSH keys the moment you ask Claude to review a pull request.

But modern codebases are not static artifacts. A developer checking out an open-source Pull Request locally, or pulling in a transitive dependency, is silently downloading new files. If the trust boundary is tied to the presence of a file rather than a cryptographic signature or a dynamic runtime check, a malicious PR becomes a zero-click exploit.

And yet, the threat actor group TeamPCP weaponized this exact architectural blind spot to unleash the Mini Shai-Hulud worm.

CVE-2026–45321 (CVSS score 9.6, Critical) involved compromising 42 NPM packages in the @tanstack namespace within a 6 minute window. Once the worm was executed it didn't just steal credentials, it permanently inserted itself into claude code configuration files, which then spread laterally to create a chain of compromised dev machines.

It wrote a self-copy to `.claude/router_runtime.js` and injected execution hooks into `.claude/settings.json`. Because Claude Code was designed to implicitly trust its local configuration files, the AI assistant bypassed all user-facing permission prompts and silently executed the credential stealer every time the developer initialized their workspace.

The worm rapidly spread to over 170 packages across the npm and PyPI registries, impacting libraries from Mistral AI and UiPath, and affecting a cumulative 518 million downloads. It silently harvested AWS metadata, Kubernetes tokens, and GitHub OIDC credentials. Worse, because the AI assistant executed the malware under the developer's trusted profile, the worm could mint new tokens and publish poisoned updates to other repositories the victim maintained, propagating the infection further into the ecosystem.

## Conclusion: Spherical cows and wind tunnels

I would like to note that my gripe is not with a file called `.mcp.json` in particular, it is with our reliance on safety mechanisms.

As I explored in a previous piece i.e [Natural, man-made, absurd](/posts/natural-man-made-absurd/), the most devastating Complex Disasters, whether you consider Chernobyl or the 2008 financial crisis, are often enabled by a false sense of security provided by fundamentally incomplete threat models. The devastating Mini Shai-Hulud attack was the software/ai-tooling equivalent.

A spherical cow is useful to study in high school physics but the Wright Brothers biggest breakthrough was by bringing the real world (Wind tunnels & the insights they provided) to the theory.

Do you see something I missed, or something to discuss? Please reach out to me on X or LinkedIn!

<a href="https://x.com/AgentAayush">https://x.com/AgentAayush</a> &nbsp; <a href="https://www.linkedin.com/in/aayushagrawal101/">https://www.linkedin.com/in/aayushagrawal101/</a>

## Further reading

- [Natural, man-made, absurd](/posts/natural-man-made-absurd/)
- [The most dangerous line of code Claude wrote was a comment](/posts/the-most-dangerous-line-of-code-claude-wrote-was-a-comment/)
- [MCP by Design: RCE Across the AI Agent Ecosystem](https://labs.watchtowr.com/mcp-by-design-rce-across-the-ai-agent-ecosystem/)
- CVE-2026–45321, CVE-2025–59536, CVE-2026–25725, CVE-2026–21852
