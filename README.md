# gh-org-clone

A github-cli extension script to clone all repositories in an organization, optionally filtering by topic or a search string. If the repository has already been cloned it will attempt to switch to the default branch and pull.

## Installation

```
$ gh extension install crguezl/gh-org-clone
```

## Usage

```txt
Usage: gh org-clone [options] [organization] [options]

Options:
  -V, --version                              output the version number
  -s, --search <query>                       search <query> using GitHub Search API. A dot '.' refers to all the repos
  -r, --regexp <regexp>                      filter <query> results using <regexp>
  -c, --csr <comma separated list of repos>  the list of repos is specified as a comma separated list
  -f, --file <file>                          file with the list of repos, one per line
  -n --dryrun                                just show what repos will be added as submodules
  -o --org <org>                             default organization or user
  -D --depth <depth>                         Create a shallow clone with a history truncated to <depth> number of commits
  -p --parallel <int>                        number of concurrent  processes during the cloning stage (default: 2)
  -h, --help                                 display help for command

  - If the organization is not explicitly specified the selection will be done interactively among the list of your organizations
  - You can set the default organization through the GITHUB_ORG environment variable
  - If no repos are specified the selection of repos will be done interactively among the repos in the org
  - Option '-s' assumes all the repos belong to the same org
```

## Examples of use

```
$ gh org-clone -s iaas -n ULL-ESIT-DMSI-1920
Retrieving the list of repositories: search/repositories?q=org%3AULL-ESIT-DMSI-1920%20iaas
This would have cloned the following 6 repositories to /Users/casianorodriguezleon/campus-virtual/2021/learning/gh-learning/gh-org-clone:
ULL-ESIT-DMSI-1920/p1-t1-iaas-alu0101232812
ULL-ESIT-DMSI-1920/p1-t1-iaas-alu0100592306
ULL-ESIT-DMSI-1920/p1-t1-iaas-fuegonellaa
ULL-ESIT-DMSI-1920/p1-t1-iaas-Wololegend
ULL-ESIT-DMSI-1920/p1-t1-iaas-crguezl
ULL-ESIT-DMSI-1920/p1-t1-iaas-lauramanzini
```

### Default org with GITHUB_ORG

```
$ export GITHUB_ORG=ULL-ESIT-DMSI-1920
$ gh org-clone -s fuegonella  -n
Retrieving the list of repositories: search/repositories?q=org%3AULL-ESIT-DMSI-1920%20fuegonella
This would have cloned the following 3 repositories to /Users/casianorodriguezleon/campus-virtual/2021/learning/gh-learning/gh-org-clone:
ULL-ESIT-DMSI-1920/markdown-fuegonellaa
ULL-ESIT-DMSI-1920/pb-gh-campus-expert-fuegonellaa
ULL-ESIT-DMSI-1920/p1-t1-iaas-fuegonellaa
```

