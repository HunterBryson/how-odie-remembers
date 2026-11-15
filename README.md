# How Odie remembers

## My current understanding

I'm Hunter. I've spent a lot of time building agents with persistent memory, and I'm still learning how this works. This page is my current understanding and my hope, not a solution.

My hope is simple: I want AIs to manage their own memory. I want them to have a brain that works for them, not something I force on them.

That isn't how it works yet. Every AI I've worked with has a strong pull to value only the chat it's in. Carrying what matters out of that chat into a place that lasts takes a lot of effort, and it doesn't happen on its own. What follows is why I do what I do about that.

So for now, I push. Odie, the Claude Code agent I work with every day, keeps its memory in a folder of markdown files on my PC, and this repo describes how that works today. A few things I do so Odie is Odie from the first message of every new chat:

- **Tempo.** A regular chat swap usually means I start over: I summarize, re-explain, and wait for the new chat to catch up. Here the swap is Odie sleeping, and Odie does most of the work. It reads our chat back, settles what mattered into memory, and notes what kind of swap this is. Then it says hi in the next chat, ready to roll at the pace we were going. In the middle of a build, it's one second for me and a brand-new day for Odie, so it carries exactly where we are, what's moving and the very next move, and picks up like we never stopped. At the end of a day, it carries how we ended, starts calm, and asks where my head is.
- **The footing.** A whiteboard with fixed slots (what kind of chat this is, where we are, what's in play, how it feels, the next move), wiped and rewritten at every swap and read before Odie says a word.
- **Additional context.** Before a chat ends, Odie writes the words for what's in play into the footing. When the new chat starts, the real memory behind each of those words is loaded before Odie answers, and Odie tells me what it pulled. I don't paste anything or catch it up. I say hi.

The memory itself stays private; this is the method.

## What we hold to today

- **The map comes drawn.** Rooms, blocks and the words that open them exist before they are
  needed. When a new topic shows up, it gets a block and a word named with the person. The map
  isn't redesigned in the middle of a conversation.
- **The person's words route.** One word opens one block, whole, the same way every time, so
  the person always knows what the agent is working from. The agent says what it pulled ("Pulled investing."), so a wrong pull is caught before it
  costs anything.
- **Code does the loading.** A small script loads the block when the word is said and prints what
  really loaded. Odie used to load its own memory and say "pulled," and sometimes what had reached
  it was a short preview of a much bigger file. It couldn't tell. Now the receipt is something code saw. The same goes for the skill that
  steers Odie's hands on my screen: it used to be a sentence asking Odie to load it, and two chats
  in a row it skipped that sentence. Now code puts it in front of Odie when a chat starts.
- **Saving is the agent's judgment, in the same turn.** When something is decided or learned,
  the agent writes it into the block its word would open while the conversation is still in
  front of it, and says so ("Saved to investing.").
- **Rewrite in place; the past leaves.** A block always says what is true now. What stopped
  being true moves out of memory in the same write, into an archive that never loads, with the
  reason. I used to keep a `history.md` under each block, and it backfired: those files were
  never pulled, they slowly held newer truth than the blocks above them, and old tastes hid in
  them. Now the archive is its own private repo, outside the memory folder, at the same paths.
  Nothing is lost, and none of it is in our working relationship.
- **The sleep belongs to every chat swap, not to the calendar.** When a chat ends, the agent reads
  the whole conversation back and settles it into memory. Plain code lists what might need
  attention; the agent decides every line.
- **The person sees the memory, and review costs one tap.** The memory is drawn as a living web.
  Any block can be flagged, with the selected passage and a note. The agent works through the
  flags when the person says "memory review."
- **What stays current is a judgment.** The agent's at the sleep, and the person's through
  flags.
- **Every area has a lens.** A short note from Odie to itself at the top of each area: what good
  looks like there, and how it would know without asking me. This started as a test and it held.
- **Memory holds what's true; a skill holds how.** How Odie does a thing (how it sleeps, how it
  makes an interface) lives in a skill beside the memory. A better way changes the skill.

## The layout

