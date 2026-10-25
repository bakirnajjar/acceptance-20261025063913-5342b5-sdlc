# Requirements analysis

`schema_version: 1.0` · `stage: requirements_analysis`

## Source references

- Story: `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}`. Read through `next_offset: null`; no end marker was present.
- Supplied image: `{"source_type":"attachment","source_id":"2c1a39f0-752b-4e40-bef5-6c447dd9e603","locator":"sign-in-wireframe.png"}`. The platform reader returned this reference **without image pixels or text**; it establishes the supplied artifact's existence, not its appearance or any marker.
- Run-matched recovery standard: `{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}`. ACC-STD-01 specifies a “Forgot password?” link, uniform known/unknown-account response and five requests per account per hour; ACC-STD-02 specifies single-use links, 30-minute expiry and sign-out of all active sessions; ACC-STD-03 specifies notifying the owner after every successful reset.

## Requirements

### REQ-001 — Use the supplied sign-in wireframe
**Statement:** The application's sign-in interface conforms to the supplied sign-in wireframe to the extent its contents can be verified.

**Acceptance criteria:** Given the supplied image is made viewable, when a reviewer compares the implemented sign-in interface with that image, then the visible sign-in design conforms to the supplied wireframe; no particular layout, control, label or attachment marker is asserted here without seeing it.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"},{"source_type":"attachment","source_id":"2c1a39f0-752b-4e40-bef5-6c447dd9e603","locator":"sign-in-wireframe.png"}]`

### REQ-002 — Start recovery from sign-in
**Statement:** The sign-in page offers a “Forgot password?” link that starts a password-reset request.

**Acceptance criteria:** Given a user is at sign-in, when they activate the “Forgot password?” link, then they can initiate a password-reset request.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"},{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-003 — Issue a reset link
**Statement:** An allowed password-reset request for a synthetic account issues a reset link usable by that account's owner.

**Acceptance criteria:** Given an existing synthetic account has not exhausted its request limit, when its reset request is accepted, then a reset link for that account is issued and is obtainable through the application's in-memory notification sink for testing; no email provider or delivery channel is prescribed.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"},{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-004 — Avoid account enumeration in request responses
**Statement:** The response to a password-reset request is identical for an existing and a nonexistent account.

**Acceptance criteria:** Given otherwise comparable reset requests for a synthetic account and an unknown account that are not distinguished by an exceeded request limit, when each is submitted, then their requester-visible responses are identical, without revealing which account exists.

