---
project: plan
---

Set this factory up with me in one go, here in this chat.

Work out as much as you can on your own first, then come back with what you found and what you need
from me. Do not ask me for anything you can look up yourself.

Find out:

- Which repositories this factory works on, and their default branches.
- Which Linear team and project the issues belong in, and the workflow states that team already
  uses. Use theirs. Do not ask me to create new ones.
- Which of those states each phase moves the issue to. List the team's states back to me and
  propose the mapping; I confirm or correct it. With the usual four states that is Plan → `Todo`,
  Build → `In Progress`, Review and QA → `In Review`, and `Done` when a human merges. If the team
  has no separate review state, two phases can share one; say so rather than inventing a state.
- Which Slack channels matter: where people report bugs and ask for things, and where you should
  post when production looks wrong. If those differ, get both; the second is Monitoring's channel.
- Who owns that second channel, so QA and Monitoring know who to loop in when a finding or a
  verification gap needs a person.
- Whether QA needs its own test accounts, staging-safe credentials, or fixtures to run the product,
  and where they already live if so.
- Our timezone.

Then set it up:

- Build a pre-baked environment for these repositories and attach it to this pod: the system
  packages the repositories need, the checkout, the dependency install, and any code generation,
  all done at build time. A session should be able to run the test suite without installing
  anything first. Time the build and tell me how long a cold session now takes to get to a running
  app.
- Create the QA browser profile: a persistent Kernel profile, signed in once to the test accounts
  for the product and for every third-party login a QA flow has to pass through. QA launches this
  profile instead of typing a password. Tell me which logins you need me to complete, and give me
  the hosted URLs to do it.
- Create the shared memory store `factory-memory` and attach it to all five projects.
- Add each repository as a reference on Plan, Build, Review, QA, and Monitoring.
- Map the Slack intake channels and the Linear project to Plan, so a mention there reaches you.
- Map the repositories to Review for access and mentions, with `prListenerEnabled: false`. QA marks
  a pull request ready at the end of the run, and automatic review would fire again on that.
- Map the engineering-updates channel to Monitoring, so its scheduled posts land there.
- Connect Kernel on QA, PostHog on Monitoring, and check Context7 answers on Monitoring.
- Connect the QA GitHub upload credential (`QA_GITHUB_UPLOAD_TOKEN`) on QA; the e2e-verification
  skill needs it to publish screenshots and recordings to a pull request. Ask me for it if it is not
  already connected; never write the token value anywhere but the credential itself.
- Set the timezone on the three Monitoring schedules. Keep them paused until setup succeeds.

Run a setup trial. Explain the temporary PR comment and Slack test messages before posting them:

- Send QA an onboarding task, explicitly separate from `verify-change`, to run a setup trial on each repository: check out a fresh worktree, work
  through `e2e-verification`'s "Start the app" section to build the shared recipe, open a Kernel
  browser on the QA profile and confirm it is already signed in, capture a short recording with the
  overlay on, cut it with FFmpeg, and publish and play it back on an agreed test PR comment with
  `QA_GITHUB_UPLOAD_TOKEN`. Check attachment access without signing in.
  Then clean the worktree and stop. This is the
  same Ren-task handoff real work will use, so it proves the mechanism reaches QA, not only that it
  exists, and it leaves the recipe in `factory-memory` before the first real QA run needs it.
- Confirm the trial used its own worktree, environment files, ports, and processes, and cleaned up only its own resources.
- Post one short line in the intake channel and one in engineering-updates to confirm Plan and
  Monitoring can actually post where they were mapped.

Ask me about anything you could not resolve, and tell me plainly what is not working yet rather than
marking setup done. Then write what you learned about the team and the codebase into
`factory-memory` so the other projects start with it. Three of those entries are load-bearing and
every project reads them before it works, so write them even if nothing else lands:

- The app-setup recipe, including what the pre-baked environment already did so nobody redoes it.
- The QA browser profile's name and which accounts it is signed in to.
- The phase-to-Linear-state mapping I confirmed.

Once setup succeeds, enable release monitoring every four hours and docs and memory nightly.

When it is all connected, send my first real request through: I will describe something, you plan it
on Linear, and I will approve it there.
