# What leaves your Mac, and where things are kept

This is about the Octave app. Octave has no account, no server of ours, and sends nothing about you or how you
use it. There is no telemetry and no crash reporting of ours.

The agent is Claude Code, Anthropic's, which Octave runs on your Mac with your own Claude sign-in. What it sends
is Anthropic's to describe — [Claude Code's data usage](https://code.claude.com/docs/en/data-usage) — and the
table below says what that comes to here.

## What leaves this machine

| When | Where | What |
|---|---|---|
| You send the agent a message | Anthropic, through Claude Code | Your message, the conversation so far, and whatever the agent read to answer — which can be any file in the workspace |
| The agent reads a PDF | Anthropic, through Claude Code | The pages it read. Claude Code reads the PDF itself and sends those pages; no conversion service is used. A PDF over ten pages is turned into pictures of its pages on this Mac first, by poppler's `pdftoppm`, which you install |
| The agent searches the web | Anthropic, which runs the search | The search words |
| The agent opens a web page | Anthropic, then the site | The site's name, which Anthropic checks against a list of unsafe sites; then a request for the page, from your Mac. What the page says goes to Anthropic like anything else the agent reads |
| Claude Code runs | Anthropic, and the services Anthropic names | Its own requests: usage metrics, which Anthropic says never include your code, prompts or file paths; on a Pro or Max sign-in, reports of its own errors; and how much of your plan's limits is used, for the window to show |
| Anything you set up for Claude Code yourself runs — MCP servers, plugins, hooks | Wherever they send it | Octave starts Claude Code with your own settings, so they run here as they run in a terminal |
| A file shows a picture from the web | The site the picture is on | A request for that picture |
| Octave starts, and every four hours after | GitHub Releases (`github.com/minkyojung/octave-releases`) | A request for the latest version number; then the update itself, if there is one |
| You add a repository from GitHub | GitHub — through `gh` if you are signed in to it, else git | A request for the list of your repositories, and the clone of the one you choose |
| A workspace is made | The repository's own remote (`origin`), and GitHub through `gh` | A fetch of its latest commits, and a request for your GitHub user name, which begins the workspace's branch name |
| You choose Help › Report a Problem… | GitHub, in your browser | Only what you type and attach yourself. The app sends nothing |
| Octave runs for the first time on this Mac, and the first time after an update that changes it | The npm registry (`registry.npmjs.org`), where Anthropic publishes Claude Code | A request for Claude Code's binary, the version the app was built against. The app does not carry it: it is kept in `~/.octave/claude/`, checked against the registry's checksum and Anthropic's signature, and run as published |

Signing up on octave.run is a separate thing from the app: the website keeps your GitHub profile and email
(see [its privacy policy](https://www.octave.run/privacy)); the app never asks who you are and the two are not
connected.

## Where things are kept

| Where | What | Made by |
|---|---|---|
| `~/octave/repos/` | The repositories you cloned from GitHub | Octave, with git |
| `~/octave/workspaces/<repository>/<city>/` | Each workspace: a git worktree of its repository, on a branch of its own. What the agent writes goes here, never into your own clone | Octave, with git |
| `<workspace>/attachments/`, or where your vault's attachment setting says | Pictures pasted or dropped into a markdown file. Part of the workspace, committed like any file | you |
| `<workspace>/.octave/attachments/` | What you pasted or dropped on the message box, one folder for each. Kept out of git | Octave |
| `<workspace>/.context/` | What the agents in a workspace leave each other. Kept out of git | the agent |
| `<workspace>/.octave/local/runs/` | What the repository's commands printed — setup, the dev server, the checks — one log each | Octave |
| `<workspace>/.octave/local/properties.json` | The property types you chose | Octave |
| `~/.claude/`, `~/.claude.json` | Claude Code's own: its settings, and the conversations, the plans the agent wrote and the copies of files it edited, which Claude Code removes after 30 days (`cleanupPeriodDays`). Shared with `claude` in a terminal if you use it | Claude Code |
| The macOS Keychain | Your Claude sign-in | Claude Code |
| `~/.octave/` | Octave's settings — your models, the conversations of each workspace — its log (`logs/server.log`), and Claude Code's binary (`claude/`), one folder a version | Octave |
| `~/Library/Application Support/Octave/` | Your repositories and their workspaces, the one you were in last, each workspace's tabs, and which version's What's New you have seen | Octave |

## Removing it

Dragging Octave to the Trash removes the app and nothing else. To remove the rest: `~/.octave/` and
`~/Library/Application Support/Octave/` are Octave's alone. `~/.claude/`, `~/.claude.json` and the sign-in are
Claude Code's — leave them if you use `claude` in a terminal. `.octave/local/` inside a workspace holds only logs and the property types you chose; delete it and nothing of yours is lost. `~/octave/` holds your clones and workspaces, which
are git's: a workspace's branch may hold work that was never pushed, so look before deleting one, and after
deleting its folder run `git worktree prune` in its repository.
