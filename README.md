# VoiceDot reviewer website

A small, fictitious Harbor Notes website for reproducing the full workflow:
page feedback → authorized coding change → Git push → public deployment → human
review. This is sample website source, not the VoiceDot backend or MCP server.

## Addresses

- Coding preview: **https://review.voicedot.ai/** (verify its current deployment)
- Source: https://github.com/piotrsynowiec/voicedot-review-demo
- MCP: https://mcp.voicedot.ai/mcp
- Dedicated demo-account sign-in: https://app.voicedot.ai/reviewer/login/
- Existing, independent browser fixtures:
  https://app.voicedot.ai/reviewer/demo/harbor-notes/ and
  https://app.voicedot.ai/reviewer/demo/fieldwork-notes/

Reviewer sign-in and repository-write credentials are supplied privately in the
OpenAI review form. None are included in this repository. Do not use a customer's
account or change its feedback. The public `.voicedot.json` selects Harbor Notes;
the authenticated connection must separately authorize it.

## Open the coding workspace

```sh
git clone https://github.com/piotrsynowiec/voicedot-review-demo.git
cd voicedot-review-demo
git switch review
```

`main` is the starting version. `review` is the only deployed branch. Fetch and
inspect the current review branch before editing; never reset another reviewer's
work. The intentionally light **Start a workspace** button is the baseline
contrast issue. If a previous rehearsal already fixed it, ask the operator for
a reset; do not pretend to make the same fix twice.

## Reproduce the full cycle

1. Open the coding preview and leave a real comment pinned to the primary button.
   Example: “Please make this button easier to read by using a darker green.”
2. Connect the plugin using the dedicated account and authorize the intended
   project. Ask the coding agent to read the comment **from review.voicedot.ai**,
   inspect the relevant thread and propose the change.
3. Explicitly authorize the change and publication to this demo repository.
   The agent changes `site/styles.css`, shows the diff and checks the page.
4. Run the lightweight publication tests:
   `python3 -m unittest discover -s tests -v`.
5. Commit and push to `review` using your authorized Git credential helper:
   `git push origin review`. Never put a token in the remote URL or a command.
6. Wait for **Publish review website** in GitHub Actions to succeed. Open
   `https://review.voicedot.ai/deployment.json` and compare its full `commit` with
   `git rev-parse HEAD`. The page footer also shows the deployed revision.
7. Open the actual HTTPS page and check the changed button. A local edit or a
   green test is not evidence of the public deployment.
8. Ask the agent to prepare a review round for the actual changed thread. Confirm
   its preview. Open the returned VoiceDot review URL and personally mark the
   change **Checked** or **Needs fix**.

For optional local layout checks, `python3 -m http.server 4391 --directory site`
serves http://localhost:4391. That HTTP address is only a layout preview; it is
not a verified VoiceDot installation. Check widget collection on the exact
authorized public HTTPS origin. No Python package install or npm build is needed.

## Publication design

Pushes to `review` run the pinned official GitHub Pages actions. Only four
explicit static sources and generated deployment manifests are published;
repository instructions, credentials and build scripts are not part of the site.
CSS/JS filenames include their content hash. The page footer reads a manifest
bound to that HTML revision, so cached assets cannot claim a newer deployment.
The publishing job has no Cloudflare or VoiceDot credentials. The deployment
job uses short-lived GitHub permissions confined to this Pages repository.

The public source may be downloaded or forked without write access. Pushing to a
fork does not update this shared preview. Reviewers use the separately provided,
repository-limited write credential or their own authorized collaborator access.
Credentials must not grant workflow/administration access or access to other
repositories. `main` is protected after bootstrap. Do not share an owner's token.

The first comment on a newly added VoiceDot address and final review decisions
are human actions. The agent can inspect a fresh real backend ping but must not
manufacture it. The registration CLI is a separate workflow, outside this MCP.

## Operator reset

After a completed rehearsal, preserve the before/after commit IDs and evidence.
Restore only the intended sample file content from `main` in a new commit on
`review`; do not rewrite branch history, delete feedback or impersonate Checked.
Keep the existing browser fixtures unchanged and use a new identifiable human
comment for a new recording. See the private operator runbook for the current
release checklist and credential provisioning.
