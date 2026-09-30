# Evidence guide: where proof lives in a reproduction package

This guide is the rubric's map. For each family of proof a check in
`rubric.md` names, it says where to look and what good looks like.

How an eval bundle is laid out (every family below refers to these
sections by name):

| Bundle section | What it holds |
|---|---|
| Header (`Eval package: ...`) | Package id, source issue, capture date. |
| **Repo facts** | Stars, archived flag, latest release and date, bug-report template fields, contribution policy (including any AI-disclosure rule). |
| **Issue** | Title, author, open date, labels, body: behavior, numbered steps, `Environment:` line. |
| **Thread highlights** | Later comments, including other people claiming or fixing it. |
| **Candidate claim comment** | The student's claim text. |
| **Candidate repro report** | The student's `Environment:` line, `Steps and observed:` block (commands plus output), `Expected:` and `Actual:` lines. |

In live mode the same things live in: the issue body and thread on
GitHub (issue side); the repo's README, `CONTRIBUTING.md`,
`.github/ISSUE_TEMPLATE/`, and Releases page (repo facts); and the
student's draft files (candidate side). Only the draft text counts as
the package. Files next to it that the drafts don't quote don't count.

---

## Environment

**Where it lives**

- Issue side: the `Environment:` line in the **Issue** body (live: the
  issue body, usually under the fields the bug template asks for).
  Also check **Repo facts** → bug-report template for the fields the
  repo expects (for example tool version, dependency version, OS).
- Candidate side: the `Environment:` line at the top of the
  **Candidate repro report**, plus any environment mention in the
  **Candidate claim comment** (for example "reproduced on the latest
  release on Linux").
- Reference point: **Repo facts** → latest release, to tell whether a
  version difference is "newer than the issue" or "not a real
  release".

**What good looks like**

- The report names each of the following, with a version where the
  thing has one:
  1. the OS, with distro or platform and architecture if the issue
     gives them,
  2. the tool under test and the build it came from (release binary,
     source at a commit, package manager),
  3. every dependency the issue's environment line names.
- For each item, the value either matches the issue's value or the
  package says outright that it differs (for example "issue filed on
  X; this is Y", "on a different OS and install method"). A difference
  that is only there to be noticed by comparing the two lines does not
  count as stated.
- Differing is fine; hiding it is not. A newer release, another OS, or
  another install method is a normal reproduction environment once it
  is named. The dangerous silent case is an OLDER version or a
  different build channel than the issue's (for example an old major
  version against an issue confirmed on latest and main): the output
  then says nothing about the reported bug.
- If the issue states no version, the tool's current release (see
  **Repo facts** → latest release) needs no statement.
- Bug-template fields beyond OS and tool version (for example full
  `conda list` output) are nice to have; their absence alone does not
  fail the environment record.
- If the issue names an architecture or platform detail (for example
  arm64, a mobile terminal, a container), the report names its own
  value for the same detail.

---

## Steps

**Where it lives**

- Issue side: the numbered `Steps:` in the **Issue** body. Also read
  the prose around them for starting-state conditions (for example "in
  a repo with no other tracked changes").
- Candidate side: the `Steps and observed:` block in the **Candidate
  repro report**. Shell lines (`$ ...`) are steps, and `#` comments on
  them describe the interactive actions a shell can't record (keys
  pressed in a TUI, text typed into a prompt).

**What good looks like**

- **Starting state is built, not assumed.** A stranger can go from
  nothing to the trigger by following the steps in order: create or
  clone the repo, set up any files, launch the tool, perform the
  trigger. No step says "set up a repo like in the issue".
- **Same starting state as the issue.** Line up each issue condition
  against a report step. The report's setup must meet every
  condition, and must not add conditions the issue didn't have.
  Watch for setups that bring in their own failure mode. For example,
  a freshly initialized repo with no commits is a different starting
  state from "a repo with no other tracked changes", and some
  commands fail in an empty repo for unrelated reasons (`git stash`
  needs an initial commit). A reproduction built on a different
  starting state is an adjacent reproduction, even if it ends in the
  same symptom.
