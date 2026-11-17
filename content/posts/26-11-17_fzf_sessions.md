---
title: "fzf-sessions"
date: 2026-11-17T10:34:11+02:00
draft: false
---

With the advances in agentic development, I've spent a lot of my time working with agents across dozens of sessions. Initially with [tmux](https://tmux.app/), but for the past couple of months I've been very much in <3 with [herdr](https://herdr.dev/tui) in [Ghostty](https://ghostty.org/).

One main issue remained: I usually start new sessions straight from a repo. I'm not that big a fan of orchestrated agent swarms because, as a human, I only have so much capacity to keep an eye on my herd of agents. But sometimes I stray from that plan: I start a session somewhere else, make a change here, raise a PR there and close the session again.

A couple of days later, I'm trying to remember where my session was. Claude and Codex come with the `/resume` command, but usually I cannot find that exact session anymore. Getting back to the sessions I'm looking for is cumbersome.

Long story short: **I released my first herdr plugin two days ago:** [fzf-sessions](https://github.com/christianblessing-me/fzf-sessions). A small Python-based TUI that uses fzf to find all Codex and Claude Code sessions on your machine. You can search by session name or change the query params to search the session content itself. If you still cannot remember what you did in a session, it will give you a tiny preview + use the built-in Apple Foundation Model (if you're on a Mac) to summarize the session for you.

Once you've found your session, you can hit Enter to resume it. If you use it in herdr and the session is already open, it switches to that session's workspace. Inactive sessions resume in a new workspace.

It's quite fast and easy to use. I benchmarked it with 150+ sessions, and the tool loads in 200 ms. This could be fine-tuned further, maybe with a rewrite in Rust, but for now I'm happy with the result. You can bind it to your favorite hotkey in herdr. I'm currently using Prefix+CTRL+S to open up the overlay.

If that sounds interesting to you, give it a shot. You can find the [landingpage here](https://fzf-sessions.christianblessing.me), on [GitHub](https://github.com/christianblessing-me/fzf-sessions) or install it straight within herdr via (yes, the plugin is [officially listed](https://herdr.dev/tui/plugins/)).

{{<highlight Bash>}}
herdr plugin install christianblessing-me/fzf-sessions
{{</highlight>}}
