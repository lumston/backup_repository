# backup_repository

Reusable GitHub Actions workflows used across the `lumston` org to:

1. Scan pushed code for leaked credentials
2. Mirror the repository to AWS CodeCommit (`us-east-1`) as an offsite backup

## Consuming from another repo

Add a workflow file (e.g. `.github/workflows/scan_credentials.yml`) that calls both reusable workflows:

```yaml
name: scan_credentials_and_backup_repo

on:
  push:
    branches:
      - '**'
      - '!main'
      - '!qa'
      - '!dev'
  pull_request:
    branches: [main, qa, dev]
  workflow_dispatch:

jobs:
  reusable_credentials_workflow:
    uses: lumston/reusable_workflows/.github/workflows/scan_credentials.yml@main
  backup_repo_workflow:
    uses: lumston/backup_repository/.github/workflows/backup.yml@main
    secrets:
      USER_CODECOMMIT:     ${{ secrets.USER_CODECOMMIT }}
      PASSWORD_CODECOMMIT: ${{ secrets.PASSWORD_CODECOMMIT }}
      TOKEN_GITHUB:        ${{ secrets.TOKEN_GITHUB }}
      AWS_CODECOMMIT_KEY:    ${{ secrets.AWS_CODECOMMIT_KEY }}
      AWS_CODECOMMIT_SECRET: ${{ secrets.AWS_CODECOMMIT_SECRET }}
```

## Required secrets

All five live at the **organization** level so consumer repos inherit them automatically:

| Secret | Purpose |
| --- | --- |
| `USER_CODECOMMIT` | HTTPS Git username for the CodeCommit IAM user |
| `PASSWORD_CODECOMMIT` | HTTPS Git password for the CodeCommit IAM user |
| `TOKEN_GITHUB` | GitHub PAT with `repo` scope, used to clone the source |
| `AWS_CODECOMMIT_KEY` | AWS access key ID (used to create the target repo if it doesn't exist) |
| `AWS_CODECOMMIT_SECRET` | AWS secret access key |

## About the CodeCommit password

AWS auto-generates HTTPS Git passwords that frequently contain characters reserved in URL userinfo (`@`, `/`, `:`, `+`, `#`, `%`, etc.). The workflow URL-encodes both username and password before embedding them in the push URL, so **store the raw password as-is** in the secret — no manual encoding required.

Reference for the encoding the workflow applies: [Percent-encoding reserved characters](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)

| ␣ | ! | # | $ | % | & | ' | ( | ) | * | + | , | / | : | ; | = | ? | @ | [ | ] |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| %20 | %21 | %23 | %24 | %25 | %26 | %27 | %28 | %29 | %2A | %2B | %2C | %2F | %3A | %3B | %3D | %3F | %40 | %5B | %5D |

## Troubleshooting

**`fatal: ... 403` on the push step.** Most common causes, in order:

1. `PASSWORD_CODECOMMIT` is stale — regenerate HTTPS Git credentials in IAM → Users → `<USER_CODECOMMIT>` → Security credentials, and update the org secret.
2. The IAM user lacks `codecommit:GitPush` on the target repo.
3. The secret value has stray leading/trailing whitespace (can happen when pasting from the AWS console).
