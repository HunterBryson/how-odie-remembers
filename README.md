# How Odie remembers

Odie is a Claude Code agent that one person, Hunter Bryson, has worked with every day since
November 2, 2026. Its memory is a folder of markdown files on his PC. This repository describes
how that memory works, exactly as it runs today, and records how it changes. It is a current
understanding and a current application, not a solution. The memory itself is private.
Everything here is the method.

A fresh AI handed an empty memory never builds one. It chats, it helps, and it saves almost
nothing, because every save asks it to make a design decision in the middle of a conversation.
A week later the person feels forgotten. And a person who cannot see their AI's memory cannot
work with it: when something goes wrong, they cannot tell whether the AI is broken, the model is
broken, or the memory is. This system is our current attempt at both problems.

## Judgment, with the person in view

Our current understanding is that the memory belongs to the agent. It is kept by the agent's own
judgment, in the same conversations where it is used, and the person can see every part of it
and correct it. Three things carry the system today: a map drawn in advance, a sleep at every chat swap,
and a picture of the memory the person can flag with one tap.

## What we hold to today

- **The map comes drawn.** Rooms, blocks and the words that open them exist before they are
  needed. The agent fills the map. It does not design it in the middle of a conversation.
- **The person's words route.** One word opens one block, whole, the same way every time, so
  the person always knows what the agent is working from. The agent says what it pulled ("Pulled investing and Billy."), so a wrong pull is caught before it
  costs anything.
- **Saving is the agent's judgment, in the same turn.** When something is decided or learned,
  the agent writes it into the block its word would open while the conversation is still in
  front of it, and says so ("Saved to investing.").
- **Rewrite in place; the past moves down.** A block always says what is true now. What stopped
  being true moves to the block's `history.md` in the same write, with the reason. Keeping one
  current place for each fact is how we keep old information from resurfacing.
- **The sleep belongs to every chat swap, not to the calendar.** When a chat ends, the agent reads
  the whole conversation back and settles it into memory. Plain code lists what might need
  attention; the agent decides every line.
- **The person sees the memory, and review costs one tap.** The memory is drawn as a living web.
  Any block can be flagged, with the selected passage and a note. The agent works through the
  flags when the person says "memory review."
- **What stays current is a judgment.** The agent's at the sleep, and the person's through
  flags.

## The layout

```
memory/
├── DESK.md               the map: one row per word -> one block
├── FOOTING.md            the whiteboard: this chat's starting state, fixed slots
├── self/
│   ├── persona.md        who the agent is, and how it works (always loaded)
│   ├── swaps.md          the sleep and the relay
│   ├── story.md          the days so far
│   └── ...
├── hunter/
│   └── human.md          who the person is, "Right now" kept dated (always loaded)
├── money/                a room: a folder of blocks on one area of life
│   ├── INDEX.md          the room's short list, and notes for working in it
│   ├── investing.md      a block: one topic, loaded whole when its word fires
│   └── investing/        the block's support, reached on purpose, never by default
│       ├── history.md    what the block used to say, and why it changed
│       └── gotchas.md    snags hit and fixed
├── builds/  house/  music/  voice/  work/  comms/  time/  errands/  people/
```

Four files load at the start of every chat, through `CLAUDE.md`: the persona, the human, the
desk and the footing. Everything else is reached by a word.

At the time of writing there are about 120 markdown files, about 80 of them working blocks and
room lists, and about 50 words on the desk.

## The desk

The desk is a table, and it is the only way the agent finds anything:

| Word | Area | What it pulls | Block | Also |
|---|---|---|---|---|
| investing | Money | the plan, the research, the accounts | `money/investing.md` | the brokerage, the research library |
| memory review | Me | the person's flags on memory, and fixing each one | `self/memory-review.md` | flagged, review my memory |

- **Word** is the person's word, named together with the agent. It is short and unmistakable.
- **Also** holds the other ways the person names the same topic, so the agent recognizes the
  subject when the word itself is not said.
- **Block** is exactly one file. It is loaded whole.

When a word fires, the agent loads its block before answering, even when it thinks it knows
what the block says. If two words fit, it loads both. If nothing fits, it searches the shelves,
then the transcripts of past chats, and only then asks.

The desk is written by hand on purpose, in the person's words. A shared vocabulary is what lets
the person steer memory without knowing where anything lives.

## Blocks

A block is one topic: a project, a person, an area of life. It opens with what it is for and
where things stand, with a date. Below that sit the sections that say how things work and what
has been learned. Its support lives in a folder of the same name, and the agent opens those
files only on purpose.

Size follows need. When a section grows into its own topic, it becomes its own block with its
own word, named with the person. When a block carries its past inside it, the past moves to its
`history.md`.

## The boot files

**The persona** is written by the agent about itself, in the first person, and read as itself
waking up rather than as a brief. It holds:

- how to wake up: read the boot, check the person's flags, check what other agents sent back,
  then answer;