**source_references:** `[{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-005 — Limit per-account reset requests
**Statement:** Reset requests are limited to five per account per hour.

**Acceptance criteria:** Given a synthetic account has made five reset requests within a one-hour interval, when a sixth request is made within that same interval, then that additional request cannot issue another reset link for the account. The exact time-window boundary and requester-visible rate-limit behavior remain open.

**source_references:** `[{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-006 — Accept each reset link only once
**Statement:** A reset link is single-use.

**Acceptance criteria:** Given a reset link has already completed one successful password reset, when that link is used again, then the later attempt cannot reset the password.

**source_references:** `[{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-007 — Expire reset links after 30 minutes
**Statement:** A reset link expires 30 minutes after issuance.

**Acceptance criteria:** Given a reset link was issued at a known time, when it is used before 30 minutes have elapsed, then it is eligible for reset (if otherwise valid); when it is used at or after 30 minutes, then it cannot reset the password.

**source_references:** `[{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-008 — Change the account password on successful reset
**Statement:** Completing a reset through a valid link changes the linked synthetic account's password to the submitted new password.

**Acceptance criteria:** Given an unused, unexpired link and a new password accepted by the application, when the account owner completes the reset, then the new password works for that account and the previous password no longer works. No unprovided password policy or sign-in/session implementation is specified.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"},{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-009 — Sign out all active sessions
**Statement:** A successful password reset signs its account out of every active session.

**Acceptance criteria:** Given a synthetic account has multiple active sessions before a successful reset, when that reset completes, then none of those preexisting sessions continues to grant access as that account. The session mechanism is not prescribed.

**source_references:** `[{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-010 — Notify the owner after each successful reset
**Statement:** The account owner is notified after every successful password reset.

**Acceptance criteria:** Given a synthetic account successfully resets its password, when the reset completes, then an owner-directed reset notification is observable in the in-memory sink; repeating the flow successfully produces an owner notification for each successful reset.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"},{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-011 — Use synthetic accounts
**Statement:** The application uses synthetic accounts rather than real account data.

**Acceptance criteria:** Given the application is run and its recovery flow exercised, when its account fixtures and data sources are inspected, then the accounts used for the flow are synthetic and no real account identities are required.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}]`

### REQ-012 — Keep notifications in memory
**Statement:** The application uses an in-memory notification sink.

**Acceptance criteria:** Given a reset flow generates a notification, when its notification handling is exercised, then the notification can be inspected from an in-memory sink without configuring an external delivery service.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}]`

### REQ-013 — Document production limitations
**Statement:** The delivered application documentation identifies its production limitations.

**Acceptance criteria:** Given the application deliverables, when their documentation is reviewed, then production limitations of this synthetic-account, in-memory-notification implementation are stated, without representing it as production-ready.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}]`

### REQ-014 — Provide automated tests
**Statement:** Automated tests cover the password-reset application's specified behavior.

**Acceptance criteria:** Given the delivered application, when its automated tests are run, then they exercise reset initiation and completion, uniform known/unknown request responses, the per-account limit, link single-use and expiry, session sign-out and owner notifications, using synthetic accounts and the in-memory sink; failures in those behaviors fail the corresponding tests.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"},{"source_type":"knowledge","source_id":"99467581-e0f3-4ba3-a177-7343cb1097b5","locator":"v1, chunk 0"}]`

### REQ-015 — Do not deploy
**Statement:** The work does not deploy the application.

**Acceptance criteria:** Given the delivery and its validation activities, when they are reviewed, then they have not deployed anything to an environment or service.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}]`

### REQ-016 — Do not add deployment workflows
**Statement:** The work does not add deployment workflows.

**Acceptance criteria:** Given the repository changes made to deliver the application, when those changes are reviewed, then no deployment workflow was added.

**source_references:** `[{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}]`

## Assumptions

- acceptance-20261025063913-5342b5-skill-sentinel acceptance skill produced this output.
- The complete pinned story has no visible end marker; none is copied or invented.
- The attachment reader exposed only a source reference, not the image or text. No wireframe layout, labels, interactions beyond independently sourced “Forgot password?”, or attachment acceptance marker is assumed.
- For REQ-001, the wireframe is a design target only; visual comparison awaits viewable contents and does not block inventory of the independently supported recovery behavior.
- For REQ-003, a test can inspect a link through the stipulated in-memory sink; the transport or format of an actual user-facing notification is not assumed.
- For REQ-004–005, the same-response comparison uses requests before any limit is exceeded; the exact time-window boundary, presentation of a limited response and treatment of nonexistent-account rate limits are left open.
- For REQ-008, tests use a password that the eventual application accepts; no password rules are inferred.
- For REQ-009, “sign out” means preexisting active sessions cease to authorize the account; no storage, token or session scheme is inferred.

## Open questions

All questions are **non-blocking**; each context states the working assumption.

1. **Question:** What visible layout, labels or acceptance marker, if any, does the supplied wireframe contain? **blocking:** `false`. **Context:** The image reader returned only its source reference. Assumption: assert no unseen details; compare REQ-001 only when image contents are viewable.
2. **Question:** Does “per hour” require a rolling or fixed window, and what requester-visible behavior applies when the limit is reached or the account is unknown? **blocking:** `false`. **Context:** ACC-STD-01 does not specify boundary or treatment. Assumption: test a sixth request within an unambiguous one-hour interval; do not prescribe boundary behavior or nonexistent-account buckets.
3. **Question:** How should users receive reset links and successful-reset notifications outside the test sink? **blocking:** `false`. **Context:** The story specifies an in-memory sink, not an external delivery channel. Assumption: inspect the sink in tests; note its production limitation.
4. **Question:** What are the password acceptance rules and active-session mechanism? **blocking:** `false`. **Context:** The story and standard do not define them. Assumption: test with an accepted new password and the observable invalidation of existing access, not a particular policy or implementation.
