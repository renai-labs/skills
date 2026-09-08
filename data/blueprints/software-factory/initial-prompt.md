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
- Which Slack channels matter: where people report bugs and ask for things, and where you should
  post when production looks wrong.
- Our timezone.

Then set it up:

- Create the shared memory store `factory-memory` and attach it to all five projects.
- Add each repository as a reference on Plan, Build, Review, QA, and Monitoring.
- Map the Slack intake channels and the Linear project to Plan, so a mention there reaches you.
- Map the repositories to Review for access and mentions, with `prListenerEnabled: false`. QA marks
  a pull request ready at the end of the run, and automatic review would fire again on that.
- Connect Kernel on QA, PostHog on Monitoring, and check Context7 answers on Monitoring.
- Set the timezone on the three Monitoring schedules and enable them: release monitoring every three
  hours, docs and memory nightly.

Ask me about anything you could not resolve, and tell me plainly what is not working yet rather than
marking setup done. Then write what you learned about the team and the codebase into
`factory-memory` so the other projects start with it.

When it is all connected, send my first real request through: I will describe something, you plan it
on Linear, and I will approve it there.
