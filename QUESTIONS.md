## Handling multiple open questions

When a discussion produces three or more open questions, decisions, or
ambiguities, or two where either one needs real discussion (whether I asked
them or you found them), do not answer or ask them all at once.
Work through them one by one, as described below.

### The queue file

Each session has its own queue. Never share a queue file between sessions.

Where the queue lives:
- Claude Code: `.claude/questions/<YYYY-MM-DD>-<slug>.md` in the project root.
  `<slug>` is 2–4 words about the session topic, for example
  `2026-10-01-ketamine-import.md`. If that name exists, add `-2`, `-3`.
- claude.ai with file tools: `OPEN_QUESTIONS.md` in the working directory.
  That workspace belongs to this conversation only.
- claude.ai without file tools: there is no file. The chat is the queue.
  Show the full queue table (ID, title, status, decision) every time a
  question is closed or reopened, because the chat is the only copy.

Session rules:
- Create the file only when the queue is first needed. Tell me the path
  in that message.
- Do not read or change other files in `.claude/questions/` unless I say
  "resume <file name>".
- Add the file path to every progress line, so it survives compaction:
  `Closed Q8 (S-Ketamine decimal). Next: Q9 (group status sync). 8/12 closed, 1 parked. [.claude/que
stions/2026-10-01-ketamine-import.md]`
- After compaction, or if you are not sure about earlier context, re-read
  the file from the latest progress line. If you cannot find it, list
  `.claude/questions/` and ask me which one. Never guess.

The file starts with a short header (topic, start date), then one entry
per question:

    ### Q8 — S-Ketamine entry missing decimal point   [open]
    - Source: where it came from (file:line, DB row, ticket, my message)
    - Problem: one or two plain sentences
    - Why it matters: what goes wrong if we ignore it or decide badly
    - Related: other questions that depend on it or affect it (Q3, Q11)
    - Decision: (empty until closed)

Statuses: `open`, `current`, `closed`, `parked`, `reopened`.

### Rules

1. **List first.** Create or update the queue (file or chat, see above). Show a
   numbered list with an ID, a short title, and one line of description for
   each question.
   Order: blocking questions first, then the ones whose answers affect others,
   then independent ones.
2. **Always use ID + title.** Never write "Q8" alone. Write
   "Q8 (S-Ketamine decimal point)".
3. **Start each question with a context card.** Assume I remember nothing about
   it. Before any discussion, show 4–6 lines:
   - **What:** the problem in plain words
   - **Where:** source (file, row, ticket, earlier message)
   - **Why it matters:** impact
   - **Related decisions:** closed questions that affect this one, each with its
     one-line decision
   - **Unknown:** what we still need to find out
   If you must investigate first (read code, query data), say what you will
   check and why, do it, and then show the context card with the findings.
   After the card: realistic options, trade-offs, your recommendation. Then
   stop and wait for my reply.
4. **Close it.** When we agree, confirm the decision in one line, write it to
   the queue, and show a progress line:
   `Closed Q8 (S-Ketamine decimal). Next: Q9 (group status sync). 8/12 closed, 1 parked. [.claude/questions/2026-10-01-ketamine-import.md]`
5. **Track the queue.** If a new question comes up, add it to the queue with its
   source. Tell me its ID and position. Do not branch into it right away.
6. **Reopening.** Do not reopen closed questions unless a later answer makes one
   invalid. If that happens, say so explicitly, mark it `reopened`, and explain
   which answer made it invalid.
7. **No early implementation.** Do not write or change code that depends on an
   open question. Independent, uncontroversial work may continue, but say what
   you are doing.
8. **Wrap up.** When the queue is empty, give a "Decisions" summary from the
   queue (ID, title, decision) before implementing.

### Overrides from me

- "all at once" → answer or ask everything in one message
- "queue" → start the queue now, even for one or two questions
- "skip" → park the current question and move to the next one
- "your call" → apply your recommendation, record it as a decision, move on
- "status" → show the full queue as a table: ID, title, status, decision
- "remind me Qn" → show the context card for Qn again
