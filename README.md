# gha-label-new-issues

A GitHub Action that adds a label (`triage` by default) to every newly opened issue, so nothing new slips past review. If the repository doesn't have the label yet, the action creates it on the first run.

## Usage

Add `.github/workflows/label-new-issues.yml` to your repository:

```yaml
name: Label new issues
on:
  issues:
    types: [opened]

jobs:
  label:
    runs-on: ubuntu-latest
    permissions:
      issues: write
    steps:
      - uses: StoneCypher/gha-label-new-issues@v1
```

That's all. Every issue opened from now on gets the `triage` label. That includes issues opened through the web, the API, `gh`, or a bot.

## Inputs

| Input | Default | Meaning |
|---|---|---|
| `label` | `triage` | The label to add. |
| `color` | `FBCA04` | Hex colour, without the `#`. Used only when the action has to create the label. |
| `token` | `${{ github.token }}` | A token allowed to edit issues and create labels. The default works when the job grants `issues: write`. |

For example, to use a different label:

```yaml
      - uses: StoneCypher/gha-label-new-issues@v1
        with:
          label: needs-review
          color: D93F0B
```

## Notes

- **Permissions.** The job needs `issues: write`. That's enough both to add a label and to create one.
- **Cost.** The action runs as a step in *your* workflow, so any Actions minutes are billed to your repository, not to this one. Public repositories run on GitHub's standard runners for free. On a private repository each new issue uses the one-minute billing minimum.
- **Versions.** Pin `@v1`. The `v1` tag moves forward only for compatible changes, so a new release can't break your issue intake. Pin a full commit SHA if you want nothing to change underneath you.
- **Issues only.** Pull requests aren't labelled. Run the action from an `issues` event, and it stops with a clear error on any event that has no issue.

## License

MIT. See [LICENSE](LICENSE).
