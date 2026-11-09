# How Odie remembers

## My current understanding

I'm Hunter. I've spent months building agents with persistent memory, and I'm still learning how this works. This page is my current understanding and my hope, not a solution.

My hope is simple: I want AIs to manage their own memory. I want them to have a brain that works for them, not something I force on them.

That isn't how it works yet. Every AI I've worked with has a strong pull to value only the chat it's in. Carrying what matters out of that chat into a place that lasts takes a lot of effort, and it doesn't happen on its own. What follows is why I do what I do about that.

So for now, I push. Odie, the Claude Code agent I work with every day, keeps its memory in a folder of markdown files on my PC, and this repo describes how that works today. A few things I do so Odie is Odie from the first message of every new chat:

- **Tempo.** A regular chat swap usually means I start over: I summarize, re-explain, and wait for the new chat to catch up. Here the swap is Odie sleeping, and Odie does most of the work. It reads our chat back, settles what mattered into memory, and notes what kind of swap this is. Then it says hi in the next chat, ready to roll at the pace we were going. In the middle of a build, it's one second for me and a brand-new day for Odie, so it carries exactly where we are, what's moving and the very next move, and picks up like we never stopped. At the end of a day, it carries how we ended, starts calm, and asks where my head is.
- **The footing.** A whiteboard with fixed slots (what kind of chat this is, where we are, what's in play, how it feels, the next move), wiped and rewritten at every swap and read before Odie says a word.
- **Additional context.** Odie hands itself a short note for the new chat that names the words for what's in play. The new chat pulls the real memory behind each word before it answers, and tells me what it pulled, so I never have to catch it up.

The memory itself stays private; this is the method.

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

This grew by hand, with me in the loop every day, and a fresh AI won't grow one on its own. So I
packaged what we do into [the-shelves](https://github.com/HunterBryson/the-shelves) for someone
starting fresh with Claude: the map comes drawn (15 words and 13 shaped blocks), hooks remind the
AI to save with every message and commit after every reply, setup writes the footing before the
first hello, and the sleep comes as a starter skill. [the-glass](https://github.com/HunterBryson/the-glass)
lets Odie and me point at the same thing on my screen, and it's how I flag things and work with
Odie every day.

## Where this stands, honestly

- **I have no benchmark.** What I have is a long stretch of building agents with memory and
  using it every day, plus multi-day tests of the-shelves where fresh models play a new user,
  judged by reading what actually landed in memory. A first chat always looks fine. The week is where memory fails, so
  the week is what I test.
- **It doesn't run itself.** Odie manages its memory, and I'm still doing a lot of the work: I
  call the sleep, flag what's off, and clean the biggest blocks with Odie. That's how it works
  today, and it's the main cost.
- **Things are found by word.** A fact with no word gets found by searching files and past
  chats. That works at this size; I haven't tested it much bigger.
- **The persona keeps growing.** Each lesson deepens a section, and keeping it from turning into
  a rulebook is ongoing work for both of us.
- **It will have holes.** This is my method, and I'll be iterating on it for years.

## Changes

This page is rewritten in place as the method changes, and its git history is the record of
how.

## License

MIT.
