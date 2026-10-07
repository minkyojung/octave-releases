# Getting started with Octave

Octave is a coding agent for the Mac. You say what to build; the agent — Claude Code — plans it with you if it
needs a plan, and builds it in a workspace of its own: a folder and a branch, which become a pull request.

**You need:** a Mac with Apple Silicon (M1 or later) on macOS 13 Ventura or later, and a Claude subscription
or an Anthropic API key.

## 1. Install

Download `Octave-<version>-arm64.dmg` from the [releases page](https://github.com/minkyojung/octave-releases/releases/latest),
open it, and drag Octave to Applications. It is signed and notarized, so it opens without a warning.

Octave updates itself. When a new version has been downloaded, a small notice in the bottom-right corner offers
to restart; if the agent is in the middle of something, *Restart when done* waits for it. Closing the notice is
fine — the update goes in the next time you quit. **Help › What's New** says what changed.

## 2. Add a repository

With no repository yet, Octave opens on a screen that adds one. **Open local repository** takes a repository
you have on this Mac. **Clone from GitHub** lists yours to pick from — signed in with `gh`, private ones too —
or takes any by owner/name or its address, and clones it into `~/octave/repos/`. Either way the repository is
added to your list and nothing else happens: no workspace is made or opened until you ask for one.

A workspace is a folder and a branch of its own, made from the latest on the remote, under
`~/octave/workspaces/`; the agent works there, and your own clone is never touched. The sidebar lists your
repositories and their workspaces — and so does the first screen, until a workspace is open: click a workspace
to move into it, + beside **Repositories** adds another repository, and `⌘O` opens a local one. Drag a
repository's row to put the list in the order you want it in. Right-click one and **Take off the list…** when
you are done with it: nothing on your disk is touched, and adding it again brings its workspaces back with it.

+ on a repository makes a workspace. Say what you want built, choose the model and effort for it, and **Create**
(`⌘↵`): the workspace is made, the window moves into it, and your words are sent there as your first message, to
the agent as it is.

One piece of work to a workspace: for the next one, press + again. How the work goes from there — deciding,
planning, building — is below, under *Plan, build*.

The dialog opens from anywhere with **Folder › New Workspace…** (`⌘⇧N`), over the repository you are in; the name at
its top changes to another. Behind the ⋯ beside it is the branch the workspace starts from — the remote's default
unless you choose another, for work that stands on work not merged yet. The button at the far end starts from
one of the repository's open GitHub issues, listed with `gh`: its number, title and words go into the box, for you
to read and change before Create.

When you are done with a workspace, right-click its row and **Archive workspace…**. Its folder is given back;
the branch and its commits stay, the conversation stays, and so does the row — under **Archived** at the foot of
the repository, where a click makes the folder again from the branch, sets it up and opens it. Changes you have
not committed would go with the folder, so you are told how many there are first.

**Plan, build.** Shift+Tab goes between two modes — anywhere in the window but a file, the terminal or a
dialog, where the key is their own — and the message box says which you are in. Plan is not a step you have to
take: a small request is built without one.

- **Plan** (the box filled grey, a map on its send button) is for seeing how the work will be done before any of it is. The agent reads,
  searches, and asks what it needs to know — a question takes the message box's place until you answer it —
  and then writes a plan. The plan is in the conversation, whole. **Approve plan**, over the message box, puts
  the agent to work on it; to have the plan changed, say what in the box instead. Nothing is changed.
- **Execution** (the box as it always is) is where a conversation starts, and where the agent changes files and runs commands.

The panel at the right is a row of tabs you open and close: **Changes**, the files changed and not committed —
a file there opens what changed in it; a **terminal** for each shell; and **Run**, the repository's commands
(below). Close one with its ×, and open one again, or another terminal, from the + at the end of the row.
Which are open, and which is in front, is kept for the workspace. `⌘\` hides the panel and shows it; `` ⌃` ``
brings a terminal to the front, and puts the panel away when one is in front already.

The plan is a file where Claude Code keeps plans, in `~/.claude/plans/`. It is not in the workspace and does not go with the branch, and Claude Code removes it after 30 days, as it does the
conversation.

What the agents in a workspace leave each other — notes, plans, what they found — goes in its `.context/`
folder, which every conversation there can read. It is not committed, and goes with the workspace when you
archive it.

To read a file as it is now, ⌘P finds any file in the repository and opens it in a tab — read-only, unless it is markdown — and from
there, in your own editor at the line you were reading.

**A repository can say how it is worked in.** Octave makes a fresh folder for every workspace, and a fresh folder has
only what is committed — no dependencies, no `.env`. Where the repository has no `.octave/config.toml`, the
**Run** tab says so; press **Set up with the agent**, and the agent drafts one from what is there: package files,
CI, the README. Read it and fix it; it is a short file, and yours. What goes in it:

- `copy`, the files kept beside the code and out of git — `.env*` unless you say otherwise — brought over
  from the repository's own folder into every new workspace first. A file the branch already has is left alone.
- `setup`, run in every new workspace as it opens — `npm ci`, say. The **setup** row in the Run tab says it
  is going, and what you typed reaches the agent once it is done. Fails, and your line goes to the message
  box instead, with why; the row is marked failed, with *Send to agent* and ▶ to run it again.
- `[[scripts.check]]`, one per check — `npm test` and the rest, what you would run before a pull request.
  Each is a row under **Checks** in the Run tab, with **Run checks** at the head to run them all in order and ▶
  on each to run that one; the row's mark says whether it passed, and one that failed offers *Send to agent*.
- `[scripts.run.dev]`, a dev server or a watcher, on a port of the workspace's own in `OCTAVE_PORT`: a row
  under **Run** in the Run tab, one per run the file names, with ▶ to start it and ■ to stop it (one at a
  time in a workspace) and its `localhost` link while it runs. Under the rows, a line
  says how the run stands — Running with its port, or Exited with its code — then what it prints, as it
  prints it, and **Send to agent** puts that in the message box. With the panel folded, the run's state is at
  the end of the tabs.
- `archive`, run just before a workspace is removed.

What each command printed is a file in the workspace's `.octave/local/runs/` folder; pressing the command's
row in the Run tab opens it in a tab like any file, at its end, following it while it runs. Octave runs these commands and understands none of them, so any language
and any tool is fine; it only reads the exit code.

## 3. Sign in

Octave uses Claude Code's own sign-in. If you use `claude` in a terminal, you are signed in already. If not,
open Settings (`⌘,`) › Accounts and press **Sign in**: a terminal opens beside the conversation with Claude Code's
sign-in running in it, and your browser opens to sign in to your Claude account. If the browser shows a code
instead of coming back by itself, paste it into that terminal. Claude Code need not be installed on its own:
Octave fetches it. Accounts says which account the agent runs on. An Anthropic API key in your environment is
used before a subscription, and is billed by use — Accounts says so when it is. You are signing in to
Anthropic, not to us: Octave has no account of its own.

## 4. Ask

- The conversation is a tab in the middle of the window, in a row with the files you open. Type `@` in the
  message box to name a file and `/` for Claude Code's commands.
- Choose lines in a file and press `⌥K`: they go in the message box as that place, `@file#5-10`, and the agent
  reads those lines.
- **PDFs and pictures.** Drop one on the message box, or type `@` and pick one in the workspace, and ask: the
  agent reads it. Words chosen in a PDF go in the box with `⌥K` too. A PDF longer than ten pages is read a few
  pages at a time, which needs poppler installed (`brew install poppler`).
- `↵` starts a new line in the message box, and `⌘↵` sends. A message sent while the agent is working waits
  over the box, and is read as soon as the step it is on is done; until then it can be taken back.
- Point at a message of yours for two buttons: **Rewind** goes back to before it — the files the agent edited
  (not what its commands changed), the conversation, or both — and **Fork** goes on from there in a new conversation, this one left as it was.
- A file opens in a tab. A markdown file can be written in, and saves on its own; `⌘E` goes between reading it
  and writing in it. Any other file is read-only.

## 5. What the agent may do

There are two modes, on Shift+Tab. *Execution*, where a new conversation starts, lets the
agent change files, run commands, and search and open the web. *Plan* lets it read, search the code and run
commands that only look, and nothing else: it writes the plan, which is no change to your code.

Octave does not ask before each step, so the mode is the boundary. Drop to *Plan* when you only want to talk.

## 6. What leaves your Mac

When you ask the agent something, your message and whatever it reads to answer — which can be any file in the
workspace — go to Anthropic. Claude Code also sends its own usage metrics, to Anthropic and a logging
service, and on a Pro or Max sign-in reports of its own errors, to an error-tracking service; Anthropic says
neither includes your code or prompts. Octave itself sends nothing about you. Octave checks GitHub for a new
version when it starts and every four hours. The full table, where every file is kept, and how to remove it
all: [PRIVACY.md](PRIVACY.md).

## 7. Coming from Obsidian

Open your vault as it is; Obsidian can stay open on the same folder. What is drawn the way Obsidian draws it:

| | |
|---|---|
| Links, tags, properties | `[[note]]`, `[[note#heading]]`, `#tag`, the properties at the top of a note |
| Pictures | `![[photo.png]]`, `![[photo.png\|300]]`, `![alt](path.png)`. Paste or drop a picture and it is saved where your vault's attachment setting says — `attachments/` if it says nothing |
| Embedded notes | `![[Note]]`, `![[Note#Heading]]`, `![[Note#^block]]`, kept current as the other note changes |
| Tables | Drawn as tables. Inside one, `Tab` and `⇧Tab` move between cells, `↵` adds a row, and the columns are squared up when you leave |
| PDFs | `[[paper.pdf]]` opens it, `[[paper.pdf#page=3]]` at that page |
| Footnotes | `[^1]` and its note; type `[^` to pick one or start a new one, point at a number to read it |
| Math | `$x^2$` and `$$` blocks |
| The rest | Callouts, highlights, comments, task lists, `H~2~O`, `x^2^`, and a short list of HTML (`<u>`, `<kbd>`, `<details>` …) |

As in Obsidian, the markup comes back when the cursor is in it. Not here yet: a list of the notes that link to
the one you are in, plugins, and canvas.

## 8. Keys

| | |
|---|---|
| `⌘⇧N` | New workspace |
| `⌘O` | Open a repository |
| `⌘P` | Open a file by name |
| `⌘F` | Find and replace in the file |
| `⌘E` | Read a markdown file, or write in it |
| `⌥K` | Put what is chosen in the file in the message box |
| `⌘↵` in the message box | Send |
| Shift+Tab | Plan, or Execution |
| `⌃⌘1`…`9` | Run the conversation on the 1st…9th model of the model menu |
| `⌘⇧/` | The next effort for the conversation's model |
| `⌘\` | Show or hide the side panel |
| `` ⌃` `` | A terminal, or the panel put away when one is in front |
| `⌘,` | Settings |

## 9. When something goes wrong

**Help › Report a Problem…** opens the folder Octave keeps its log in and a form on GitHub with your versions
filled in. The app sends nothing itself — the log is yours to read over and drag in.
