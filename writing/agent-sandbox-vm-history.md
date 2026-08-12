---
title: My Agent Sandbox VM history
slug: agent-sandbox-vm-history
date: 2026-08-12
category: History
tags: [history, security, tooling, agents]
dek: A new job, a machine I had to bring myself, and code I couldn't trust. How a Lima VM stopped being a sandbox and became my work machine.
reading_time: 3 min
---

Recently, I've started a new job at a new company. It was a pretty urgent hire, they didn't provide any machine for me to work on, and I would have to work on my own computer. The compensation was good. The work was relatively easy. There were only some problems with management that I had been told about at the beginning.

I couldn't trust the code. I would not run it on my machine. That's Safety 101 for me, don't run code you don't trust on your machine, period. I was afraid of encountering prompt injections and falling for a scam. So I started to look for something that could help me with this situation, and I came across Fabio Akita's [AI Jail](https://github.com/akitaonrails/ai-jail) project. AI Jail is a good option for creating a process boundary for the agent, you allow access to some things and deny access to others. Simple and direct. But the README itself says that it is not 100% safe. The only way to achieve a higher level of safety is by using a disposable VM. I was in need of a decent sandbox environment to be able to work on projects that I don't fully trust, so I decided to give VMs a try.

My first try was a macOS VM using [UTM](https://mac.getutm.app/). It didn't work. It was slow, and I couldn't see myself working on that machine. I would have to change how I was isolating things, otherwise, I would have to run the code on my machine. So, while exploring the problem with Claude Code, I discovered the option of using [Lima](https://lima-vm.io/) for the VM. After a few tries, I was able to get a small setup for my sandbox. It is a VM config file that mirrors a local folder into the VM, so I can change the files in that folder only. Besides the mirroring, I also have some configurations, like installing Claude Code natively, an Oh My Zsh configuration, and other small things that I add while trying to create something "more robust" and familiar.

At the beginning, I had a network limitation as well, preventing full internet access on the VM so I could have more security. That made sense, but it wasn't actually easy to keep adding domains all the time and restarting everything just to give access to some simple thing that the agent needed. So I dropped it. Now the VM works with full internet access.

The interesting part of this is… I never actually had a real problem with prompt injection (or at least I didn't notice anything). Today, the VM works like my work machine. I keep all the keys, logins, configs, AI memories, and skills on that machine. In the end, it works to protect me against something destructive that the agent might do, and also to keep my credentials isolated.

I'm constantly thinking about changing this into a local application, not necessarily AI Jail. I'm missing something like an interface to work with agents, so I'll probably do something like this in the future. Isolated agent management with credential selection. It would be a good project.
