<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://mergify.com/brand/mergify-logo-white.png">
    <img src="https://mergify.com/brand/mergify-logo-black.png" alt="Mergify" width="240">
  </picture>
</p>

<h3 align="center">Keep every merge green.</h3>

<p align="center">
  Mergify covers the path from pull request to green main branch: a merge queue that tests each
  change against the code it will merge into, CI Insights for slow and failing jobs, and Test Engine
  for flaky tests.
</p>

<p align="center">
  <a href="https://dashboard.mergify.com"><kbd> <br> Get started <br> </kbd></a>
  <a href="https://docs.mergify.com"><kbd> <br> Documentation <br> </kbd></a>
  <a href="https://mergify.com/contact"><kbd> <br> Talk to our team <br> </kbd></a>
</p>

<p align="center">
  <a href="https://mergify.com">Website</a> ·
  <a href="https://mergify.com/pricing">Pricing</a> ·
  <a href="https://mergify.com/customers">Customers</a> ·
  <a href="https://changelog.mergify.com">Changelog</a> ·
  <a href="https://mergify.com/blog">Blog</a> ·
  <a href="https://status.mergify.com">Status</a>
</p>

---

## Products

Every plan includes all three.

### [Merge Queue](https://mergify.com/product/merge-queue)

Mergify tests each pull request on top of the latest main and the pull requests queued ahead of it,
and merges it only when CI passes on that combined state. Two changes that each pass CI on their own
can no longer break main together.

- **Batching with bisection.** One CI run validates several pull requests. When a batch fails,
  Mergify splits it to find the change responsible.
- **Two-step CI.** Fast checks decide which pull requests enter the queue, and the full suite runs
  once, right before the merge.
- **Scopes.** Independent parts of a monorepo merge in parallel lanes, defined from your build graph
  or from file patterns.
- **Merge control.** Scheduled freezes, `Depends-On:` dependencies across repositories,
  `Merge-After:` scheduled merges, and GitHub stacked pull requests landed in order.

Coming from GitHub's merge queue? The
[comparison](https://mergify.com/compare/github-merge-queue) covers the differences and the
migration.

### [CI Insights](https://mergify.com/product/ci-insights)

CI Insights tracks every CI job and runner, shows which ones are slow, failing or flaky, and
automatically retries jobs that failed for a transient reason.

### [Test Engine](https://mergify.com/product/test-engine)

Test Engine runs inside the test framework you already use, including pytest, Jest, RSpec, Vitest,
Playwright and JUnit. It reruns the tests a pull request adds or changes to catch flaky ones before
they merge, and quarantines known-flaky tests so their failures stop blocking merges.

## Deployment

- **Mergify Cloud** is operated by Mergify. Install the GitHub App and the queue is running.
- **[Mergify Enterprise Server](https://mergify.com/solutions/enterprise-server)** runs the full
  product on your own infrastructure, with GitHub Enterprise Server or GitHub.com, for teams whose
  code cannot leave their network.

## Get started

1. Install the [Mergify GitHub App](https://github.com/apps/mergify).
2. Comment `@mergifyio queue` on an open pull request. Mergify updates it on top of main, runs your
   existing CI and merges it once CI passes. No configuration file is needed for this step.
3. Add a `.mergify.yml` to tune the queue. This one batches up to five pull requests per CI run and
   keeps the expensive suite for the final check:

```yaml
queue_rules:
  - name: default
    batch_size: 5
    batch_max_wait_time: 5 min
    queue_conditions:
      - check-success = quick-tests
    merge_conditions:
      - check-success = full-suite
```

The [documentation](https://docs.mergify.com) covers every option, and `mergify config validate`
from the [Mergify CLI](https://docs.mergify.com/cli/) checks a configuration before it lands.

## Customers

More than 2,000 organizations and 25,000 users work with Mergify every day, across 300,000
repositories. Engineering teams at [Mozilla](https://mergify.com/customers/mozilla),
[Elastic](https://mergify.com/customers/elastic), [Cerebras](https://mergify.com/customers/cerebras),
[Back Market](https://mergify.com/customers/back-market) and
[PayFit](https://mergify.com/customers/payfit) describe how they run it in their own words, and the
rest are on the [customer stories](https://mergify.com/customers) page.

## Security and compliance

Mergify is SOC 2 Type II attested and does not store repository contents: for a git operation such
as a rebase, it clones the repository into a temporary directory and deletes it when the operation
finishes. Configuration and metadata are encrypted at rest with AES-256 and in transit with TLS 1.2
or higher. Authentication and permissions are delegated to GitHub, so a user's role in Mergify is
their role on the repository.

- [Trust Center](https://trust.mergify.com): audit reports and the sub-processor list.
- [Security documentation](https://docs.mergify.com/security/): data handling and each permission
  the GitHub App requests.
- [HackerOne](https://hackerone.com/mergify): vulnerability reports, through the bug bounty program.
- [security@mergify.com](mailto:security@mergify.com): Data Processing Addendum requests.

## Support

- [Documentation](https://docs.mergify.com) for setup guides and the configuration reference.
- [support@mergify.com](mailto:support@mergify.com), or **Help Center › Contact us** in the
  [dashboard](https://dashboard.mergify.com), for anything that blocks you.
- [Mergifyio/mergify](https://github.com/Mergifyio/mergify) for public bug reports and feature
  requests.
- [Slack community](https://slack.mergify.com) for questions to the team and other users.
- [status.mergify.com](https://status.mergify.com) for availability and incident history.

## Integrations

Mergify publishes its command line and CI integrations from this organization:

| Repository | Purpose |
| --- | --- |
| [mergify-cli](https://github.com/Mergifyio/mergify-cli) | The `mergify` command: stacked pull requests, merge queue status, test result uploads and configuration validation. Holds the releases and the issue tracker. |
| [setup-cli](https://github.com/Mergifyio/setup-cli) | GitHub Action that installs a pinned version of the CLI. |
| [gha-mergify-ci](https://github.com/Mergifyio/gha-mergify-ci) | GitHub Action that uploads JUnit results and detects merge queue scopes. |
| [mergify-ci-buildkite-plugin](https://github.com/Mergifyio/mergify-ci-buildkite-plugin) | The same for Buildkite. |
| [mergify-pre-commit](https://github.com/Mergifyio/mergify-pre-commit) | pre-commit hook that validates Mergify configuration files. |

## About Mergify

Mergify has run merge queues in production since 2018. It is built by a small remote team across
France and the Netherlands.

[About](https://mergify.com/about) ·
[Careers](https://careers.mergify.com) ·
[LinkedIn](https://www.linkedin.com/company/mergify/) ·
[X](https://x.com/Mergifyio)
