# I ripped one organ out of the control plane and made it its own thing. It's called readfirst.

So after the whole Gene-seed disaster, the one thing I actually learned was this: if you force-feed an agent a giant pile of rules at the start, it doesn't absorb them as experience. It reads them once like the rulebook of a board game nobody wants to play, nods along, and then two hundred lines later it's casually rewriting the billing table that the mobile app reads, because the rule about that table is buried somewhere back in the prehistoric era of the session. And when I point it out? "You're absolutely right. I apologize." Head bowed, performance tanking, same as always.

The control plane did fix this for my own projects. But let's be honest, that thing is a fucking Imperial bureaucracy at this point. Task contracts, handovers, permission boundaries, a whole database of canonical records, nearly 140 PRs of me going full RimWorld mode. Nobody in their right mind is going to install the Administratum just to stop their agent from breaking one file.

But while I was building all that, there was one piece that kept working no matter what else blew up around it: **hand the agent the rule at the exact moment it touches the thing the rule is about, and nothing else.** Not a 300-line CLAUDE.md that it skims once and forgets. Not my best session's chat log transplanted into a new body like some organ transplant recipient guy. Just, right before it edits this file: amounts are in cents, the mobile app reads this table, run the payment e2e after you touch it. That's it.

So I cut that one organ out, cleaned the blood off, and made it a standalone tool. That's [readfirst](https://github.com/jeonghun917/readfirst).

How it works is stupidly simple, on purpose. There's one small map in the repo, `.readfirst/map.json`, that says which files belong together and, for each group, the rules, the risks, and the checks to run. Right before Claude Code edits a file, a hook shoves the rules for that file in front of its face. Other agents get them through MCP. `npx readfirst verify` runs only the checks for what the branch actually changed, instead of the agent either running every test in the universe or running none and calling it done. And since I know for a fact I'd forget to update the map, a GitHub Action keeps it in step on every PR by itself.

To be clear, it doesn't physically stop the agent from editing anything. It just makes sure the damn thing was told before it did. The one thing it can refuse, if you turn it on, is letting Claude Code declare "done" while a check is still failing, which, if you've ever watched an agent proudly announce victory over a red test suite, you'll understand why that exists.

It ended up following the same rule the control plane finally beat into me: a structure that neither the human nor the LLM has to think about to use. `npx readfirst init` and it's running.

It's free and open source. And if you don't feel like writing the first map yourself (I wouldn't), there's a [hosted version](https://readfirst-cloud.vercel.app/) that reads your repo and opens the map as a PR, with the file and line behind every single rule, so you can check for yourself that it isn't spitting out proud, self-congratulatory bullshit.

[Back to the AI Development Control Plane](../README.md)
