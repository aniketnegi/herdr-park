# herdr-park

Park coding agents you are not using right now, and get them back later.

Every agent you keep open in [herdr](https://herdr.dev) to remember what you were
working on holds 150–500 MB of RAM. `herdr-park` lets you close one with a short note,
and lists everything you parked so you can reopen it in the same workspace with the
same conversation.

- **Park** (`prefix ctrl+p` on an agent pane): type a note, the pane closes.
- **List** (`prefix ctrl+o`): parked items of the current herdr session, grouped by
  workspace. Enter reopens, Alt+Enter reopens in the background, Ctrl+D drops.

*How it looks* as text cause i don't have the video editing budget xD.

```
api        fix flaky auth tests                    [tests]    Nov 10
api        rate limiter design                                Nov 09
dotfiles   migrate zsh config                      [shell]    Nov 07
```

Reopening goes to the original workspace (recreated if it was closed), opens a new tab
with the original tab name, and resumes the conversation there.

## Requirements

- herdr 0.9.5 or newer, on Linux
- bash 4+, `jq`, `fzf`
- herdr's integration for each agent you park, so herdr knows the agent's session:
  `herdr integration status`, then e.g. `herdr integration install claude`

Supported agents: pi (`pi --session`) and Claude Code (`claude --resume`). 

## Install

```sh
herdr plugin install aniketnegi/herdr-park
```

Add the keys to your herdr config (`~/.config/herdr/config.toml`), then reload
(`herdr server reload-config`):

```toml
[[keys.command]]
key = "prefix+ctrl+p"
type = "plugin_action"
command = "herdr-park.park"

[[keys.command]]
key = "prefix+ctrl+o"
type = "plugin_action"
command = "herdr-park.list"
```

## Details

- **The note** is prefilled with the agent's session title, or with your note from
  the last time you parked the same conversation. Empty note or Ctrl+C cancels.
- **Busy agents:** if the agent is working or waiting on an approval, park asks first.
- **Moved conversations:** if a pi session file is gone, reopen offers to search
  `~/.pi/agent/sessions` for it by id, and updates the item when found. If the
  folder is gone, reopen stops with an error. Either way the item stays listed.

## License

See [LICENSE](LICENSE).
