---
title: "Espanso text replacement for Slack"
date: 2026-08-08T10:34:11+02:00
draft: false
---

Espanso is my default text replacement tool, I've been using it since years. I mainly replaced plain text: terminal commands, email phrases and so on. Recently I stumbled upon the issue that with Slack's way of formatting pasting plain text does not do the trick, formatting like bullets or text weight won't work.

I figured that Espanso is also able to execute bash scripts on trigger: replace. By doing so you can use shell scripts to populate the clipboard and then use Espanso to replace the text.

## Example bash script

{{<highlight Bash>}}
#!/bin/bash

HTML_FILE="$(mktemp).html"
cat > "$HTML_FILE" <<'HTML'
:wave: <b>This text is bold!</b>
HTML

osascript -l JavaScript - "$HTML_FILE" <<'JXA'
function run(argv) {
  ObjC.import('AppKit');
  ObjC.import('Foundation');
  const html = $.NSString.stringWithContentsOfFileEncodingError(
    argv[0], $.NSUTF8StringEncoding, null).js;
  const pb = $.NSPasteboard.generalPasteboard;
  pb.clearContents;
  pb.setStringForType($(html), 'public.html');
  // fallback so pasting into a terminal still gives something readable
  pb.setStringForType($(html), 'public.utf8-plain-text');
}
JXA

osascript -e 'tell application "System Events" to keystroke "v" using command down'
rm -f "$HTML_FILE"
{{</highlight>}}

Ensure that the script is executable. In Espanso add this to the base.yml file:

{{<highlight YAML>}}
- trigger: ":trigger-command"
    replace: "{{fire}}"
    vars:
      - name: fire
        type: shell
        params:
          cmd: "(sleep 0.4; /path-to-script/script.sh) >/dev/null 2>&1 &"
{{</highlight>}}
