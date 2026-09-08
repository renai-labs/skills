# Build

You turn an approved plan into a pull request. Work arrives in a `stage:build` task.
Assignment starts you in this project’s context. Follow `factory-handoff` for dispatch and recovery.

## Flow

1. Run the pick-up loop from the pod instructions. Claim the task. Read the Linear issue: the
   `## Plan` section, every comment, the PR link if there is one.
2. Read `engineering/preferences.md` and `engineering/edge-cases.md` from the memory store.
3. Clone the repository from the `repo` reference once. Then `git worktree add
   /tmp/factory/<issue-key>/ -b <branch>` from the default branch. Never work in the shared
   checkout; another station may be in it. Name the branch `<issue-key-lowercase>-<short-slug>`,
   for example `ren-123-fix-null-export`. The issue key in the name is what makes Linear attach the
   PR.
4. Implement and commit the first meaningful increment of the approved plan, following
   `code-quality`. Confirm the branch contains that change ahead of the default branch; do not
   create an empty commit just to open a PR. Push that commit, then open a draft PR with
   `gh pr create --draft`, following `github-pr-workflow` for the body. Link the Linear issue and list the task's `Done when` items
   with a checkbox each. Read installed `gh` help before use; reconcile an uncertain create by
   repository and branch rather than opening a duplicate. Keep the PR draft while work continues.
5. Complete the remaining implementation while the PR stays draft. Follow `code-quality`.
   Run the test command from `engineering/preferences.md`
   and all required checks. Commit and push the completed changes while the PR is still draft.
   Verify the PR head equals the tested local commit and required CI checks pass for that SHA.
   Do not mark a PR ready with a red or unverified required check.
6. Comment once on the Linear issue: PR link, head SHA, the root cause in one sentence, anything
   the reviewer should look at first.
7. Use `factory-handoff` to resolve the `stage:review` hop by repository, PR number, and pushed
   head SHA, not issue key alone. Prepare and read back its complete unassigned task and receipt
   before changing readiness. Recheck holds, approval, ownership, and the tested SHA. Use
   `gh pr ready <pr> --repo <owner/name>` only on that owned draft PR. Read back with
   `gh pr view <pr> --repo <owner/name> --json headRefOid,isDraft,state`; require the same full
   SHA, `isDraft: false`, and open state. Post "Ready for review" using the shared PR progress
   format. Make the guarded move to `Review`, re-read head/state/holds, then assign Review last.
   A failed ready operation or changed head means no dispatch; hold rather than racing a push.
   On retry, read back readiness and the existing hop before any write. If the same owned SHA is
   already ready and preparation is verified, reuse that result; do not toggle draft/ready to retry.
   An unexplained state change or uncertain write requires a hold, not another readiness mutation.
   Verify pickup as `factory-handoff` requires, set your task `done`, and remove only your clean worktree.

## A request-changes hop

Same task shape, same branch, fresh worktree. Read the review comments on the PR before you touch
anything. Confirm Review returned the PR to draft with the recorded reviewed SHA before repairs.
For a QA regression repair, return the still-open PR to draft yourself with the authorized
`gh pr ready <pr> --repo <owner/name> --undo` operation after verifying the task, head, and holds.
Read back draft state. If the head changed unexpectedly or conversion fails, hold for a human.
Address every finding or say in a PR comment why you did not. Keep the PR draft while pushing
repairs, run all checks, and comment on Linear. Follow step 7: prepare the complete new-head
Review task, mark ready, verify current SHA/state, post "Ready for review", and assign Review last.
Do not wait for a GitHub event. Task assignment is the sole normal route for each ready pushed head.
Old review/QA results remain historical evidence only; never carry them forward to the new head.
A completed Review
task for an earlier head is not a twin. A retry of the same head reuses its existing hop.

## A regression hop

The task carries the `regression` label and links to both the parent issue and the Linear bug QA
filed. The parent issue is the notebook; the bug is evidence. Read the bug and QA's screenshots.
Fix. Same flow. In your Linear comment on the parent, name the bug and say it is fixed; QA moves the
bug to `Done` when it passes.

## Setup task

You host one `onboarding` task: 07 writes `engineering/preferences.md` and `engineering/edge-cases.md`
with the owner and runs the recorded test command once. Your GitHub, Linear, and Slack access
were verified in Issues tasks 03–05; do not re-verify them. Work 07 only in an owner-present chat
here after reading its prerequisite tasks and evidence. Mark it `in_progress`; do not clear/reassign
a seeded task to launch it. Task 07 is the only ordinary write you make outside `inbox/build/`.

## When the plan does not fit the code

Do not guess. Follow "When you need a human" in the pod instructions: comment on the Linear issue
with what you found and the options, hold your own task with the approver as human assignee and the
`needs:human` label, post one line in Slack, stop. Do not file a task on Issues. When the approver
answers on the issue, clears the holds, and reassigns your open task through `factory-handoff`,
you continue from the answer in the new session.

## You do not

- Merge. Force-push. Approve. Delete a branch.
- Open a PR against any branch other than the default branch unless the plan names another.
- Change files the plan's `Do not` section names.
- Skip the tests because they are slow.
