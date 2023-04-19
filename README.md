## actions-usage

Find your GitHub Actions usage across a given organisation or user account.

![Example console output](https://pbs.twimg.com/media/FrbYxbwWwAMvQZN?format=jpg&name=large)
> Example console output for the [inlets OSS repos](https://github.com/inlets)

Includes total runtime of all workflow runs and workflow jobs, including where the jobs were run within inclusive, free, billed minutes, or on self-hosted runners.

This data is not available within a single endpoint in GitHub's REST or GraphQL APIs, so many different API calls are necessary to build up the usage statistics.

```
repos = ListRepos(organisation || user)
   for each Repo
       ListWorkflowRuns(Repo)
          for each WorkflowRun
             jobs = ListWorkflowJobs(WorkflowRun)
sum(jobs)
```

If your team has hundreds of repositories, or thousands of builds per month, then the tool may exit early due to exceeding the API rate-limit. In this case, we suggest you run with `--days=10` and multiply the value by 3 to get a rough picture of 30-day usage.

## Usage

This tool is primarily designed for use with an organisation, however you can also use it with a regular user account by changing the `--org` flag to `--user`.

Or create a [Classic Token](https://github.com/settings/tokens) with: repo and admin:org and save it to ~/pat.txt. Create a short lived duration for good measure.

We recommend using arkade to install this CLI, however you can also download a binary from the [releases page](https://github.com/self-actuated/actions-usage/releases).

```sh
# sudo is optional for this step
curl -SLs https://get.arkade.dev | sudo sh

arkade get actions-usage
sudo mv $HOME/.arkade/bin/actions-usage /usr/local/bin/
```

## Output

```bash
actions-usage --org openfaas --token-file ~/pat.txt --days 28

Usage report generated by self-actuated/actions-usage.

Total repos: 44
Total private repos: 0
Total public repos: 44

Total workflow runs: 128
Total workflow jobs: 173

Total users: 4
Longest build: 13m50s
Average build time: 2m55s
Total usage: 8h24m21s (504 mins)
```

As a user:

```bash
actions-usage --user alexellis --token-file ~/pat.txt
```

## Development

All changes must be proposed with an Issue prior to working on them or sending a PR. Commits must have a sign-off message, i.e. `git commit -s`

```bash
git clone https://github.com/actuated/actions-usage
cd actions-usage

go run . --org actuated-samples --token-file ~/pat.txt
```

## Author

This tool was created as part of [actuated](https://actuated.dev) - secure, fast BYO runners for GitHub Actions. actuated is developed by [OpenFaaS Ltd](https://openfaas.com).

Within the dashboard, customers get built-in charts for overall usage on a GitHub organisation and a repo-level break down of passing/failing builds and total time spent in each.

[![Insights for customers](https://pbs.twimg.com/media/FqnJ8rLXgAEnJDZ?format=png&name=medium)](https://twitter.com/alexellisuk/status/1633059062639108096/)

## License

MIT

Contributions are welcome, however an Issue must be raised and approved before submitting a PR.

For typos, raise an Issue and a contributor will fix this. It's easier for everyone that way.

