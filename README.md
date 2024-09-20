# gh-org-clone

A github-cli extension script to clone all repositories in an organization, optionally filtering by topic or a search string. If the repository has already been cloned it will attempt to switch to the default branch and pull.

## Installation

Yo can install the extension from the [original repository](https://github.com/crguezl/gh-org-clone/tree/main):
```
$ gh extension install crguezl/gh-org-clone
```

or you can install the [synced version at the GitHub CLI for Education organization](https://github.com/gh-cli-for-education/gh-org-clone):

```
$ gh extension install gh-cli-for-education/gh-org-clone
```

It is convenient to have installed [fzf](https://github.com/junegunn/fzf/blob/master/README.md)

## Requirements

Node.js has to be installed.
To have fzf installed is convenient but not essential.

## Usage

```
Usage: gh org-clone [options] [organization] [options for git clone]

Usage: gh org-clone [options] [organization] [options for git clone]

Options:
  -V, --version                              Output the version number
  -s, --search <query>                       Search <query> using GitHub Search API
  -r, --regexp <regexp>                      Filter <query> results using <regexp>
  -c, --csr <comma separated list of repos>  The list of repos is specified as a comma separated list
  -f, --file <file>                          File with the list of repos, one per line
  -k, --fork <value>                         Include fork repos. Legal values: "true", "only", "false". Default is
                                             "true"
  -n --dryrun                                Just show what repos will be cloned
  -o --org <org>                             Use as organization
     --default                               Set "org" as default organization for future uses
  -D --depth <depth>                         Create a shallow clone with a history truncated to <depth> number of
                                             commits
  -p --parallel <int>                        Maximum number of concurrent  processes during the cloning stage (default: 2)
  -h, --help                                 display help for command

  - If the organization is not explicitly specified or there is a default org, 
    the selection will be done interactively among the list of your organizations
  - You can set the default organization through the "--default" option for future uses of this program
  - If no repos are specified the selection of repos will be done interactively among the repos in the org 
  - Option '-s' assumes all the repos belong to the same org
  - When called with option  '-s .', the dot '.' refers to all the repos.  fzf will be open to select the repos
  - Not conflicting "git clone" options as "--recurse-submodules" can be passed as additional options
  - When in fzf, use CTRL-A to select all, tab to select/deselect
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

### Default org with --default

```
➜  iaas gh org-clone --default -s fuegonella ULL-ESIT-DMSI-1920
cloning with 2 concurrent processes ...
[1] Clonando en 'pb-gh-campus-expert-fuegonellaa'...
[0] Clonando en 'markdown-fuegonellaa'...
[1] warning: Pareces haber clonado un repositorio sin contenido.
[1] git clone https://github.com/ULL-ESIT-DMSI-1920/pb-gh-campus-expert-fuegonellaa.git  exited with code 0
[0] git clone https://github.com/ULL-ESIT-DMSI-1920/markdown-fuegonellaa.git  exited with code 0
true
```

The variable `current-org` has been set inside the gh configuration for later uses:

```
➜  iaas gh config get current-org
ULL-ESIT-DMSI-1920
```

## Default Org Alias

I use this extension in combination with these two alias:

```
$ gh alias set cd '!gh config set current-org "$1" 2>/dev/null'
$ gh alias set pwd !'gh config get current-org'
```

Example of use: 

```
$ gh cd ULL-MII-SYTWS-2122
$ gh pwd
ULL-MII-SYTWS-2122
$ gh cd ''
$ gh pwd
```

## Forked repositories

In 2024 GitHub Classroom changed the way student repositories are created for
assignments. Now they are created change lead to the introduction of the `-k  -fork` option. 

Here follows an example. I f you use `-k only` only the forked repositories will be cloned:

```
➜  gh-org-clone git:(main) ✗ gh pwd
ULL-ESIT-DMSI-2425
➜  gh-org-clone git:(main) ✗ ./gh-org-clone -nk only -s aprender-markdown
ULL-ESIT-DMSI-2425/aprender-markdown-aaron-ramirez-valencia-alu0101438238
ULL-ESIT-DMSI-2425/aprender-markdown-casiano-rodriguez-leon-alu0100291865
ULL-ESIT-DMSI-2425/aprender-markdown-alejandro-melian-lemes-alu0101443126
```
By default the `-k` option is set to `true` and all repositories are cloned:

```
➜  gh-org-clone git:(main) ✗ ./gh-org-clone -n -s aprender-markdown 
ULL-ESIT-DMSI-2425/ull-esit-dmsi-2425-aprender-markdown-aprender-markdown-template
ULL-ESIT-DMSI-2425/aprender-markdown-prueba
ULL-ESIT-DMSI-2425/aprender-markdown-aaron-ramirez-valencia-alu0101438238
ULL-ESIT-DMSI-2425/aprender-markdown-casiano-rodriguez-leon-alu0100291865
ULL-ESIT-DMSI-2425/aprender-markdown-alejandro-melian-lemes-alu0101443126
```