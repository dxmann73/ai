# The thing that writes the instruction file

Instruction files (`AGENTS.md`, `CLAUDE.md`) are where you put the things agents frequently get
wrong, and the things too important to leave to inference. Today the human writes them, by hand,
from memory of the last time something went sideways. That is the weakest link in the whole
arrangement: the file is only as good as what one person happened to remember to write down.

What should exist instead is an entity that reads the traffic — agent-to-human and agent-to-agent
communication — and looks for patterns that recur. A correction the human types three times in a
week is a rule that is missing from the file. A clarification one agent keeps having to give another
is the same signal. The recurrence is the evidence; the instruction file is where the evidence goes
once there is enough of it.

So the trigger for updating an instruction file is not a feature landing and not a scheduled review.
It is **frequency**, observed in the communication itself. The candidate list writes itself if
anything is watching.

Open parts:

- What does it watch? Session transcripts, review comments, rebase and merge chatter, the corrections
  a human types into a running session. All of it is already text and already stored.
- What is the threshold? "Three times" is a guess. The real question is whether the signal is
  frequency alone or frequency weighted by cost — a rare mistake that is expensive to unwind may
  deserve a line before a frequent one that is cheap to correct.
- Who accepts the candidate? Mining produces candidates, not rules. Something has to say yes, and
  the file gets worse if that step is automatic, because the file has a size ceiling and every line
  is paid for on every session.
- Does the same entity propose deletions? It should. The patterns it stops seeing are the rules that
  have been internalized or made mechanical, and those are exactly the lines to remove.

This connects to the size ceiling argument in [`aglc/artifacts.md`](../aglc/artifacts.md): the
instruction file is the one artifact whose cost is per-invocation rather than per-revision, so it
cannot only grow. A miner that only ever adds is a miner that eventually breaks the thing it is
maintaining.
