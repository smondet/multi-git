Git: Multi-Repository
=====================

This project provides a couple of scripts which handle multiple Git
repositories at once. One can provide a list of directories to scan for
repositories (non-recursively) on the command line or through Git's
configuration mechanism.

The scripts provided as of now are:

- `git-multi-status`: Show the status of a bunch of Git repositories.
- `git-activity-report`: Make a report of developments in a bunch of Git repositories.
- `git-fetch-all`: Call git fetch a bunch of Git repositories.


It may be interesting for the user to also alias them in
`~/.gitconfig`, for instance:


    [alias]
        mst = multi-status --show-all-extras
        arfd = activity-report --since


See below for detailed usage information ⮷.

Usage: Git-multi-status
-----------------------

Show the status of a bunch of Git repositories.

Use `git multi-status /path/to/repos1 /path/to/repos2` to display
a compact report of all the git repositories found in the folders 
`/path/to/repos1` and `/path/to/repos2`.

Default paths to explore can be set in Git's configuration:

    git config --global --add multi-git.paths /path/to/repos1
    git config --global --add multi-git.paths /path/to/repos2


The table shows 7 columns:

* `Untrk`→ Untracked files.
* `Modf` → Modified files.
* `Ahd`  → Branches ahead of their remote.
* `Bhd`  → Branches behind on their remote.
* `Umr`  → Branches not merged in `HEAD`.
* `Lcl`  → Not-remote-tracking local branches.
* `L/H`  → Not-remote-tracking local branches not merged in `HEAD`.

Options:

* `--show-modified`: Show the list of modified files.
* `--show-ahead`: Show the list of ahead branches.
* `--show-local`: Show the list of local branches.
* `--show-all-extras`: Like all the `--show-*` options together.
* `--no-config`: Do not look at the `multi-git.paths` git-config option.
* `--version`: Show version information.
* `--describe`: Show the status of a bunch of Git repositories


See also `git-multi-status --help`.

Usage: Git-activity-report
--------------------------

Make a report of developments in a bunch of Git repositories.

Use `git activity-report --since 2018-10-31 /path/to/repos1 /path/to/repos2` to display
a detailed “recent happenings” report of all the git repositories found
in the folders `/path/to/repos1` and `/path/to/repos2`.

Default paths to explore can be set in Git's configuration:

    git config --global --add multi-git.paths /path/to/repos1
    git config --global --add multi-git.paths /path/to/repos2


Options:

