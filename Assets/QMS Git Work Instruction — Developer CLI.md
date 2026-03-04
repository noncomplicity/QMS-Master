---
id:
title:
version:
author:
effective_date:
type:
process: "Document and Record Control"
requirements: "ISO 13485_2016 - 4.2.4 ISO 13485_2016 - 4.2.5"
owner:
---
Owner [[Head of Quality Management|Head of Quality Management]]
Type: #WorkInstruction

## Purpose 
Enable a QMS developer to make controlled changes via CLI using the CI→CR→CIA→CCR flow defined in the [[QMS Change Control Procedure]].  

## Scope
Changes to QMS content stored in Git. 

GitHub is used with the GitHub CLI (`gh`). 
Adjust notes at the end if using GitLab (`glab`).

## Prerequisites

- Git installed and configured with your name and email.
    
- SSH or HTTPS access to the repo and permissions to create branches and PRs.
    
- GitHub CLI authenticated: `gh auth status`.
    
- Mainline branch is `main`.
    
- Local repo cloned and clean working tree.
    

## 1) Change Initiation (CI) — Create a branch

Each QMS change begins with creating a dedicated branch that clearly describes the purpose of the change. Branch names should be descriptive and self-explanatory.

Examples:

- `docs/update-document-control-procedure`
    
- `docs/revise-training-record-template`
    
- `docs/add-capa-workflow-description`
    

Run the following to create the branch directly from the latest `origin/main`:

```
git fetch origin
BRANCH="docs/update-document-control-procedure"
git checkout -b "$BRANCH" origin/main
```

## 2) Implement changes and commit

Edit with your preferred editor. Keep changes atomic and commit frequently. Keep commit messages simple and clear (`docs:`) and include a Signed-off-by line.

```
git status
git add path/to/changed-files
make check || true
git commit -m "Update Document & Record Control procedure"
```

Push your branch upstream:

```
git push -u origin "$BRANCH"
```

## 3) Change Request (CR) — Create a Pull Request

Create the PR as a draft while you complete the CIA in the PR body using the template.

```
gh pr create \
  --title "QMS: Update Document & Record Control" \
  --body-file .github/pull_request_template.md \
  --base main \
  --head "$BRANCH" \
  --draft
```

Convert to “Ready for review” when CIA fields are complete:

```
gh pr ready
```

## 4) Change Impact Assessment (CIA) — Review changes

Request the correct reviewers and monitor checks.

```
gh pr edit --add-reviewer "@org/qms-approvers" --add-label qms,cia
gh pr status
gh pr checks --watch
```

Address feedback with follow-up commits and keep the PR description synchronized with scope.

## 5) Change Control Record (CCR) — Approvals and merge

Merge only after required approvals and passing checks. Prefer a linear history.

```
gh pr merge --squash --delete-branch
```

If a tagged release is required, create it:

```
gh release create v2025.10.28 --notes "QMS updates incl. doc control"
```

## 6) Post-merge housekeeping

Sync your local main and record CCR metadata in your QMS register.

```
git checkout main
git pull --ff-only
```

## Troubleshooting quickies

Rebase small conflicts on latest main:

```
git fetch origin
git rebase origin/main
git rebase --continue
```

Amend the last commit message (before reviews):

```
git commit --amend
git push --force-with-lease
```

Convert PR back to draft if more CIA work is needed:

```
gh pr ready --undo
```

## Notes for GitLab users

Replace `gh` with `glab` and PR with MR.

```
glab mr create --title "QMS: Update Document & Record Control" --description-file .gitlab/merge_request_templates/QMS.md --target-branch main --draft
glab mr update --add-reviewer @group/qms-approvers
glab mr merge --squash --delete-source-branch
```

## Definition of Done for a QMS change

- PR contains the completed CIA fields.
    
- All required reviews and checks have passed.
    
- PR merged with squash; source branch deleted.
    
- CCR metadata captured in the register (or traceable directly to the merged PR).