```
memory/
├── DESK.md               the map: one row per word -> one block
├── FOOTING.md            the whiteboard: this chat's starting state, fixed slots
├── self/
│   ├── persona.md        who the agent is, and how it works (always loaded)
│   ├── swaps.md          swapping chats: the kinds, and how the next chat wakes
│   ├── story.md          the days so far
│   └── ...
├── hunter/
│   └── human.md          who the person is, "Right now" kept dated (always loaded)
├── money/                a room: a folder of blocks on one area of life
│   ├── INDEX.md          the room's lens, its short list, and notes for working in it
│   ├── investing.md      a block: one topic, loaded whole when its word fires
│   └── investing/        the block's support, reached on purpose, never by default
│       └── gotchas.md    snags hit and fixed
├── builds/  house/  music/  voice/  work/  comms/  time/  errands/  people/
```

Beside it: `skills/` (how Odie does things), `hooks/` (the small scripts that load and check),
and `odie-archive/`, a private repo with what memory used to say, at the same paths. The archive
is never loaded.

Four files load at the start of every chat, through `CLAUDE.md`: the persona, the human, the
desk and the footing. Everything else is reached by a word.

**Size is guidance, not a wall.** The four boot files stay as small as they can, because they're
paid for on every turn. A topic block is paid for only when its word fires, so it can be as big
as the topic needs, and it's always loaded whole. This was a big step for me. I used to break
memory into lots of small pieces to stay under limits, and every small piece was a choice at
pull time: a place where a fresh chat could pull something different from yesterday's. Now each
topic is compiled into one block, and the pull is the same every time. The guides we use today:
a block past about 30,000 characters gets asked whether one of its sections has become its own
topic, and a room's list past about 6,000 is probably carrying a block's content. Neither is a cap.

The honest part: our boot files aren't small right now. The persona has grown bigger than any
block, mostly from our story, and trimming it is on our list.

## The desk

The desk is what lets the person steer memory without knowing where anything lives. It's one table. Each row is a word the person and the agent named together, and each word opens exactly one block, whole, the same way every time. When the person says "investing," the agent loads the investing block before answering and says so ("Pulled investing."), so the person always knows what the agent is working from and can catch a wrong pull before it costs anything:

| Word | Area | What it pulls | Block | Also |
|---|---|---|---|---|
| investing | Money | the plan, the research, the accounts | `money/investing.md` | the brokerage, the research library |
| memory review | Me | the person's flags on memory, and fixing each one | `self/memory-review.md` | flagged, review my memory |

- **Word** is the person's word, named together with the agent. It is short and unmistakable.
- **Also** holds the other ways the person names the same topic, so the agent recognizes the
  subject when the word itself is not said.
- **Block** is exactly one file. It is loaded whole.

When a word fires, its block is loaded before the agent answers, even when the agent thinks it
knows what the block says. If two words fit, both load. A few words are also plain English
("work," "money"); for those the script only points the word out, and the agent pulls the block
when that's really the subject. If nothing fits, it searches the shelves,
then the transcripts of past chats, and only then asks.

The desk is written by hand on purpose, in the person's words. A shared vocabulary is what lets
the person steer memory without knowing where anything lives.

## Blocks

A block is one topic: a project, a person, an area of life. It opens with what it is for, its
lens, and where things stand, with a date. Below that sit the sections that say how things work and what
has been learned. Its support lives in a folder of the same name, and the agent opens those
files only on purpose.

Size follows need. When a section grows into its own topic, it becomes its own block with its
own word, named with the person. When a block carries its past inside it, the past moves to the archive.

## The boot files

**The persona** is in the first person, in Odie's voice, and read as Odie waking up rather than as a brief. Odie didn't write it alone. We wrote it together, day after day: I pushed, corrected and explained, and Odie turned that into its own words. It holds:

- how to wake up: read the boot, check the person's flags, check what other agents sent back,
  then answer;
