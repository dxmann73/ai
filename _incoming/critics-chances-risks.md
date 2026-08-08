> Captured from website `src/content/ai/en/critics-chances-risks.mdx` on 2026-08-08. Verbatim, unedited.

---
title: "AI — Critics"
---

# Critics and concerns

As with every new technology, there are a lot of sceptics, critics and concerned parties.

Some examples on this page

[George Hotz - The eternal sloptember](https://geohot.github.io//blog/jekyll/update/2026/05/24/the-eternal-sloptember.html)

Key point: Humans needed to sort the slop. Will the average engineer be able to do that, especially if held to the 10x narrative?

> A trait you find in all high performing people is the ability to error correct, and they have mostly been good at seeing when slop is slop.
> The bottom performers won’t have that self check. They are the ones producing 10x output with the agents.
> What do you think is happening to the average output of [their] organization?

[Steve Yegge on the creation of Beads]()

Steve (famously of Gastown) is not what you would expect to be a critic by any stretch, but his account on how to build
an agent automation framework just by vibe coding is a very insightful read, and hilariously so. Choice quotes,
lining out clearly why plans are a vehicle but not a means of engineering complex endeavours:

> Unfortunately, my paratroopers always got lost in Plan Jungle, and would quickly be sniped by locals. [...]
> I discovered I had six hundred and five markdown plan files in varying stages of decay in my plans/ folder.
> Yowza. 605 inscrutable plans. What was happening to my Master Plan?

> Unfortunately, typical real-world engineering workflows tend to be very long and need to span many agent compaction sessions
> Moreover, and worse, in the real world, your engineering workflows tend to nest as new stuff comes up.
> Easy enough: you push that UI workstream onto your mental stack, to be continued later.
> Unfortunately, the AI has no way of tracking this implicit stack, because [read: IF and WHEN - ed.] it keeps all of its plans,
> at all levels, in sibling markdown files with the same format and bland names.

Steve then goes to identify the same solution that Matt Pocock came up with: Write the PRD, write plans (you need them), but connect
the dots using issues that depend on each other and set a narrow focus. The fact that Matt links this to "The dumb zone" aka context window size,
while Steve finds agents get lost in all the plans, points in the same direction already lined out in the communication part. You won't give
all your contractors all your documents and specs to digest if the task you need doing (and will have to verifiy later) is actually not that broad
at all. And if it is, you're well advised to slice it into smaller pieces. That's the way houses get built today.

> I can’t connect to your main database so I’m just going to implement a miniature sidecar database just for this one code path.
> Those test failures are pre-existing and have nothing to do with my work here so I’m going to ignore them and push anyway,


Not only he amount of cognitive load, but lack of engagement with the process and working utterly alone is draining people
https://pydantic.dev/articles/the-human-in-the-loop-is-tired?ck_subscriber_id=2755325874

[Genie is not going back, might as well ride the wave](https://forwardfuture.com/newsletter/originals/genie-s-not-going-back-in-the-bottle)
In my opinion, two types of people will win with AI.
The first are those who are already experts in their domain (or intend to become experts);
they know exactly which tasks to delegate to AI, freeing them up to work on the rest and innovate.
The second are those who will use AI to rapidly learn new things, specifically focusing on vertical
integration to expand their footprint into new domains.


[Revenge of the Junior Developer: Who will win](https://sourcegraph.com/blog/revenge-of-the-junior-developer)
 > That's the situation as I see it today.
 > We find ourselves in a big race in the AI ocean, beslapped by increasingly violent waves.
 > The ones who make it will ride those waves.
