# Engineering Working Standard

This standard applies to human contributors and AI agents. It is intentionally language- and project-independent so it can be reused across repositories.

## Shared ownership and collaboration

- Treat code as a team asset. Another developer or agent must be able to understand, review, and continue the work without access to a private conversation.
- Inspect the existing code, documentation, and conventions before proposing changes. Improve unfamiliar systems without unnecessary rewrites.
- Distinguish agreed requirements, proposals, assumptions, and unresolved questions. Do not present an inference as an established fact.
- During guided pair programming, the human leads the pace and design decisions. Discuss the next implementation increment before editing; do not complete later learning steps ahead of agreement.
- Once an increment is agreed, carry it through implementation, appropriate verification, and review without repeatedly requesting permission for routine details within that scope.
- Communicate blockers and material discoveries promptly. If multiple contributors are involved, establish ownership and interfaces before overlapping edits. Do not overwrite unrelated work.

## Describe the task before implementation

For meaningful work, prepare a short task brief using `docs/templates/task-brief.md`, then create or update the repository issue before implementation. The issue is the authoritative task record. Local drafts and discussion can prepare it; do not maintain duplicate task checklists in Markdown and Issues. Link supporting design documents when needed. If issue creation is unavailable or not yet authorized, keep the brief as a clearly labeled draft and report that the issue is pending.

Capture:

1. **Problem and purpose:** Why the change is needed, who uses it, and the desired outcome.
2. **Scope:** Deliverables and explicit exclusions.
3. **Requirements:** Inputs, outputs, observable behavior, and relevant constraints.
4. **Failure behavior:** Relevant invalid inputs, dependency failures, partial outcomes, cancellation, and recovery expectations.
5. **Proposed approach:** Responsibilities, interfaces, important alternatives, and reasons for the choice.
6. **Acceptance criteria:** Observable conditions that establish completion.
7. **Implementation increments:** Small, reviewable steps and how each will be verified.

Keep open questions visible and resolve those that affect the next increment before implementing it. Existing prototypes or skeletons are provisional, not automatic design commitments.

Scale the detail to the change. A typo or routine local fix does not need a separate design document. Do not turn the standard into ceremony or attempt to design every future feature upfront.

## Development and commit workflow

Use Git and the hosting platform's tools, such as GitHub CLI (`gh`). Git tracks versions; Issues and PRs support collaboration; CI runs automated checks when configured. No custom CLI is required.

### Track the feature

- Create an issue before meaningful work. Urgent work may record the issue alongside implementation. Reuse an existing issue for already tracked work.
- One feature normally has one issue, one descriptive feature or fix branch, and multiple focused commits delivered through a PR.
- Keep two separate checklists in the issue: acceptance criteria describe the required outcomes; implementation steps describe the current delivery plan.
- Treat implementation steps as candidate commits, not a mandatory one-to-one mapping. Commit coherent, reviewable changes; a step may need multiple commits, and a commit may complete several closely related steps.
- Discoveries needed to complete the agreed scope stay in the same issue and branch. Update the checklist and record material requirement decisions. Track independent capabilities separately instead of silently expanding scope.
- After merging, use a new linked issue and branch for subsequent fixes or enhancements rather than reopening delivered work as the default.

### Deliver the change

1. Review scope, examples, failure behavior, open questions, and acceptance criteria before implementation. Resolve questions that block the next increment.
2. Inspect local work and the agreed base. Create a descriptive branch from updated main, or the repository's agreed base branch, without discarding existing changes. Continue an existing task branch when one exists; do not recreate it or overwrite other work.
3. Implement focused increments and run relevant local checks before committing. Review the diff and stage only task changes. A local commit does not trigger remote CI until pushed.
4. Push when authorized. For substantial, risky, or uncertain work, consider a draft PR after a useful commit is published. Small completed changes can open a normal PR directly. Link the issue.
5. Mark a draft ready when implementation and local checks are complete. Review the actual proposed revision and evaluate every acceptance criterion. Report local checks separately from CI; do not imply CI exists when it is not configured.
6. Address feedback and rerun relevant checks after changes. Perform service or integration verification when needed. Testing a running service is not automatically user acceptance testing (UAT); UAT explicitly verifies user/business acceptance.
7. Merge after required checks and review pass, within the authorized scope. Use a closing reference such as `Closes #3` only when the PR fully resolves the issue. Closing an unmerged PR does not deliver its changes.
8. Switch the local checkout to main and update it when safe, preserving remaining work. Verify the resulting revision and PR and issue states. Report anything that prevents completing this step.

Merge brings accepted changes into main; release tags, artifact publication, and deployment are separate actions. Merge strategy, versions, branch protection, and CI/CD triggers are repository decisions.

### Authorization and maintenance

- Issue creation, pushing, PR creation or updates, merging, and releasing have external effects. Follow the user's actual authorization and existing session scope. This standard alone does not authorize those actions; avoid repeated confirmation for actions already authorized.
- Do not bypass configured checks or protections.
- Keep issue and PR descriptions aligned with the final scope and verification evidence. Explicitly report incomplete acceptance criteria and unresolved decisions.
- Add workflow automation only when recurring needs justify it.

## Implement in small increments

- Implement one agreed behavior at a time. Keep changes focused and independently reviewable where practical.
- Prefer the simplest approach that meets current requirements. Introduce abstractions, dependencies, concurrency, or infrastructure for a concrete reason.
- Evaluate resource use and failure boundaries where relevant: memory, concurrency, timeouts, retries, duplicate effects, persistence, and backpressure.
- State guarantees precisely. Receiving data, storing it durably, processing it, and completing an external effect are different milestones.
- Revisit the brief when evidence changes the design. Record material decisions and their reasons, not a transcript of the discussion.

## Write for maintainers

- Prefer clear names, explicit responsibilities, and predictable control flow over cleverness.
- Follow the repository's language conventions and established error-handling contract. Make failures actionable and avoid silently discarding data or errors.
- Use comments to explain non-obvious intent, constraints, and tradeoffs. Avoid comments that merely repeat the code.
- Make interfaces and data ownership clear. Do not rely on compile-time types alone to validate external input.
- Keep unrelated refactoring out of the change. Preserve compatibility unless a change is intentional and documented.

## Verify and review

- Choose checks from acceptance criteria and risks, rather than mirroring implementation details.
- Exercise relevant normal, boundary, and failure behavior. For performance or memory claims, use measurements under stated conditions.
- Run the appropriate type checks, builds, tests, or manual checks. Report what was actually checked and any limits; do not claim unperformed validation.
- Review for correctness, readability, resource handling, compatibility, and scope. Welcome critique and correct mistakes openly.
- Do not expand testing or refactoring without a reason arising from the change or evidence.

## Finish and hand off

An increment is complete when its agreed behavior is implemented, appropriate checks pass, and remaining limitations are explicit.

- Update the authoritative issue with the implemented state, verification evidence, and next step; update a local draft only while no issue exists.
- Keep setup and usage documentation accurate when behavior changes.
- Summarize what changed, why, how it was verified, and any material remaining risks or questions.
- Do not describe partial work as complete or imply that future capabilities already exist.
- Finish the agreed scope thoroughly without expanding it into speculative future work.