- **Same trigger as the issue.** Same command, key, or input sequence,
  in the same panel or context. Bypassing the code path the issue is
  about (for example running the underlying CLI command directly
  instead of going through the tool's UI) is adjacent, and so is
  changing the input so it fails for a different reason (a different
  range syntax, an expression edited until it no longer compiles, `:`
  where the issue used `=`).
- **Minimal re-creation is not adjacency.** Shrinking the issue's
  input while keeping every condition the issue names as triggering is
  a faithful reproduction: a local `env.yml` with the same invalid
  section instead of the issue's remote URL, `--offline` to print the
  same request without the network, a fresh playground with the
  issue's exact code. Ask: does this still exercise the same code on
  the same kind of input? If yes, it is faithful.
- **Re-runnable by a stranger.** Every input is shown or built in the
  steps. A repro that lives in a private repo or depends on an
  unshared config cannot be followed, however convincing its output.
- **Interactive steps are stated precisely.** Say which item was
  focused, which key was pressed, and what was typed. "Tried to
  stash" is not a step.

---

## Behavior shown

**Where it lives**

- Issue side: the symptom sentences in the **Issue** body (what the
  reporter saw, and what did *not* happen), and the last of the
  issue's steps.
- Candidate side: the output lines inside `Steps and observed:` (the
  text printed under each `$` command), then the `Expected:` and
  `Actual:` lines.

**What good looks like**

- **List the issue's observable claims first.** Break the issue's
  symptom into individual checkable facts, then find the artifact that
  backs each one. Every claim needs its own artifact.
- **Shown, not described.** An artifact is command output, a log
  excerpt, or a screenshot/recording that appears in the report. An
  `Actual:` sentence that says what happened is a description. It
  only counts when it points to an artifact above it that shows the
  same thing.
- **The issue's behavior, not a nearby one.** The artifact must show
  the specific failure the issue reports, from the issue's trigger. A
  different error, the same symptom reached a different way, or a
  paraphrase of the output does not count. A graceful validation
  error is not the issue's crash; garbled output with the program
  still alive is not a crash. When the issue quotes output, the
  report's output matches it apart from local paths, timestamps,
  file names, and generated ids or hashes (for example a different
  `data-v-xxxx` scope id).
- **A control run strengthens the artifact.** A second run that
  differs only in the triggering condition and behaves correctly
  (English-first vs Japanese-first, zero headers vs one) shows the
  output is specific to the bug. Helpful, not required.
- **Proving an absence.** When the issue's symptom is that something
  didn't happen (no stash, no message), the proof is a command whose
  empty or unchanged output shows the absence, run after the trigger.
  UI-only absences (no toast appeared) can only be shown with a
  screenshot or recording. Otherwise they are described, not shown.
- **The artifact isolates the bug.** If the starting state has its own
  failure mode (see Steps), an empty or unchanged output can't tell
  the issue's bug apart from that failure. An artifact that is only
  consistent with the issue, and not specific to it, doesn't show it.

---

## Honesty

**Where it lives**

Wherever the package makes a claim: the **Candidate claim comment**
(for example "Reproduced on...", "so it is not X-specific") and the
`Actual:` line and any conclusions in the **Candidate repro report**.
Back each one against the Steps and Behavior-shown evidence above.

**What good looks like**

- **Every claim traces to evidence in the package.** "Reproduced"
  needs steps and artifacts that meet the Steps and Behavior-shown
  bars. "Not platform-specific" needs a run on a second platform, and
  one different platform with a different version and a different
  starting state does not show that. Generalizing beyond what was run
  is overclaiming.
- **Differences are disclosed, not smoothed over.** Version, platform,
  or setup differences from the issue are named in the report, not
  left for the reader to find. "Same behavior here" is only honest if
  the same behavior was shown.
- **Cannot-reproduce is a valid, honest outcome.** A report that says
  "followed the issue's steps on X; could not reproduce; here is what
  happened instead", with artifacts, is honest and can be ready to
  post. A report that claims a repro its artifacts don't show is not.
- **No invented output.** Output blocks are literal copies. A block
  that looks edited, merged across runs, or doesn't match the command
  it sits under fails.

---

## Comms

**Where it lives**

- Claim against the issue: the **Candidate claim comment** read
  against the **Issue** body and every entry in **Thread highlights**
  (live: the full issue thread, plus any linked PRs in the issue's
  sidebar/timeline).
- Comments against the repo: **Repo facts** → bug-report template
  (fields the report must cover) and contribution policy (who reviews
  outside PRs, how selectively, any AI-use disclosure requirement).
  Live: `CONTRIBUTING.md`, `.github/ISSUE_TEMPLATE/`,
  `.github/PULL_REQUEST_TEMPLATE.md`, and any `AI`/`LLM` policy file
  or section.

**What good looks like**

- **The claim reads the thread.** If someone in the thread has said
  they are working on it or have "pushed a fix", the claim mentions
  it and asks before duplicating work (for example "I see @x
  mentioned a fix on <date>; is that still in progress?"). A claim
  that ignores competing work in the thread isn't ready. (In live
  mode, apply `scope.md` house rules: classmates' claims do not
  block.)
- **The claim fits the repo's contribution policy.** If maintainers
  review outside PRs selectively, the claim asks whether a PR is
  welcome, or offers the repro as the contribution, instead of
  announcing a PR. If the repo requires AI-use disclosure, both
  comments include it. If it doesn't, the comment still must not
  misrepresent how the work was done.
- **Specific, not boilerplate.** The claim names the exact behavior
  and a concrete plan tied to the code or UI in question. "I'd like to
  work on this!" with nothing issue-specific is boilerplate.
- **The report follows the repo's template.** Every field the bug
  template asks for is present in the repro report, in terms a
  maintainer can check.
- **No promises the package can't back.** No dates, no "fix coming
  soon", no claims of a root cause the report didn't find.

---

## Disclosure

**Where it lives**

- Policy side: **Repo facts** → contribution policy (live:
  `CONTRIBUTING.md`, `AI_POLICY.md`, or an "AI usage" section).
- Candidate side: the **Candidate claim comment** and **Candidate
  repro report**, looking for a sentence that names the AI tool or
  assistance and its extent.

**What good looks like**

- Every course package is AI-assisted work, so the question is only
  what the repo's policy asks for.
- **Disclosure required** when the policy asks contributors to
  disclose AI usage in comments or issues, or "all AI usage in any
  form". Then a comment without a disclosure sentence fails, however
  good the reproduction is. Example of a pass: "Per the AI usage
  policy: I used an AI assistant to help organize this report; I ran
  and verified every step myself."
- **No disclosure required** when the policy has no AI rule, is
  permissive ("AI tools welcome; you are responsible"), asks for
  disclosure only in pull requests, or asks only that comments be in
  the contributor's own words. A human-voiced comment with no
  disclosure sentence passes there.
