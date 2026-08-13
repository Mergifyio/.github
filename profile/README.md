<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://mergify.com/brand/mergify-logo-white.png">
    <img src="https://mergify.com/brand/mergify-logo-black.png" alt="Mergify" width="240">
  </picture>
</p>

<p align="center">
  <b>Mergify keeps your main branch green.</b><br>
  A GitHub app that queues your approved pull requests, tests each one against the code it will
  actually merge into, and batches them so your CI runs a fraction of the jobs it runs today.
</p>

<p align="center">
  <a href="https://github.com/apps/mergify"><kbd> <br> Install the GitHub App <br> </kbd></a>
  <a href="https://docs.mergify.com"><kbd> <br> Read the docs <br> </kbd></a>
</p>

<p align="center">
  <a href="https://mergify.com">mergify.com</a> ·
  <a href="https://dashboard.mergify.com">Dashboard</a> ·
  <a href="https://mergify.com/blog">Blog</a> ·
  <a href="https://changelog.mergify.com">Changelog</a> ·
  <a href="https://slack.mergify.com">Slack community</a>
</p>

---

## The problem it solves

Your pull request was green when it merged, and main is red now.

That happens because CI tested the pull request against a version of main that no longer existed by
the time it landed. Two changes, each correct on its own, touch the same assumption, and nothing
catches it until they meet.

It gets worse with team size, and faster than headcount does: what matters is how many pairs of
changes are in flight at once, and that grows quadratically. Three engineers can absorb it. Forty
cannot, and somewhere between the two a merge queue stops being optional. We publish what our own
queue data says about where that line falls, in
[State of Merge Queues](https://mergify.com/reports/state-of-merge-queues-2026).

## Try it on one pull request

Install the [GitHub app](https://github.com/apps/mergify), then comment on any open pull request:

```
@mergifyio queue
```

Mergify rebases it on current main, runs the CI you already have against that state, and merges when
it goes green. No config file needed to get that far.

The config file is where you spend less on CI:

```yaml
queue_rules:
  - name: default
    # Test up to 5 pull requests in a single CI run. If the batch fails,
    # Mergify bisects it to find the culprit instead of failing all five.
    batch_size: 5
    batch_max_wait_time: 5 min

    # Two-step CI: cheap checks to get into the queue, the expensive
    # suite once, right before the merge.
    queue_conditions:
      - check-success = quick-tests
    merge_conditions:
      - check-success = full-suite

priority_rules:
  - name: hotfix
    conditions:
      - label = urgent
    priority: high
```

## If you are comparing us to GitHub's merge queue

Most people who land here are. GitHub's queue is genuinely fine for a small team on one fast
workflow. Three of the differences are structural, in that they come from how each queue is built
rather than from a setting either side could turn on. The GitHub column quotes GitHub's own
[merge queue documentation](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue),
read in August 2026.

| | GitHub merge queue | Mergify |
| --- | --- | --- |
| **How many pull requests share a CI run** | One build per queued pull request. The docs are explicit: "Merge limits do not combine `merge_group` builds." | A batch of pull requests validated by one CI run. If the batch fails, Mergify bisects it to find the one bad change instead of failing all of them. |
| **When your checks run** | "Merge queue and pull requests checks are coupled." One set of checks, at one point. | Two gates: cheap checks to enter the queue, the expensive suite once, right before the merge. |
| **What the queue orders** | One first-in, first-out line per protected branch. | Scopes. Independent parts of a monorepo advance in parallel instead of queuing behind each other. |

Longer version, plus a migration guide:
[Mergify vs GitHub merge queue](https://mergify.com/compare/github-merge-queue).

## The rest of it

The queue is what most people arrive for. It shares its data with the rest of the platform, which is
where the interesting parts are:

- **[CI Insights](https://mergify.com/product/ci-insights)** watches your jobs and runners, finds
  the ones that are flaky or slow, and auto-retries jobs that failed for a transient reason.
- **[Test Insights](https://mergify.com/product/test-insights)** classifies every test as healthy,
  flaky or broken, catches new flakiness on the pull request that introduced it, and quarantines the
  known-bad ones so they stop blocking merges.
- **[Merge Protections](https://mergify.com/product/merge-protections)** covers what branch
  protections cannot: conditions on pull request metadata, dependencies between pull requests, merge
  windows, and freezes.
- **[Stacks](https://mergify.com/product/stacks)** splits a branch into one reviewable pull request
  per commit, managed from the CLI with plain git rebase underneath.

## In production

Two customers wrote up how they run it, in their own words:

- **[Cerebras](https://mergify.com/customers/cerebras)** on getting expensive hardware-test runs
  down to one per merge instead of one per push.
- **[Apex Fintech](https://mergify.com/customers/apex)** on merging into a multi-language monorepo
  with speculative branches.

## Our public repos

They are all on the [Repositories tab](https://github.com/orgs/Mergifyio/repositories), and two are
worth pointing at directly: [mergify-cli](https://github.com/Mergifyio/mergify-cli), the `mergify`
binary for stacked pull requests, queue control and config validation, and
[docs](https://github.com/Mergifyio/docs), the source of
[docs.mergify.com](https://docs.mergify.com), where corrections by pull request are welcome.

## Before your security team asks

Mergify is SOC 2 Type II attested. It processes repository contents in memory and does not persist
them; what we store is your configuration and the metadata around a merge, encrypted at rest.
Authentication and permissions are delegated entirely to GitHub, so a user's role in Mergify is
whatever their role is on the repository.

Reports and the sub-processor list are on the [Trust Center](https://trust.mergify.com). The
permission-by-permission breakdown of what the GitHub App asks for, and why, is in the
[security docs](https://docs.mergify.com/security/).

## Elsewhere

We are a small, fully remote team in France.

[Careers](https://careers.mergify.com) ·
[LinkedIn](https://www.linkedin.com/company/mergify/) ·
[X](https://x.com/Mergifyio) ·
[Status](https://status.mergify.com) ·
[security@mergify.com](mailto:security@mergify.com)