* `--no-config`: Do not look at the `multi-git.paths` git-config option.
* `--since <string>`: Date to get the logs/information since (default: “last sunday”).
* `--section-base <string>`: The base markdown section ('##', '###', etc. default: ###)
* `--fetch`: Run `git fetch --all` before showing a repository.
* `--version`: Show version information.
* `--describe`: Make a report of developments in a bunch of Git repositories


See also `git-activity-report --help`.

**Current Limitations:**

- Often, there are redundancies between branches that the script does
not detect.

Usage: Git-fetch-all
--------------------

Call git fetch a bunch of Git repositories.

Use `git fetch-all /path/to/repos1 /path/to/repos2` to run
`git fetch --all` in the folders `/path/to/repos1` and `/path/to/repos2`.

Default paths to explore can be set in Git's configuration:

    git config --global --add multi-git.paths /path/to/repos1
    git config --global --add multi-git.paths /path/to/repos2


Options:

* `--no-config`: Do not look at the `multi-git.paths` git-config option.
* `--version`: Show version information.
* `--describe`: Call git fetch a bunch of Git repositories


See also `git-fetch-all --help`.



Authors / Making-of
-------------------

This repository is generated by an OCaml program which itself was
written by [Seb Mondet](https://seb.mondet.org), it uses the
[Genspio](https://smondet.gitlab.io/genspio-doc/) EDSL library, and
serves as one of its examples of usage, see also its
[implementation](https://github.com/hammerlab/genspio/tree/master/src/examples/multigit.ml).

Similarly, you may check out the <https://github.com/smondet/cosc>
repository, which is also a bunch of shell scripts maintained by an
OCaml program.

Example Session / Demo
----------------------

Let's see a sequence of examples to demo the scripts. First, we prepare
a set of *“test”* repositories in `/tmp/git-repos-example`:

    $ mkdir -p /tmp/git-repos-example/hammerlab
    $ mkdir -p /tmp/git-repos-example/smondet
    $ mkdir -p /tmp/git-repos-example/bitbucket
    $ git clone https://github.com/hammerlab/ketrew.git \
      /tmp/git-repos-example/hammerlab/ketrew
    $ git clone https://github.com/hammerlab/biokepi.git \
      /tmp/git-repos-example/hammerlab/biokepi
    $ git clone https://github.com/hammerlab/genspio.git \
      /tmp/git-repos-example/hammerlab/genspio
    $ git clone https://github.com/hammerlab/coclobas.git \
      /tmp/git-repos-example/hammerlab/coclobas
    $ git clone https://gitlab.com/smondet/genspio-doc.git \
      /tmp/git-repos-example/smondet/genspio-doc
    $ git clone https://gitlab.com/smondet/vecosek.git \
      /tmp/git-repos-example/smondet/vecosek
    $ git clone https://bitbucket.org/smondet/nonstd.git \
      /tmp/git-repos-example/bitbucket/nonstd


For now, we haven't changed anything to the repositories so the
“multi-status” is full of zeros (we use the `--no-config` option to
get consistent output w.r.t. users' configuration):

    $ git multi-status --no-config /tmp/git-repos-example/hammerlab \
      /tmp/git-repos-example/smondet /tmp/git-repos-example/bitbucket

````````````````````````````````````````````````````````````````````````ok-output
    #=== /tmp/git-repos-example/hammerlab:================================================
                                             | Untrk | Modf | Ahd | Bhd | Umr | Lcl | L/H |
    GHub::biokepi............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    GHub::coclobas...........................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    GHub::genspio............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    GHub::ketrew.............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    #=== /tmp/git-repos-example/smondet:==================================================
                                             | Untrk | Modf | Ahd | Bhd | Umr | Lcl | L/H |
    GLab::genspio-doc........................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    GLab::vecosek............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    #=== /tmp/git-repos-example/bitbucket:================================================
                                             | Untrk | Modf | Ahd | Bhd | Umr | Lcl | L/H |
    BBkt::nonstd.............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
````````````````````````````````````````````````````````````````````````



The activity-report is, for now, more interesting, and it outputs
directly Markdown:

    $ git activity-report --section-base '####' --since 2018-10-31 \
      --no-config /tmp/git-repos-example/hammerlab \
      /tmp/git-repos-example/smondet /tmp/git-repos-example/bitbucket

> 
> #### In `/tmp/git-repos-example/hammerlab`
> 
> ##### GHub: genspio
> 
> 
> Working tree:
> 
> ````````````````````````````````````````````````````````````````````````````````
> ## master...origin/master
> ````````````````````````````````````````````````````````````````````````````````
> 
> Graph:
> 
> ````````````````````````````````````````````````````````````````````````````````
> *  (origin/sm@git-fetch-all) Qualify known remotes (`git-fetch-all`)
> *  Display `No remotes` when there are no remotes
> *  Improve the multigit README
> *  Improve display of `git-fetch-all`
> *  Add the script `git-fetch-all` to `multigit`
> *  (HEAD -> master, origin/master, origin/HEAD) Merge
> |   `sm@improve-multigit-display` (PR #100)
> *  (origin/sm@improve-multigit-display) Get multigit version-string from
> |   environment
> *  Improve multigi README
> *  Add `--show-all-extras` (`git-multi-status`)
> *  Fix typo in column description
> *  Fix `Git.wrap_string_hack` (when last is a merge)
> *  Add option `--show-local`
> *  Fix display bug (some terminals had trouble)
> *  Add a really-local-branches column
> *  Add option `--show-ahead` to `git-multi-status`
> *  Add the `Lcl` column to the multi-status
> *  Fix both multigit scripts for relative paths
> *  Fix multi-status display on OSX
> *  Improve `README.md` generation
> *  Add help-message about multi-status columns
> *  Update multigit tests
> ````````````````````````````````````````````````````````````````````````````````
> 
> ###### On `HEAD -> master, origin/master, origin/HEAD`
> 
> - Update multigit tests.  
> - Add help-message about multi-status columns.  
> - Improve `README.md` generation.  
> - Fix multi-status display on OSX.  
> - Fix both multigit scripts for relative paths.  
> - Add the `Lcl` column to the multi-status.  
>   This also improves the test, cf. more precise `grep` call.
> - Add option `--show-ahead` to `git-multi-status`.  
> - Add a really-local-branches column.  
> - Fix display bug (some terminals had trouble).  
> - Add option `--show-local`.  
> - Fix `Git.wrap_string_hack` (when last is a merge).  
> - Fix typo in column description.  
> - Add `--show-all-extras` (`git-multi-status`).  
> - Improve multigi README.  
> - Get multigit version-string from environment.  
> - Merge `sm@improve-multigit-display` (PR #100).  
> 
> ###### On `origin/sm@improve-multigit-display`
> 
> - Update multigit tests.  
> - Add help-message about multi-status columns.  
> - Improve `README.md` generation.  
> - Fix multi-status display on OSX.  
> - Fix both multigit scripts for relative paths.  
> - Add the `Lcl` column to the multi-status.  
>   This also improves the test, cf. more precise `grep` call.
> - Add option `--show-ahead` to `git-multi-status`.  
> - Add a really-local-branches column.  
> - Fix display bug (some terminals had trouble).  
> - Add option `--show-local`.  
> - Fix `Git.wrap_string_hack` (when last is a merge).  
> - Fix typo in column description.  
> - Add `--show-all-extras` (`git-multi-status`).  
> - Improve multigi README.  
> - Get multigit version-string from environment.  
> 
> #### In `/tmp/git-repos-example/smondet`
> 
> #### In `/tmp/git-repos-example/bitbucket`


The script `git-fetch-all` is the simplest it just provides a nice
display even when working with ≥ 30 repositories, and with **errors**,
so we are going to add a couple of wrong remotes to spice things up.

    $ git -C /tmp/git-repos-example/hammerlab/ketrew remote add wrong-http \
      https://example.com/dadams/h2g2.git
    $ git -C /tmp/git-repos-example/smondet/vecosek remote add wrong-ssh \
      git@gitlab.com/i-don-t-have-access/to-this.git


And *now,* we call `git-fetch-all`:

    $ git fetch-all --no-config /tmp/git-repos-example/hammerlab \
      /tmp/git-repos-example/smondet /tmp/git-repos-example/bitbucket

````````````````````````````````````````````````````````````````````````ok-output
    >> Repository biokepi: [GHub:origin OK]
    >> Repository coclobas: [GHub:origin OK]
    >> Repository genspio: [GHub:origin OK]
    >> Repository ketrew: [GHub:origin OK][Unkn:wrong-http ERROR]
    >> Repository genspio-doc: [GLab:origin OK]
    >> Repository vecosek: [GLab:origin OK][GLab:wrong-ssh ERROR]
    >> Repository nonstd: [BBkt:origin OK]
    
    ## Errors:
    * Repository `ketrew`, remote `wrong-http`:
        * > fatal: repository 'https://example.com/dadams/h2g2.git/' not found
        * See also `/tmp/multi-fetch-ketrew-wrong-http.log`.
    * Repository `vecosek`, remote `wrong-ssh`:
        * > 
        * > Please make sure you have the correct access rights
        * > and the repository exists.
        * See also `/tmp/multi-fetch-vecosek-wrong-ssh.log`.
````````````````````````````````````````````````````````````````````````



Let's do some modifications:

    $ echo 'This is Great!' >> \
      /tmp/git-repos-example/hammerlab/biokepi/README.md
    $ echo 'More lawyery stuff' >> \
      /tmp/git-repos-example/hammerlab/biokepi/LICENSE
    $ echo 'This is tracked' >> \
      /tmp/git-repos-example/hammerlab/coclobas/README.md
    $ echo 'This is *not* tracked' >> \
      /tmp/git-repos-example/hammerlab/coclobas/NOT-TRACKED.md
    $ echo 'This is Great' >> \
      /tmp/git-repos-example/hammerlab/ketrew/README.md
    $ git -C /tmp/git-repos-example/hammerlab/ketrew/ checkout -b \
      new-branch-that-tracks -t master

````````````````````````````````````````````````````````````````````````ok-output
    M	README.md
    Branch 'new-branch-that-tracks' set up to track local branch 'master'.
````````````````````````````````````````````````````````````````````````

    $ git -C /tmp/git-repos-example/hammerlab/ketrew/ commit -a -m 'Add \
      greatness to the README'

````````````````````````````````````````````````````````````````````````ok-output
    [new-branch-that-tracks 801f28d] Add greatness to the README
     1 file changed, 1 insertion(+)
````````````````````````````````````````````````````````````````````````

    $ git -C /tmp/git-repos-example/hammerlab/ketrew/ checkout -b \
      new-branch-more-local


Now in the multi-status we can see the modified files, the untracked
counts, and one branch is “ahead” (since we used `-t master` while
creating, it has a remote to define it):

    $ git multi-status --show-all-extras --no-config \
      /tmp/git-repos-example/hammerlab /tmp/git-repos-example/smondet \
      /tmp/git-repos-example/bitbucket

````````````````````````````````````````````````````````````````````````ok-output
    #=== /tmp/git-repos-example/hammerlab:================================================
                                             | Untrk | Modf | Ahd | Bhd | Umr | Lcl | L/H |
    GHub::biokepi............................| 0     | 2    | 0   | 0   | 0   | 0   | 0   |
      |- Modified:
      |    - LICENSE
      |    - README.md
    GHub::coclobas...........................| 1     | 1    | 0   | 0   | 0   | 0   | 0   |
      |- Modified:
      |    - README.md
    GHub::genspio............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    GHub::ketrew.............................| 0     | 0    | 1   | 0   | 0   | 1   | 0   |
      |- Ahead: new-branch-that-tracks.
      |- Local: new-branch-more-local.
    #=== /tmp/git-repos-example/smondet:==================================================
                                             | Untrk | Modf | Ahd | Bhd | Umr | Lcl | L/H |
    GLab::genspio-doc........................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    GLab::vecosek............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
    #=== /tmp/git-repos-example/bitbucket:================================================
                                             | Untrk | Modf | Ahd | Bhd | Umr | Lcl | L/H |
    BBkt::nonstd.............................| 0     | 0    | 0   | 0   | 0   | 0   | 0   |
````````````````````````````````````````````````````````````````````````



Let's concentrate the activity-report on
`/tmp/git-repos-example/hammerlab` and on the past 3 days:

    $ git activity-report --no-config --since $(date -d '-3 days' \
      +%Y-%m-%d) /tmp/git-repos-example/hammerlab

````````````````````````````````````````````````````````````````````````ok-output
    
    ### In `/tmp/git-repos-example/hammerlab`
    
    #### GHub: genspio
    
    
    Working tree:
    
    ````````````````````````````````````````````````````````````````````````````````
    ## master...origin/master
    ````````````````````````````````````````````````````````````````````````````````
    
    Graph:
    
    ````````````````````````````````````````````````````````````````````````````````
    *  (origin/sm@git-fetch-all) Qualify known remotes (`git-fetch-all`)
    *  Display `No remotes` when there are no remotes
    *  Improve the multigit README
    *  Improve display of `git-fetch-all`
    *  Add the script `git-fetch-all` to `multigit`
    *  (HEAD -> master, origin/master, origin/HEAD) Merge
        `sm@improve-multigit-display` (PR #100)
    ````````````````````````````````````````````````````````````````````````````````
    
    ##### On `HEAD -> master, origin/master, origin/HEAD`
    
    - Merge `sm@improve-multigit-display` (PR #100).  
    
    #### GHub: ketrew
    
    
    Working tree:
    
    ````````````````````````````````````````````````````````````````````````````````
    ## new-branch-more-local
    ````````````````````````````````````````````````````````````````````````````````
    
    Graph:
    
    ````````````````````````````````````````````````````````````````````````````````
    *  (HEAD -> new-branch-more-local, new-branch-that-tracks) Add greatness
        to the README
    ````````````````````````````````````````````````````````````````````````````````
    
    ##### On `HEAD -> new-branch-more-local, new-branch-that-tracks`
    
    - Add greatness to the README.  
````````````````````````````````````````````````````````````````````````



We can see the new commit in the new branch appears in the report ⮵.

And that's all for today ☺ !

License
-------

The code generator is covered by the Apache 2.0
[license](http://www.apache.org/licenses/LICENSE-2.0), the scripts are
ISC [licensed](https://opensource.org/licenses/ISC).

