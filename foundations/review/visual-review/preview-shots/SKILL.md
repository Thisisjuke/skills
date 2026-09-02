---
name: preview-shots
description: Present a small, ordered set of visual artifacts for human inspection using the available image viewer, with clear captions and without altering unrelated user windows or files.
license: MIT
---

# Preview shots

Put the minimum useful set of saved visual artifacts in front of the user so
they can inspect and compare them efficiently. This skill presents evidence; it
does not perform the critique or decide acceptance.

## Curate the set

Choose artifacts that answer the review question:

- the current result;
- the relevant reference or previous result when one exists;
- materially different states, viewports or variants;
- focused crops only when the full artifact hides important detail.

If the user names files, preserve that scope. Otherwise omit redundant debug
captures while retaining any state that could change the verdict. Order files
from the primary decision surface to supporting evidence.

## Present safely

Use an image viewer available in the current environment. Prefer one grouped
view, gallery, contact sheet or viewer session that supports quick navigation.
Opening a desktop application is an external side effect: follow the active
environment's authorization rules and do not assume a GUI exists.

Platform examples are optional implementation choices:

- macOS Preview can open several paths in one invocation;
- Linux and Windows may use an installed image viewer or file manager;
- headless environments should return artifact paths or a generated contact
  sheet instead of pretending that a window was opened.

Do not close, reuse or rearrange unrelated user windows. Close only a viewer
session created for this task, and only when requested or when ownership of
that session is certain. Never modify the reviewed artifacts merely to make
them easier to present.

## Caption the evidence

List each artifact in display order with a short explanation of what it shows
and why it matters. State capture conditions when differences in viewport,
state, theme, locale, data or rendering platform affect interpretation.

Return the opened or presented paths, the ordering, and any limitation such as
missing GUI support or an artifact that could not be displayed.
