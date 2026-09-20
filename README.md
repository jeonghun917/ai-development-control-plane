# AI Development Control Plane

## Why I started building this
The background of coming up with this system in the first place was while I was making a game like Bannerlord, whenever the session changed, the code, work, or concepts kept getting flipped upside down, and the agent—relying strictly on memory—kept throwing out absolute shit ideas. And whenever I pointed it out, it would just repeat, "You're right. I was wrong just now," bowing its head in deep apology over and over while its performance tanked.

At first, I tried feeding it the entire set of responses up to the best section of the best session, like a Gene-seed or a Primaris Space Marine from Warhammer 40K. But in the end, this agent was no different from the one that lost its damn mind after seeing the work and bowed down in apology. Just replacing the work with chat history was all that changed.

Next, I tried shoving in best practice examples too. But since it couldn't learn from failures, the result was the same. Shoving in failure examples was the same story. In the end, it was just exception after exception after exception, slowing down work progress to a crawl.

So the next thing I tried was making it generate reports and handover documents for every single task. But here, managing the canonical state of documents became a bigger job than the actual work itself. On top of that, individual documents started contradicting each other, or cases popped up that the reports couldn't even cover.

And that's how the current automatic handover feature came to be.

## How the handover system evolved
Looking back at the old dashboard, to be honest, the whole idea was just "let's classify the existing documents themselves so I can look over them and catch things in advance." At that point, the document classification structure on Drive wasn't completely fucked up yet. But as I kept going, starting from permissions, I saw the LLM interpreting things however the fuck it wanted, calling that the "canonical" state, and spitting out proud, bullshit self-congratulatory nonsense—even though it was supposed to be a deterministic system. So what I added to fix that was an automatic handover structure, which included task continuity, explicit task scopes, and permission boundaries.

Then at some point, as the structure grew bigger, I suddenly felt the LLMs becoming complete fucking dumbasses. Looking into it, they couldn't handle tasks, and they couldn't update the canonical records properly either. It was just a complete mess. As a solution, the next thing I tried was, "Then let’s automatically inject context straight into its brain to strip away its ability to think for itself." At the same time, all those projects that were using the system however they pleased like it was the Warring States period—I just cut them down and forced them into one system.
However, context injection turned out to be just another Gene-seed made out of rules anyway, so I scrapped it and tried a redesign. But for this redesign too—like a complete fucking idiot, without realizing how ridiculously huge the structure was getting—I got all excited and went full RimWorld mode. In the end, I had to wipe all of that out, too.

What I realized at that moment was that a simple structure—a structure that neither humans nor LLMs have to think to use—is the absolute best. The key point is that this took fucking forever to figure out. By this point, it had swallowed up nearly 140 PRs.

## Notes

- [I tried using my best AI session as a Warhammer 40K gene-seed. It didn't work.](docs/gene-seed-experiment.md)
