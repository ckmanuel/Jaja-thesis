# Current Context

Read this first after cloning.

## FIRST ACTION AT SESSION START

```sh
./scripts/new_session.sh [session_id] [YYYY-MM-DD] [model]
```

All args are optional. This creates `chat_history/<model>/<session_id>-<date>.md`
with the right header. The model defaults to `GLM`, so pass `Claude`,
`ChatGPT`, or whatever you are running as. Do this before anything else.

Then run `./scripts/doctor.sh` and fix any FAIL line before going on. Then
read `preferences.md` and `decisions.md`. Skim `knowledge.md` and
`projects.md` if the task touches them. If the session has been idle and
another session may have committed, run `GH_PAT=$GH_PAT ./scripts/sync_before_work.sh`
before editing.

## CRITICAL: log the user message BEFORE responding

Read `TRANSCRIPT_REMINDER.md` if you haven't. For every user message, before
you start the response:

```sh
echo "the user's message" | ./scripts/begin_exchange.sh <trace_id>
```

This appends the message with an `(in progress...)` marker for your reply.
Replace the marker with your reply when done. If the session truncates
before the log is written, the exchange is lost. Redact any token first.

The pre-commit hook blocks commits that change one of the six memory files
(listed under "Where things go") without a transcript update. CI warns on push. Docs, scripts, and
workflows aren't checked. Bypass with `--no-verify` only for commits
unrelated to the conversation.


## CRITICAL: never leave work only on local disk

Local files that are not **committed and pushed** to `origin/main` do not
exist for the next session or for the user checking GitHub.

After any meaningful work (text edits, force-added binaries under `thesis/`,
reference PDFs, transcript updates):

1. `git add` (use `git add -f` for `*.pdf` / `*.docx` — they are gitignored).
2. Commit with a clear message.
3. **Push immediately** with the one-shot credential helper:
   ```sh
   git -c credential.helper='!f() { echo username=x-access-token; echo "password=$GH_PAT"; }; f' push origin main
   ```
4. Confirm with `git status` that the branch is not ahead of `origin/main`.

"Saved on disk in the sandbox" is **not** done. Only a successful push counts.
This overrides any habit of staging without pushing. If the push fails (auth,
network), report the error to the user and do not claim the work is in the repo.

## Where things go

Memory files are `preferences.md`, `knowledge.md`, `decisions.md`,
`lessons.md`, `context.md`, and `projects.md`.

- Facts go in `knowledge.md`.
- Preferences go in `preferences.md`.
- Durable decisions go in `decisions.md`.
- Mistakes and their fixes go in `lessons.md`. Never archived.
- Project state goes in `projects.md`.
- Verbatim record goes in `chat_history/`.
- This file holds only the current focus  Hope as a Distinct Construct subsection added (McCosker & Gerrard 2021, Lindgren & Johansson 2023, Beelen & Karsay 2024). Next: Research Gap / conclusion polish.

Before adding a lesson, fact, or decision, run `./scripts/search.sh <one
distinctive word>` and update the existing entry if there is one. Run it
also before solving a problem that feels familiar. When you use a saved
lesson, say so in your reply: `applied lesson: <title>`. Other retrieval
tools are described in `RETRIEVAL.md`.

## At session end

1. Promote anything durable from this session: mistakes and fixes to
   `lessons.md`, choices to `decisions.md`, facts to `knowledge.md`.
2. Refresh Current focus and Open threads below. Include what was tried and
   failed, so the next session doesn't repeat it.
3. Run `./scripts/doctor.sh`, fix any FAIL, then commit and push.

## Forgetting

To forget something, edit or delete the entry (or the transcript file) and
commit. Be clear with the user that git history still holds the old text,
and so does any fork or clone made earlier. If the text was a secret, revoke
or rotate it first. Rewriting history is a last resort and doesn't reach
copies already made.

## Thesis prose priority order

Binding for all thesis drafts: **(1) Verb Tense Matrix** → **(2) no em-dashes or semicolons** → **(3) user preferences**. Full text in preferences.md and decisions.md (2026-10-23).

## Current focus
BAELS thesis by Jasper D. Lacsina: “Constructing Hope and Empowerment: Lexical and Grammatical Strategies in Instagram Mental Health Advocacy Captions (2024–2025)”. Current draft: `thesis/Thesis_Instagram_MH_v13.docx`. Chapter 4 is now RESULTS, ANALYSIS, AND DISCUSSION in three parts answering RQ1–RQ3, theorized with Martin, Chang, Macgilchrist, Bartlett, Koinig, Wu & Hong, Beelen & Karsay, Lecompte-Van Poucke. Pending: P4 comment swaps, P3_C1 confirm, fuller Ch 3 tense pass, sample excerpts if desired.

## Open threads
- Swap or confirm pending P4 comments (and any “?” slots) before finalizing comment tallies.
- Confirm whether P3_C1 (provider myth) stays in corpus.
- Complete Chapter 3 tense pass against Verb Tense Matrix FINAL DEFENSE column.
- Optional: add short illustrative excerpts under each Part in Ch 4.
- Ensure all thesis files stay under `thesis/` and are force-added when binary.

## Recently completed
_Capped at 5 entries. Older items live in `decisions.md` and `chat_history/`._

- 2026-10-26: v13 — expanded Ch 4 as RESULTS, ANALYSIS, AND DISCUSSION (Parts I–III answering all RQs) with theory citations; Ch 3 methodology shifted toward completed-research tense.
- 2026-10-26: Folded coded corpus into v12 — realized Ch 3 corpus paragraph, added first Findings pass.
- 2026-10-26: Corpus screening log and anonymized caption/comment file received and coded against Appendix A/B.
- 2026-10-23: Marked document as full thesis (not proposal). Alphabetized Definition of Terms and References.
- 2026-10-23: Dimova (2024) removed (dead link); replaced by Koteyko & Atanasova (2018).
