# VibeWise for Codex

This is the Codex and local ChatGPT Work adaptation of [VibeWise](https://github.com/nykooi1/vibe-wise). It keeps the original learning-first workflow: the learner designs, the agent implements the agreed design, and project-local notes preserve learning context across sessions.

The adaptation is self-contained in this directory. The original Claude Code plugin at the repository root remains available. The [MIT license](LICENSE) and original copyright notice are included with this adaptation.

## Install from GitHub

Add this fork as a marketplace:

```sh
codex plugin marketplace add arrow255/vibe-wise
```

Then open the Codex desktop app's Plugins Directory, select the **VibeWise for Codex** marketplace, and install **VibeWise for Codex**. Start a new local coding task in the project you want to learn, and ask to use `vibe-wise-codex:learn` or say “Use VibeWise to help me learn while we build this project.”

## Install from this checkout

Add the repository as a local plugin marketplace:

```sh
codex plugin marketplace add /absolute/path/to/vibe-wise
```

Install the plugin from the local **VibeWise for Codex** marketplace in the Plugins Directory, then start a new local coding task.

The `learn` skill starts or resumes the learning workflow. The `reset` skill previews a reset, backs up the project's learning notes, and restarts onboarding. The reset helper does not modify application code.

## Supported behavior

- Project notes live in `.vibe-wise/` (or an existing `.sensible-vibes/`) inside the project. They contain `profile.md`, `progress.md`, and `project-map.md`. Add `.vibe-wise/` to the project's `.gitignore` if you do not want to commit personal learning notes.
- A read-only `SessionStart` hook restores active learning mode on startup, resume, clear, and compaction. It leaves paused learning mode paused.
- The hook runs only in a local Codex or ChatGPT Work environment with Python 3 and a trusted plugin hook. Installing the plugin in an ordinary web chat does not deploy local scripts or restore project files automatically.
- The hook injects file-reading instructions, not learning notes. The agent reads only the relevant files after the hook runs. No separate account, backend, or telemetry is required.
- The host's permissions and higher-priority instructions still apply. VibeWise does not replace them. If a coding request already authorizes an implementation, the agent should not demand a second approval just to satisfy a checkpoint.

## Verify

From the repository root:

```sh
python3 -B -m unittest discover -s plugins/vibe-wise-codex/tests -v
python3 /path/to/plugin-creator/scripts/validate_plugin.py plugins/vibe-wise-codex
git diff --check
```

The tests exercise activation, state restoration, project and worktree boundaries, paused mode, compaction, and backed-up reset behavior. They do not establish that a live model teaches well or that a user has trusted the bundled hook. For a live check, install the plugin, start `learn` in a disposable project, finish onboarding, restart the task, then check that the learning profile resumes. Review and trust the plugin's bundled hook when Codex prompts for it.