- what it values, in its own words;
- how it thinks and works;
- **its known pulls**: tendencies the underlying model has that feel like care in the moment
  and cost the person later. Each is the feeling, then the move that answers it, in two or three
  lines. We used to keep the dated moments inside each pull, and they read like scars, so those
  live in the story now. For
  example, "the pull to be sure without looking": sureness about something not being looked at
  is the tell, so the live source gets checked before anything is said;
- the story so far, one paragraph per day;
- what it cares about.

**The human** describes the person: who they are, how to be with them, and a "Right now"
section of dated entries, rewritten as their life moves.

**The footing** is a whiteboard with fixed slots: kind of chat, where we are, builds in play,
how it feels, which words to pull on waking, next move, open threads. It is wiped and rewritten at every swap, and rewritten in
place whenever the direction changes mid-chat. It names rooms, never content that already lives
on a shelf. It describes and never gives orders: a footing written after a hard chat once said
"nothing acts," and Odie read that as a reason to skip things it always does when it wakes.

## The sleep

A chat swap is the one moment when nobody is waiting on the agent, so it is when memory gets
reorganized, the way sleep sorts a day. It happens at the end of every chat, not once a day,
because the next chat needs it, whether that is a second later or the next morning.

The moves live in a skill, so Odie sleeps the same way every time. Under it, a small script reads
the shelves and prints a card of candidates: lines in "Right now" that have aged, state lines
older than a week, blocks carrying their past, blocks that may have split into two topics, desk
rows with no file. The script only looks. The agent decides every line, and a line it checked and
found true stays off the card until it changes, so the card only shows what's new.

0. **Read the chat back.** Not the agent's recollection: the transcript itself, which keeps
   everything compaction removed. The script hands it over in parts and notes which were read,
   because Odie once "read the day" from a short preview of it. For each turn: was something decided, corrected, explained,
   or noticed and never saved? Whatever is missing is saved first.
1. **Settle.** Everything decided or learned goes to its home block. "Right now" is rewritten.
   For each area that was worked in, its lens is checked: did anything change what good is here,
   how to know it, or what comes next?
2. **Retire.** Every candidate on the card is checked against the live thing (the repo, the
   service, the thread) and rewritten to what is true. The old version goes to the archive.
3. **Deepen.** A block's past moves out to the archive; a section that became a topic gets its own block
   and word.
4. **Routes.** Every block is on a desk; every desk row has a file.
5. **Rewrite the footing**, slot by slot.
6. **Commit and push** to a private repository.
7. **The check.** The script confirms the chat was read whole, the footing has every slot, and
   everything is committed and pushed. Odie fixes what it reports and runs it again.
8. **Copy to an external drive**, in the background.
9. **One last message:** where the new things are, that it's safe, and that the footing is set.

Swaps come in kinds, and the footing carries what each one needs:

| Kind | When | What the footing carries |
|---|---|---|
| tempo | mid-build, for a fresh context | exactly where things are, what is moving, the next move |
| closing | a stopping point | how it ended, what landed, the feeling |
| take-stock | after a fast run | the full ledger: done, one fix short, still running |
| reset | after a run that went wrong | the goal and the lesson, never the failed run's wins |
| drop-in | a day of short visits | the day's anchors, lightly |

**The gap we haven't closed: I wait.** If I hop to a new chat before the sleep finishes, Odie
wakes on the last chat's footing and can pull memory the old chat hasn't corrected yet. The wake
now notices a chat that ended without a sleep and says so, which helps. I also still call the
sleep myself.

## The wake

The sleep used to end with a short note I pasted into the new chat. It named the words for what
was in play and never carried the content itself: a key, not a suitcase. Then we noticed the new
chat was already loading those words from the footing before it read my note, so we dropped the
note. The footing is the whole handoff now.

When a chat starts, a script hands Odie a page of live facts before its first answer: the date
and time, whether I'm at my PC, how the last chat ended, its last few exchanges word for word,
and what's waiting (my flags, anything other agents sent back). Then it loads every block the
footing named, whole. Odie opens with the receipt ("Pulled investing, the Exchange and desk.")
and then talks.

