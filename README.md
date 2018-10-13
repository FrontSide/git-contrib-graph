# Git contrib graph

Displays a github-like contribution graph, of every contributors of a repository: [example](https://github.com/moby/moby).

## Why

I've been developing this tool for getting a fast overview of student involvement in scholar group projects, at [Epitech](http://epitech.eu) (French university).

## Usage

```sh
$ go get github.com/samber/git-contrib-graph

$ git-contrib-graph
Usage of git-contrib-graph:
  -full-graph
    	Display days without contributions
  -git-path string
    	Fetch logs from local git repository (bare or normal)
  -git-remote string
    	Fetch logs from remote git repository Github, Gitlab...
  -interval string
    	Display contributions per day, week or month (default "day")
  -json
    	Display json output contributions object
  -max-columns int
    	Number of columns in your terminal or output (default 80)
  -no-colors
    	Disabled colors in output
```

## Example

Display contributions per week, with color and including weeks with no commit.

```sh
$ git-contrib-graph --git-remote https://github.com/samber/invoice-as-a-service \
	--full-graph \
	--interval month

Repo: https://github.com/samber/invoice-as-a-service

Contributions to master, excluding merge commits:


################################################################################
################################################################################


Author: dev@samuel-berthe.fr

Total:
   31 commits
   Insertions: 7557 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
   Deletions:   127 ------------------------------------------------------------

Per day:
   2018-03-01 | 7063(+) 101(-) +++++++++++++++++++++++++++++++++++++++++++++++++-
   2018-04-01 |   0(+)   0(-)
   2018-05-01 |   0(+)   0(-)
   2018-06-01 |   0(+)   0(-)
   2018-07-01 | 494(+)  26(-) ++++++++++++++++++++++++++++++++++++++++++++++++---
   2018-08-01 |   0(+)   0(-)
   2018-09-01 |   0(+)   0(-)
   2018-10-01 |   0(+)   0(-)


################################################################################
################################################################################


Author: catalin@zmole.ro

Total:
   4 commits
   Insertions:  121 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
   Deletions:    25 -------------------------

Per day:
   2018-03-01 |   0(+)   0(-)
   2018-04-01 |   0(+)   0(-)
   2018-05-01 |   0(+)   0(-)
   2018-06-01 |   0(+)   0(-)
   2018-07-01 |   0(+)   0(-)
   2018-08-01 |   0(+)   0(-)
   2018-09-01 |   0(+)   0(-)
   2018-10-01 | 121(+)  25(-) +++++++++++++++++++++++++++++++++++++++++---------


################################################################################
################################################################################


Author: mkingbe@gmail.com

Total:
   1 commits
   Insertions:  386 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
   Deletions:   359 ------------------------------------------------------------

Per day:
   2018-03-01 |   0(+)   0(-)
   2018-04-01 |   0(+)   0(-)
   2018-05-01 |   0(+)   0(-)
   2018-06-01 |   0(+)   0(-)
   2018-07-01 |   0(+)   0(-)
   2018-08-01 |   0(+)   0(-)
   2018-09-01 |   0(+)   0(-)
   2018-10-01 | 386(+) 359(-) ++++++++++++++++++++++++++------------------------

```

## Json Example

Display contributions per week, with color and including weeks with no commit, as JSON object.

```sh
$ git-contrib-graph --git-remote https://github.com/samber/invoice-as-a-service --json

{"interval": "day", "contributors": [{"author": "catalin@zmole.ro", "total": {"commits": 4, "insertions":  121, "deletions": 25}, "graph": []}, {"author": "mkingbe@gmail.com", "total": {"commits": 1, "insertions":  386, "deletions":  359}, "graph": [{"date": "2018-10-03", "add": 121, "sub": 25 }]}, {"author": "dev@samuel-berthe.fr", "total": {"commits": 31, "insertions": 7557, "deletions":  127}, "graph": [{"date": "2018-10-03", "add": 121, "sub": 25 }, {"date": "2018-10-02", "add": 386, "sub": 359 }]}]}

```

## Docker

Remote repository:

```
docker run --rm samber/git-contrib-graph \
       --git-remote https://github.com/samber/invoice-as-a-service \
       --interval week --full-graph
```

Local repository:

```
docker run --rm \
       -v /students/john-doe/project-a-b-c:/repo \
       samber/git-contrib-graph \
       --git-path /repo --interval week  --full-graph
```

## Todo

- image output
- custom branch
- clone on filesystem instead of memory
- test with lot of public github repo and compare contrib graphs
- better example in readme
- get graph for only one contributor

## Contributing

⚠ Quick and dirty project ;)

### Run

```sh
$ go run git-contrib-graph.go --git-remote https://github.com/samber/invoice-as-a-service \
	--full-graph \
	--max-columns 100 \
	--interval week

...
```

### About git api and go-git library

Git log command do much more things than you think !

- At low level, initial git commit does not have diff (because it has no parent). Then we fetch number of changes (add/del) based on number of lines.
- Merge commits have 2 parents. In that case, changes (add/del) are displayed once in `git log` command, but are provided twice in git api (in both original and merge commit)
