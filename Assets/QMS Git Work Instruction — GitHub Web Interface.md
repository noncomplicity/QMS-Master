---
id: 5edb1fc
title: "QMS Git Work Instruction — GitHub Web Interface"
version: 2
author: "Jakob"
effective_date: 2026-03-04
type:
process: "Document and Record Control"
requirements: "ISO 13485_2016 - 4.2.4 ISO 13485_2016 - 4.2.5"
owner:
---
Owner [Head of Quality Management](Head%20of%20Quality%20Management.md)
Type: #WorkInstruction
# Purpose

This guide describes how a QMS developer makes changes to the QMS using to make controlled changes via the **GitHub web interface** using the CI→CR→CIA→CCR flow defined in the [QMS Change Control Procedure](QMS%20Change%20Control%20Procedure.md).  

## Scope
Changes to QMS content stored in Git. 

GitHub is used with the GitHub CLI (`gh`). 
Adjust notes at the end if using GitLab (`glab`).

---

## 1. Create a Branch (Change Initiation)

1. Navigate to the repository on GitHub and ensure you are viewing the `main` branch.
    
2. Click the branch selector (e.g., `main`) and type a **descriptive branch name** that reflects the intent, for example:
    
    - `procedure/drc-tighten-approval-text`
        
    - `form/capa-log-add-risk-column`
        
    - `policy/record-retention-clarify-archives`
        
3. Press **Create branch: {your-branch-name} from `main`**.
    
4. Verify you are now on your new branch (the branch selector shows your branch name).
    

> Rationale: Branch creation is the **Change Initiation**. No tracking issue is required. Work begins directly on this branch.

---

## 2. Edit Files and Stage Changes

1. In your branch view, browse to the file you want to edit and click **✏️ Edit this file**.
    
2. Make content changes in the editor. Prefer small, focused edits grouped by topic.
    
3. Scroll to **Commit changes** and write a **simple, human-readable commit message** that explains _what_ changed.
    

**Example commit message**

```
Clarify approver role and tighten acceptance criteria
```

4. Optionally add a short description if the change needs extra context.
    
5. Choose **Commit directly to the {your-branch-name} branch** and click **Commit changes**.
    
6. Repeat for additional files as needed. Keep related edits in separate commits for clarity.
    

---

## 3. Open a Pull Request (Change Request)

1. After committing, click **Compare & pull request** or open the **Pull requests** tab and click **New pull request**.
    
2. Set **base** to `main` and **compare** to your branch.
    
3. Title the PR clearly and concisely to describe the change.
    
4. In the PR description, complete the **Change Impact Assessment (CIA)** and required sections. Include:
    
    - **Purpose of change** (what & why)
        
    - **Scope** (documents/areas affected)
        
    - **Impact assessment** (quality, regulatory, risk, dependencies)
        
    - **Verification/acceptance** plan (how we confirm the change is correct)
        
    - **Rollback**/mitigation approach if needed
        
    - **Records to update** (forms, logs) and effective date if applicable
        
5. Assign reviewers (see Section 4) and apply relevant labels (e.g., `QMS`, `procedure`, `form`).
    
6. Click **Create pull request**.
    

> The PR now functions as the **formal Change Request**. All subsequent discussion, approvals, and evidence remain in this PR.

---

## 4. Request Review (CIA Execution)

1. In the PR sidebar, click **Reviewers** and add the required roles (e.g., Process Owner, QA/RA, and an independent peer as needed).
    
2. If checks are configured (linting, spellcheck, build/preview), confirm they start running under **Checks**.
    
3. Respond to review comments by pushing additional commits to the same branch via the web editor (repeat Section 2) or by resolving discussion threads.
    
4. Ensure the **CIA content is complete and current** in the PR description before approval.
    

---

## 5. Approve (CCR) and Merge to `main`

1. Once reviews are complete and required approvals are granted, the PR represents the **Change Control Record (CCR)**, containing:
    
    - Link to branch and commits
        
    - Final CIA text in the PR description
        
    - Reviewer approvals and timestamps
        
    - Status checks and artifacts
        
2. Click **Merge**. Prefer **Squash and merge** for a linear history unless the repository policy specifies otherwise.
    
3. Confirm the merge. The PR will close, and the change is now effective on `main` (subject to any stated effective date in the CIA).
4. Delete the branch via the PR page (**Delete branch**) to keep the branch list tidy.    

---

## 6. Quality Notes & Tips

- Keep PRs **small and single‑purpose** for faster review.
    
- Use **clear commit messages** and a **descriptive PR title**; avoid internal shorthand.
    
- Make the **PR description the single source of truth** for impact analysis; update it as the discussion evolves.
    
- Prefer **suggested changes** in reviews for minor edits; reserve rework commits for substantive changes.
    
- If checks fail, open the **Checks** tab, read logs, and address findings before requesting approval.
    