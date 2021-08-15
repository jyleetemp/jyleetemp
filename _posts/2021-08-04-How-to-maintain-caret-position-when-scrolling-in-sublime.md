---
title: "How to maintain relative caret position when scrolling with `scroll_lines` in Sublime Text 4"
date: 2021-08-04 17:58:26 +0900
tags: [Sublime]
toc: false
categories: [Sublime]
---

## Key-binding `scroll_lines` in Sublime `Vintage` mode
I've been using Sublime Text 4 in `Vintage` mode, which enables vi-like editing.
When I use vi(m) in Ubuntu, I really like to scroll using `ctrl-u` and `ctrl-d`, because these shortcuts show a half-paging behavior and maintain relative caret position, which is convenient enough for me to scroll pages while keeping my hand on the keyboard.
However, in Sublime's `Vintage` mode, the shortcuts are not set as a default.
To enable scrolling using `ctrl-u` and `ctrl-d`, I've initially added following key mappings (**Preferences**-**Key Bindings** in Sublime's menu bar):
```json
{"keys": ["ctrl+u"], "command": "scroll_lines", "args": {"amount": 30.0}, "context": [{ "key": "setting.command_mode"}]},
{"keys": ["ctrl+d"], "command": "scroll_lines", "args": {"amount": -30.0}, "context": [{"key": "setting.command_mode"}]},
```
, which are the mappings commonly suggested by many Sublime users.

However, the aforementioned key mappings cause the caret to always be located 30 lines before (or after) from the currently viewable range of page lines when I scroll down (or up).
As soon as I press an arrow key to move the caret, the view jumps up or down 30 lines to where the caret is, which is very annoying as I have to relocate the caret using mouse or arrow keys every time I do scrolling.


## Retaining caret position with `scroll_lines`
To resolve this problem, I wrote a simple plugin, which retains the relative caret position after scrolling.

1. Click **Tools-Developer**-**New Plugin** in Sublime's menu bar, and save the following code as `scroll_line_retain.py` (I saved mine under `Packages/Users`). 
    ```python
    import sublime
    import sublime_plugin

    class ScrollLinesRetainCommand(sublime_plugin.TextCommand):
        def run(self, edit, amount):
            if (len(self.view.sel()) == 1 and self.view.sel()[0].empty()):
                maxy = self.view.layout_extent()[1] - self.view.line_height()
                curx, cury = self.view.viewport_position()

                row_current = self.view.rowcol(self.view.visible_region().begin())[0]
                rowcol_caret = self.view.rowcol(self.view.sel()[0].begin())
                row_rel_caret = rowcol_caret[0] - row_current
                col = rowcol_caret[1]

                delta = self.view.line_height()
                nexty = min(max(cury - delta * amount, 0), maxy)
                self.view.set_viewport_position((curx, nexty))

                row_caret = row_rel_caret + (nexty/delta)
                pt = self.view.text_point(row_caret, col)
                if pt > self.view.size():
                    pt = self.view.size()

                self.view.sel().clear()
                self.view.sel().add(sublime.Region(pt))
            else:
                self.view.run_command("scroll_lines", {"amount": amount})
    ```

2. Add the following key-mappings (**Preferences**-**Key Bindings** in Sublime's menu bar):
    ```json
    {"keys": ["ctrl+u"], "command": "scroll_lines_retain", "args": {"amount": 30.0}, "context": [{ "key": "setting.command_mode" }]},
    {"keys": ["ctrl+d"], "command": "scroll_lines_retain", "args": {"amount": -30.0}, "context": [{"key": "setting.command_mode"}]},
    ```

**Note:** This works best with `"scroll_past_end": 0.5,` which specifies how much scrolling past the end of the buffer should be allowed. (**Preferences**-**Settings** in Sublime's menu bar).
{: .notice--info}


## References
[A better paging behavior](https://forum.sublimetext.com/t/a-better-paging-behaviour/42454)<br/>
[Move up or down by N lines](https://forum.sublimetext.com/t/move-up-or-down-by-n-lines/42193/2)
- - -