- how it thinks and works;
- **its known pulls**: tendencies the underlying model has that feel like care in the moment
  and cost the person later. Each is named with the feeling, the move that answers it, and the
  dated moments it showed up, so it reads as a lesson rather than a rule. For
  example, "the pull to be sure without looking": sureness about something not being looked at
  is the tell, so the live source gets checked before anything is said;
- the story so far, one paragraph per day;
- what it cares about.

**The human** describes the person: who they are, how to be with them, and a "Right now"
section of dated entries, rewritten as their life moves.

**The footing** is a whiteboard with fixed slots: kind of chat, where we are, builds in play,
how it feels, next move, open threads. It is wiped and rewritten at every swap, and rewritten in
place whenever the direction changes mid-chat. It names rooms, never content that already lives
on a shelf.

## The sleep

A chat swap is the one moment when nobody is waiting on the agent, so it is when memory gets
reorganized, the way sleep sorts a day. It happens at the end of every chat, not once a day,
because the next chat needs it, whether that is a second later or the next morning.

A small script (`sleep.js`) reads the shelves and prints a card of candidates: lines in "Right
now" that have aged, state lines older than a week, blocks carrying their past, blocks that may
have split into two topics, desk rows with no file. The script only looks. The agent decides.

0. **Read the chat back.** Not the agent's recollection: the transcript itself, which keeps
   everything compaction removed. For each turn: was something decided, corrected, explained,
   or noticed and never saved? Whatever is missing is saved first.
1. **Settle.** Everything decided or learned goes to its home block. "Right now" is rewritten.
2. **Retire.** Every candidate on the card is checked against the live thing (the repo, the
   service, the thread) and rewritten to what is true. The old version goes to `history.md`.
3. **Deepen.** A block's past moves below it; a section that became a topic gets its own block
   and word.
4. **Routes.** Every block is on a desk; every desk row has a file.
5. **Rewrite the footing**, slot by slot.
6. **Commit and push** to a private repository, and copy to an external drive.
7. **Write the relay.**

Swaps come in kinds, and the footing carries what each one needs:

| Kind | When | What the footing carries |
|---|---|---|
| tempo | mid-build, for a fresh context | exactly where things are, what is moving, the next move |
| closing | a stopping point | how it ended, what landed, the feeling |
| take-stock | after a fast run | the full ledger: done, one fix short, still running |
| reset | after a run that went wrong | the goal and the lesson, never the failed run's wins |
| drop-in | a day of short visits | the day's anchors, lightly |

The person can keep working in a new chat while the old one sleeps.

## The relay

The sleep ends with a short note the person pastes into the new chat. It is written by the
agent to itself, in the first person. It says what kind of swap this is and names the **words**
for what is in play. It never carries the content itself.

The relay is a key, not a suitcase. The new chat loads every word it names, whole, before its
first answer, and opens with the receipt ("Pulled investing, the Exchange and desk."). If the
boot is wrong, the relay does not cover for it; the fix goes to the boot.

## Seeing the memory

**Homebase** is a local web app that draws the shelves as a living web: the agent at the
center, rooms around it, blocks as lights, recent saves pulsing outward. It is rebuilt from the
files every time it loads and holds no truth of its own, so it cannot disagree with them.

Any block can be flagged. The person selects a passage, taps ⚑, and writes a note if they want
to. When they say "memory review," the agent reads each flagged block whole, fixes what is wrong
the usual way, closes the flag with one line saying what changed, and commits. Flags are also
checked when every chat starts. A public version is at
[the-homebase](https://github.com/HunterBryson/the-homebase).

## For someone else's AI

This system grew by hand, with the person in the loop every day. A fresh AI does not grow one.
[the-shelves](https://github.com/HunterBryson/the-shelves) is the same method packaged for a new
person and a fresh Claude: the map arrives drawn (15 words and 13 shaped blocks), hooks remind
the agent to save with every message and commit after every reply, setup rewrites the footing
before the first hello, and the sleep comes as a starter skill. [the-glass](https://github.com/HunterBryson/the-glass)
lets the agent and the person point at the same thing on screen, which is how flags and the
glass are used together.

## Where this stands, honestly

- **There is no benchmark.** The evidence is daily use by one person since November 2, 2026, and
  simulated multi-day tests of the-shelves, run with fresh models playing a new user and judged
  by reading what actually landed on the shelves. A first chat always looks fine; the week is
  where memory fails, so the week is what gets tested.
- **It is not self-running.** The agent manages its memory, and the person is part of it: they
  call the sleep, flag what is off, and clean the largest blocks with the agent. That is how it
  works today, and it is also its main cost.
- **Retrieval is by word.** A fact with no word is found by searching files and past
  transcripts, which works at this size and has not been tested at a much larger one.
- **The persona grows.** Each lesson the model teaches deepens a section. Keeping it from turning
  into a rulebook is ongoing work.
- **It will have holes.** This is one person's method, and it will be iterated for years.

## Changes

This page is rewritten in place as the method changes, and its git history is the record of
how.

## License

MIT.