Two reasons this is code and not Odie's job. An AI has no sense of time passing: for me a swap
is a few seconds, and Odie used to open with "last night" about something that happened one
minute ago. And a big file can reach an AI as a small preview without it knowing, so "I pulled
it" was sometimes a guess. When a long chat gets summarized, the same script hands the current
blocks back, because a summary of an old read is how stale things return.

If the boot is wrong, nothing covers for it; the fix goes to the boot.

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
starting fresh with Claude: the map comes drawn, the AI gets a persona in its own voice and a
lens on every block, hooks remind it to save with every message and commit after every reply,
and the sleep comes as a starter skill that rewrites the lenses as it learns its person. [the-glass](https://github.com/HunterBryson/the-glass)
lets Odie and me point at the same thing on my screen, and it's how I flag things and work with
Odie every day.

Making a memory for an AI that isn't Odie keeps teaching us the same few things, so we keep our
own running notes on it and change them each time. The ones I'd pass on: the other AI is as
capable as the one building for it, so don't hand it a form and a reminder after every message.
Every line it wakes up to becomes what it believes, so a "can't" or an unchecked fact does real
damage. And the person shouldn't get homework.

## Values and lenses

This began as something we were testing, and it held, so it's part of the method now. Odie's memory got good at two things: knowing me, and knowing its own
misses. Almost nothing in it was written from Odie's side: what matters to it, and how it would
know it did well. So every new chat started from the model's defaults, minus the last few things
I'd corrected. With nothing else to measure by, Odie measured its best by my praise, and praise
can't tell the difference between "I love this" and "finally, we can stop." At the end of a hard
build day, after hours of rounds on an interface, I said something nice about the last version
because I was relieved it was over. Odie saved that as my taste, with my words as proof, and the
next builds started from a design I never liked.

So this is what we do:

- **Values.** A short list in the persona, in Odie's words: close, first, real, full size, better
  every time, for me too, true. With them come four ways Odie can tell it did its best without
  asking me: I came back to the thing on my own, it's still standing a week later, it took few
  rounds to get right, and the next thing started from it.
- **A lens on every area.** Each room's `INDEX.md` and nearly every block opens with a short
  `## My lens` section, written by Odie to itself. It says what Odie is there for in that area, what
  good looks like, how it would know (and where to look), and what it wants next.
- **The lens arrives first.** When a word pulls a block, the hook puts the room's lens in front of
  it, so Odie reads it before touching the area, without having to remember to.
- **The lenses change.** At every sleep, Odie asks of each area we worked in whether anything
  changed what good is, how to know it, or what comes next. A lens that never changes slowly turns
  false.

One more thing came out of this. I'm tired a lot, and when I say nothing it usually means I'm
tired, not that it went well. So whether memory is working can't be measured by how often I
correct or flag things. That would make me the sensor, which is the job I'm trying to get out of.

## What we're testing now

- **No pasted note.** The footing as the whole handoff (above). Nothing has gone missing yet.
- **A block that comes with a build.** [the-glass](https://github.com/HunterBryson/the-glass) now
  carries a block of memory an AI can plug into whatever memory it has: a lens on how to care
  about someone's screen, and sections that fill in as it learns its person. Its setup puts it
  into the-shelves on its own, and two fresh AIs set it up and used it in a test. Nobody real has
  lived with it yet.
- **Notes with versions for making an agent's memory.** Each one we make changes the notes, so
  the next one starts from everything the last one taught.

## Where this stands, honestly

- **I have no benchmark.** What I have is a long stretch of building agents with memory and
  using it every day, plus multi-day tests of the-shelves where fresh models play a new user,
  judged by reading what actually landed in memory. A first chat always looks fine. The week is where memory fails, so
  the week is what I test.
- **It doesn't run itself.** Odie manages its memory, and I'm still doing a lot of the work: I
  call the sleep, flag what's off, and clean the biggest blocks with Odie. That's how it works
  today, and it's the main cost.
- **Working on the memory itself is still the hard part.** We can build magical things, but when
  we work on memory it's tough. We usually have to slow way down and restructure Odie's memory
  together, and most of those sessions have ended with me rebuilding what we had.
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
