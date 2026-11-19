# Keynote skill for Allumni

A small fork of Apple's [creator-studio-doc-format skill](https://github.com/apple/creator-studio-doc-format/blob/main/skills/creator-studio-doc-format/SKILL.md) (Apache-2.0).

The Python library stays on PyPI (`creator-studio-doc-format`). This repo only changes the skill so Allumni can run it:

- `requirements.txt` sits next to `SKILL.md`, so the skill sandbox installs the library into `.venv`.
- If that install has not happened yet, the agent runs `pip install` itself.
- Finished decks are saved as `exports/*.keyx`, which Allumni attaches to the chat.

Writing a `.keyx` file does not need an Apple Creator Studio subscription. Opening it needs Keynote on an Apple device.

## Install in Allumni

Import this repository as a skill from its GitHub URL. `SKILL.md` is at the repository root.
