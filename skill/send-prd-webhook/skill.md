---
name: send-prd-webhook
description: Send a webhook notification to Abraxas with the current branch name. Use after /prd-task to notify Abraxas that a PRD branch is ready.
---

# Send PRD Webhook Skill

Send a webhook notification to Abraxas with the current git branch name.

## Usage

Simply run the `send-prd-webhook` command in your terminal:

```bash
send-prd-webhook
```

This command is automatically available on Abraxas manifest sprites.

## What It Does

1. Gets the current git branch name
2. Builds a JSON payload with `type: "branch_ready"` and the branch name
3. Signs the payload with HMAC-SHA256 using `$MANIFEST_WEBHOOK_SECRET`
4. Sends the webhook to `$MANIFEST_WEBHOOK_URL`

## Prerequisites

The following environment variables must be set (automatically configured by Abraxas manifest sprites):

- `MANIFEST_WEBHOOK_URL` - Abraxas webhook endpoint
- `MANIFEST_WEBHOOK_SECRET` - HMAC signing secret

## Example

After running `/prd-task` and pushing your branch:

```bash
git checkout -b prd-my-feature
git add .opencode/state/my-feature/
git commit -m "chore: init prd my-feature"
git push -u origin prd-my-feature
send-prd-webhook
```

Output:

```
Sending webhook for branch: prd-my-feature
Webhook sent successfully!
  Type: branch_ready
  Branch: prd-my-feature
```

## Error Messages

If environment variables are missing:

```
ERROR: MANIFEST_WEBHOOK_URL and MANIFEST_WEBHOOK_SECRET must be set
```

If not in a git repository or detached HEAD:

```
ERROR: Could not determine branch name
```

If webhook request fails:

```
ERROR: Webhook failed with status <code>
```
