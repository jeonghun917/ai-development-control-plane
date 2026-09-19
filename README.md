# AI Development Control Plane

## Why I started building this
The background of coming up with this system in the first place was while I was making a game like Bannerlord, whenever the session changed, the code, work, or concepts kept getting flipped upside down, and the agent—relying strictly on memory—kept throwing out absolute shit ideas. And whenever I pointed it out, it would just repeat, "You're right. I was wrong just now," bowing its head in deep apology over and over while its performance tanked.
At first, I tried feeding it the entire set of responses up to the best section of the best session, like a Gene-seed or a Primaris Space Marine from Warhammer 40K. But in the end, this agent was no different from the one that lost its damn mind after seeing the work and bowed down in apology. Just replacing the work with chat history was all that changed.
Next, I tried shoving in best practice examples too. But since it couldn't learn from failures, the result was the same. Shoving in failure examples was the same story. In the end, it was just exception after exception after exception, slowing down work progress to a crawl.
So the next thing I tried was making it generate reports and handover documents for every single task. But here, managing the canonical state of documents became a bigger job than the actual work itself. On top of that, individual documents started contradicting each other, or cases popped up that the reports couldn't even cover.
And that's how the current automatic handover feature came to be.
