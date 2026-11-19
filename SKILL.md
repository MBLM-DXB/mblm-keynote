---
name: keynote
description: Read, create, and edit Keynote presentations in Keynote Interchange Format (.keyx) as files, with no app running. Use when the user asks to create, make, generate, or build slides, a slideshow, a deck, a presentation, or a Keynote file, or to change, restyle, summarize, or extract text from an existing .keyx.
---

# Keynote presentations

You read, create, and edit Keynote presentations (Keynote Interchange Format `.keyx`
files) by writing a small Python program with the `creator_studio_doc_format` library.

This is Apple's `creator-studio-doc-format` skill, modified so Allumni can run it.
The unmodified text is in `SKILL.upstream.md`. You write a file. You do not connect
to the Keynote app, and this does not need an Apple Creator Studio subscription.

## Workflow

### 0. Check the setup

Allumni installs `requirements.txt` into `.venv` before the first command and puts
that interpreter first on `PATH`. Use `python3` for every script.

Confirm the library and print its package directory:

`python3 -c "import creator_studio_doc_format as m, os; print(os.path.dirname(m.__file__))"`

If the import fails, install the pinned package and retry the same command:

`python3 -m pip install -r requirements.txt`

Do that yourself. Do not ask the user to install it, and do not stop before the retry.

If this fails, stop and say the workspace Python is older than 3.13:

`python3 -c "import sys; raise SystemExit(0 if sys.version_info >= (3, 13) else 1)"`

`<package>` below is the directory the import printed. Read and search it by absolute path.

### 1. Look up the API in the installed package

Start from the examples, then look up exact fields:

- Start at `<package>/examples/README.md`. It indexes every sample script and says what
  each covers. Then read the sample scripts it points to, which are working patterns to
  copy. Take filenames from that index, not from memory.
- Search for exact field names and types when the examples don't cover something.
  Search `<package>/core/models.py` for a pattern such as `class Shape`. The file is large,
  so search it rather than reading it whole. Enumeration values are exact strings, so look them
  up rather than guessing a synonym.

What the script does depends on the request:

- To create a presentation, build the top-level document and save it.
- To edit a presentation, open the user's file with `KeyxDocument.open`, change the
  models, and save. Save to a new filename unless the user asks to overwrite the original.
- To analyze a presentation, open the file and read it. No save is needed.

A real document can hold thousands of models, so print only a depth-limited view of one
and filter `navigation.iterate_models` to find a model, instead of printing a whole
subtree. `<package>/examples/inspection.py` shows both.

### 2. Run and deliver

Write the script in this skill directory and run it with `python3`.

Save the presentation to `exports/<short-name>.keyx`. Create `exports/` if it is missing.
Pass `validate_document` to `save` as a `pre_save_hooks` entry, as the examples do.
`validate_document` raises before anything is written if the document breaks the format's
rules. If the script raises, read the error, fix the script, and rerun it. Keep
`validate_document` in `pre_save_hooks` on every run, including the rerun after a failed
save, because removing it to force a save isn't a fix. Hand over only a document that has
passed it.

After a save, Allumni attaches `exports/<short-name>.keyx` to the chat. Tell the user
that filename. They open it in Keynote. After an analysis, report what you found and
do not save.

## Design defaults

These apply when you create or restyle slides. Commit to a clear aesthetic direction, such as bold minimalism, editorial density, warm
organic, or sharp corporate. Intentionality matters more than intensity. Give each
presentation a look of its own, rather than the same generic one.

The following are soft priors, so deviate when the content justifies it:

- Hold colors, typography, and spatial logic consistent throughout.
- Limit icons and emoji to an intentional, consistent visual language.
- Prefer negative space over filling the canvas, and fragments over full sentences.
- Scale the following point sizes proportionally if you change the canvas from its
  default of 1920 x 1080 pt.
- Set titles at about 90–150 pt, and body text at about 40–60 pt and at least 32 pt. Body
  text is anything the audience reads as content, including descriptions, bullets, and
  captions under icons or images. Only footers, slide numbers, and source notes go smaller.
- Give one section of a multisection grid more weight than the rest, so the slide keeps
  its hierarchy.
