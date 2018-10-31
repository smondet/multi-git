Git: Multi-Repository
=====================

This project provides a couple of scripts which handle multiple Git
repositories at once. One can provide a list of directories to scan for
repositories (non-recursively) on the command line or through Git's
configuration mechanism.

The scripts provided as of now are:

- `git-multi-status`: Show the status of a bunch of Git repositories.
- `git-activity-report`: Make a report of developments in a bunch of Git repositories.


It may be interesting for the user to also alias them in
`~/.gitconfig`, for instance:


    [alias]
        mst = multi-status --show-modified
        arfd = activity-report --since


See below for detailed usage information ⮷.

Usage: Git-multi-status
-----------------------

See `git-multi-status --help`:

```
usage: git-multi-status <args>
Show the status of a bunch of Git repositories.

Use `git multi-status /path/to/repos1 /path/to/repos2` to display
a compact report of all the git repositories found in the folders 
`/path/to/repos1` and `/path/to/repos2`.

Default paths to explore can be set in Git's configuration:

    git config --global --add multi-git.paths /path/to/repos1
    git config --global --add multi-git.paths /path/to/repos2


Options:

* `--show-modified`: Show the list of modified files.
* `--no-config`: Do not look at the `multi-git.paths` git-config option.
* `--version`: Show version information.
* `--describe`: Show the status of a bunch of Git repositories
```

Usage: Git-activity-report
--------------------------

See `git-activity-report --help`:

```
usage: git-activity-report <args>
Make a report of developments in a bunch of Git repositories.

Use `git activity-report --since 2018-10-23 /path/to/repos1 /path/to/repos2` to display
a detailed “recent happenings” report of all the git repositories found
in the folders `/path/to/repos1` and `/path/to/repos2`.

Default paths to explore can be set in Git's configuration:

    git config --global --add multi-git.paths /path/to/repos1
    git config --global --add multi-git.paths /path/to/repos2


Options:

* `--no-config`: Do not look at the `multi-git.paths` git-config option.
* `--since <string>`: Date to get the logs/information since (default: “last sunday”).
* `--section-base <string>`: The base markdown section ('##', '###', etc. default: ###)
* `--version`: Show version information.
* `--describe`: Make a report of developments in a bunch of Git repositories
```

**Current Limitations:**

- The script does not guess the “default branch” and has a
preference for the `master` branch (it shows more information for
branches that are *not merged* in `master`).

- Sometimes there are redundancies between branches that the script
does not detect.



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
    $ mkdir -p /tmp/git-repos-example/tezos
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
    $ git clone https://gitlab.com/tezos/tezos.git \
      /tmp/git-repos-example/tezos/tezos


For now, we haven't changed anything to the repositories so the
“multi-status” is full of zeros (we use the `--no-config` option to
get consistent output w.r.t. users' configuration):

    $ git multi-status --no-config /tmp/git-repos-example/hammerlab \
      /tmp/git-repos-example/smondet /tmp/git-repos-example/tezos

````````````````````````````````````````````````````````````````````````ok-output
    #=== /tmp/git-repos-example/hammerlab:=======================================
                                             | Untrk | Modf | Ahd | Behd | Umrg |
    GHub::biokepi........................... | 0     | 0    | 0   | 0    | 0    |
    GHub::coclobas.......................... | 0     | 0    | 0   | 0    | 0    |
    GHub::genspio........................... | 0     | 0    | 0   | 0    | 0    |
    GHub::ketrew............................ | 0     | 0    | 0   | 0    | 0    |
    #=== /tmp/git-repos-example/smondet:=========================================
                                             | Untrk | Modf | Ahd | Behd | Umrg |
    GLab::genspio-doc....................... | 0     | 0    | 0   | 0    | 0    |
    GLab::vecosek........................... | 0     | 0    | 0   | 0    | 0    |
    #=== /tmp/git-repos-example/tezos:===========================================
                                             | Untrk | Modf | Ahd | Behd | Umrg |
    GLab::tezos............................. | 0     | 0    | 0   | 0    | 0    |
````````````````````````````````````````````````````````````````````````



The activity-report is, for now, more interesting, and it outputs
directly Markdown:

    $ git activity-report --section-base '####' --since 2018-10-20 \
      --no-config /tmp/git-repos-example/hammerlab \
      /tmp/git-repos-example/smondet /tmp/git-repos-example/tezos

> 
> #### In `/tmp/git-repos-example/hammerlab`
> 
> ##### GHub: genspio
> 
> ```
> * 95d94bb (origin/sm@improve-multigit-display) Fix README generation
> * ce0ea82 Improve display of multi-status table
> * 746c0ef Improve display of the activity report
> * f499d0b Fix git alias in multigit documentation
> * 241dbc0 Improve multi-status display
> *   f28085d (HEAD -> master, origin/master, origin/HEAD) Merge `sm@multigit-readme` (PR #99)
> |\  
> | * 417432a (origin/sm@multigit-readme) Add blob about limitations of activity-report
> | * e7a76cb Fix typo in multi-git README
> | * 2c9f192 Add example/demo to multi-git's `README.md`
> | *   d26da12 Merge `master` into `sm@multigit-readme`
> | |\  
> | |/  
> |/|   
> * |   59ccac7 Merge `sm@add-activity-report` (PR #98, @smondet)
> |\ \  
> | | * 0b6daf6 Add an example session
> | | * 09f3fd4 Add `README.md` generation for mutli-git
> | |/  
> | * 5274995 (origin/sm@add-activity-report) Add more tests of `git-activity-report`
> | * 33065aa Improve markdown output of `git-activity-report`
> | * b3f9f43 Compute default `--since` to “Last Sunday”
> | * c53b2fa Add `--section-base` to `git-activity_report`
> | *   cbf5ce2 Merge `master` into `sm@add-activity-report`
> | |\  
> | |/  
> |/|   
> * |   5fc3628 Merge `sm@add-multigit-example` (PR #97)
> |\ \  
> | | * 7d9b321 Add new multi-git script: `git-activity-report`
> | |/  
> | * 7308de2 (origin/sm@add-multigit-example) Add support for git-config and improve help
> | * 5efe779 Add multigit to the literally documented examples
> | * ecc5b24 Put descriptive comments in `git-multi-status`
> | * 43a98de Display repository provider (`git multi-status`)
> | * 28a8b55 Add option `--show-modified` (`git multi-status`)
> | * 841b5d6 Fix multi-status tests in OSX
> | * e4cc567 Put multigit test in separate script
> | * 9e5061d Add `multigit` to TravisCI
> | * b2c7416 Fix untracked count in `multi_status`
> | * 04aabb7 Add the “Multi-git” example
> |/  
> * 1e4e539 Trigger docker-build only in one travis-job
> * 2023d27 Add docker-build trigger in Travis build
> * b9d4b44 Tweak vm-tester documentation w.r.t Docker images
> ```
> 
> ###### On `master`
> 
> - Tweak vm-tester documentation w.r.t Docker images.  
> - Add docker-build trigger in Travis build.  
> - Trigger docker-build only in one travis-job.  
> (and fix it).
> - Add the “Multi-git” example.  
> For now there is a `git multi-status <paths>` command.
> - Fix untracked count in `multi_status`.  
> - Add `multigit` to TravisCI.  
> - Put multigit test in separate script.  
> - Fix multi-status tests in OSX.  
> - Add option `--show-modified` (`git multi-status`).  
> - Display repository provider (`git multi-status`).  
> - Put descriptive comments in `git-multi-status`.  
> - Add multigit to the literally documented examples.  
> - Add support for git-config and improve help.  
> - Add new multi-git script: `git-activity-report`.  
> - This is still work in progress.
> - The tests do not verify much yet either.
> - Merge `sm@add-multigit-example` (PR #97).  
> - Merge `master` into `sm@add-activity-report`.  
> - Add `--section-base` to `git-activity_report`.  
> - Compute default `--since` to “Last Sunday”.  
> - Improve markdown output of `git-activity-report`.  
> - Add more tests of `git-activity-report`.  
> - Add `README.md` generation for mutli-git.  
> - Add an example session.  
> - Merge `sm@add-activity-report` (PR #98, @smondet).  
> - Merge `master` into `sm@multigit-readme`.  
> - Add example/demo to multi-git's `README.md`.  
> - Fix typo in multi-git README.  
> - Add blob about limitations of activity-report.  
> - Merge `sm@multigit-readme` (PR #99).  
> 
> #### In `/tmp/git-repos-example/smondet`
> 
> ##### GLab: genspio-doc
> 
> ```
> * e209d85 (HEAD -> master, origin/master, origin/HEAD) Update `.gitlab-ci.yml` (missing in `fb3bbbd`)
> ```
> 
> ###### On `master`
> 
> - Update `.gitlab-ci.yml` (missing in `fb3bbbd`).  
> 
> #### In `/tmp/git-repos-example/tezos`
> 
> ##### GLab: tezos
> 
> ```
> * 8e640159 (origin/victor-proto_process) Update structure (WIP)
> * ebc448fb lib_shell: add fork validator (WIP)
> * 873f47a1 Update machinery to comiple the node_validator
> * b5b8ae0d Shell: remove state from apply block arg an reorganize code
> | * 64cc18ab (origin/galfour/benchmark) Benchmark: unify benchmark methods
> | * fad716fe Benchmark: use c stub instead of Core's timing function
> | * dc211808 Benchmark: hand-made regression benchmarks
> | | * 39a77121 (origin/plaforgue/p2p_discovery) Documentation and refactoring
> | | * 3844071c Local peer discovery
> | | | * 32f96ffa (origin/philb/trusted_peers) P2P: updated test_p2p_pool
> | | | * 0652b819 P2p: refactor maintenance
> | | | * fe1b2a90 P2P: launch maintenance if not enough trusted peers
> | | | * b3e0f57e P2p: sync doc
> | | | * 72dd52d6 P2p: extract independent functions from big let rec
> | | | * 2b7897dc add obj11/tup11 encoding
> | | | | * 23bfa7d9 (origin/quyen/test_michelson_types) change name of functions test
> | | | | * 73e058de failwith
> | | | | * be3d23d1 test_0 added
> | | | | * 7db399e7 todo in control structures
> | | | | * 70893afc add comments
> | | | | * e7ab960c add list map
> | | | | * d961c109 solve some TODOs case
> | | | | * f8329c64 test_2
> | | | | * 540b0983  nothing
> | | | | * 7dd7ed8c test on list
> | | | | * b03b892e Todo cases
> | | | | * a641d763 organise the codes test base on the documentation
> | | | | * c52c69ef organise the codes test base on the documentation
> | | | | * 6f8813d9 add specific operations and organise the codes base on the documentation
> | | | | * 4b918fb9 nothing
> | | | | * 7b616186 operations on maps
> | | | | * 42d0bbed test on operations on sets
> | | | | * 20962686 (origin/abate/mempool-worker) Mempool: add parse and validate functions
> | | | | * 81771470 Mempool: add global workers
> | | | |/  
> | | |/|   
> | | | | * 32199cb5 (origin/vb/raft) Raft: WIP
> | | | | * 628a3b45 Signer: prepare for Raft consensus signing
> | | | | * f054af26 Signer: refactoring
> | | | | | * 93a612af (origin/victor/apply_refacto) Shell/validator: code refactoring to allow standalone block validation
> | |_|_|_|/  
> |/| | | |   
> | | | | | * 2ab271f8 (origin/galfour/benchmark-custom) better benchmarks
> | | | | | * 2eff57e4 packaging
> | | | | | * 009f7b5f interweaved benchmarks
> | | | | | * f9726be2 proof of concept
> | | |_|_|/  
> | |/| | |   
> | * | | | 63989b7c Benchmark: cleanup parsing (removes re dependency)
> | * | | | 3290cb92 Benchmark: fix tests on every bench
> | | | | | * f734dc9f (origin/reorg_michelson_test_contracts) fix prev test
> | | | | | * 4976d3d5 test
> | | | | | * e03529b4 test2
> | | | | | * 547bed81 test
> | | | | | * 207e171f Client: : fix the fail on gitlab (3rd trial)
> | | | | | * 8f9f91c6 Client: fix the fail on gitlab (2nd trial)
> | | | | | * 98338fb4 Client: fix the fail on gitlab (trial)
> | | | | | * 995fb2ca Client: reorg Michelson contracts + update bash scripts
> | | | | | * e65d75ca Revert "Client/Michelson test contracts: fix references to 'contracts' directory"
> | | | | | * 9d9d4369 Client/Michelson test contracts: fix references to 'contracts' directory
> | | | | | * 5ad1145d Client: reorg Michelson test contracts and bash scripts (mini_scenarios, pt2)
> | | | | | * 188912bf Client: reorg Michelson test contracts and bash scripts (macros, pt2)
> | | | | | * cfcf261c Client: reorg Michelson test contracts and bash scripts (opcode, pt2)
> | | | | | * 6eb2081b Client: reorg Michelson test contracts and bash scripts (attic, pt2)
> | | | | | * 8606f5ad Tests: reorganise Michelson tests
> | | | | | * 63e51d28 Tests: split Michelson tests into category attic
> | | | | | * 8e6cd9f2 Tests: split Michelson tests into category mini_scenarios
> | | | | | * 77b5cd89 Tests: split Michelson tests into category macros
> | | | | | * 002d5a2e Tests: split Michelson tests into category opcode
> | | | | |/  
> | | | | | * 70356814 (origin/mb/pending_requests) Shell/Distributed_db: avoid requesting peers with zombie requests
> | | | | | * cf4722da Shell/Distributed_db: distinguish late pending fulfilled (zombie) requests from unrequested answers
> | | | | |/  
> | | | | | * 8ced40b3 (origin/philb/receipt_command) Alpha client: added 'get receipt' command
> | | | | |/  
> | | | |/|   
> | | | | | * 310117bf (origin/vb/crypto-lock-and-wipe) XXX
> | | | | | * eebd8603 [signer backend]: fix lock problem with sk_of_bytes
> | | | | | * 78bdb4f4 [signer backend]: use with_wipe_lock_result in decrypt function
> | | | | | * e1762083 [client commands]: wipe and unlock secret_key from Signature.generate_key
> | | | | | * e980d209 [client commands]: use with_wipe_lock_result for the foundraiser passphrase
> | | | | | * a52cc37b [stdlib]: add blit_list to blit a list of mbytes all at once
> | | | | | * 9de7fa8d Crypto: `m(un)lock` now return a result value
> | | | | | * add56825 Crypto: remove misleading functions
> | | | | | * fe3aa300 Crypto: wipe and unlock few more buffers
> | | | | | * 4243c6d1 Crypto: add environment variable to govern mlock
> | | | | | * e6d2b484 Crypto: lock buffer in `generate_key` functions
> | | | | | * 541c9ae6 Vendors/Uecc: add `unsafe_{sk,pk}_of_bytes`
> | | | | | * 08d921b9 Vendors/Secp256k1: add `unsafe_read_{sk,pk}`
> | | | | | * 1242aada HACL: fix doc
> | | | | | * eebb4564 Signer/Encrypted: use `wipe` and `lock`
> | | | | | * 4bc0c2a5 Crypto: implement `wipe` and `lock`
> | | | | | * f81d3f0c Error_monad: add `finalize`
> | | | | | * bf75fc8e Crypto/HACL: add `wipe` and `lock` primitives
> | | | |_|/  
> | | |/| |   
> | | | | | * 79a863ef (origin/mb/distributed_db_delay) Shell/Distributed_db: make initial request delay depend on resource kind
> | | | | |/  
> | | | | * fa2a9e0d (HEAD -> master, origin/master, origin/HEAD) doc: add support page
> | | | |/  
> | | |/|   
> | | | | * 26b5bbff (origin/galfour/benchmark-landmarks) skepticism
> | | |_|/  
> | |/| |   
> | * | | aebb66e4 Affine model for SEQ
> | * | | 970b5080 Unwrapped calibration tests
> | * | | 42a3d9a7 closure extended
> | * | | cbc9e3c5 More log
> | * | | e202d028 Benchmark: disable stabilize_gc flag - too much overhead
> | * | | af8d3ef8 Bench/Proto: export step instead of interp_generic, removes some overheads
> | * | | 377d94f3 Benchmark: prefix calibration benches's result to the other benches instead of computing everything again
> | * | | 8a2378e2 Benchmarks: translated old calibration benchs to the new form
> | | | | * 1de1bf1f (origin/julien/Nack-with-list-of-peer_rewritten) lib_p2p: attaching list of points to Nack
> | | | | * 21f14d0f lib_p2p/p2p_pool: misc debug messages
> | | | | * 90a98974 test_p2p_pool: Comments
> | | | | * 2d26fbf1 test_p2p_pool: pool parameters as a function of the node position
> | | | | * c3c408c1 test_p2p_pool: adding one level of debug log
> | | | | * abd7d74d test_p2p_pool: introducing overcrowded test,
> | | | | * fe738aae lib_base/p2p_point: adding formatter for list of points
> | | | | * d652bca0 lib_base/p2p_connection: Adding event pretty-printer
> | | | | * 6217d1ee lib_stdlib/option Adding a pretty printer for option types
> | | | | * 97ceb239 Exporting the `equal` fonction of Point.Id
> | | | | * 70bc11c2 lib_p2p/test Adding comment to explain wait_all behaviour
> | | | |/  
> | | |/|   
> | | | | * b0ccf33c (origin/philb/demo_protocol) Update proto_demo
> | | | | * e534e82a CI: update opam-repository branch
> | | | | * 8f5cd015 Shell: remove dead code
> | | | |/  
> | | |/|   
> | | | | * 3f251373 (origin/eztz/signer-deterministic_nonce) signer: added deterministic_nonce
> | | | | | * 9894361f (origin/julien/Nack-with-list-of-peer) p2p: indentation
> | | | | | * 9a8a3179 test_p2p_pool: restauring former tests
> | | | | | * 3504c19d lib_p2p: Nack with list of peers
> | | | | | * d3e4e895 lib_base/p2p_point: adding formatter for list of points
> | | | | | * fd74299c Note: work on Nack wth list
> | | | | | * 7f82443e lib_p2p/p2p_pool: misc debug messages
> | | | | | * d9228193 lib_p2p/test/test_p2p_pool: remove message sending from the client
> | | | | | * 981f1d79 lib_p2p/test/test_p2p_pool: white spaces preventing test success
> | | | | | * 52e1ad5f lib_p2p test_p2p_pool: pool parameters as a function of the node position
> | | | | | * 0d4d7799 lib_base/p2p_connection: Adding event pretty-printer
> | | | | | * 2f1b6e6e lib_p2p/test Adding a test for the "kick with list of pairs" functionality
> | | | | | * 1401bb57 lib_stdlib/option Adding a pretty printer for option types
> | | | | | * 9cd53d07 lib_p2p/test/process Adding wait_almost_all
> | | | | | * 57348d3e lib_p2p/test Adding comment to explain wait_all behaviour
> | | | | | * 886479af Exporting the `equal` fonction of Point.Id
> | | | | | * 14f981d0 Note: Todos
> | | | | | * 247ace4b Note: reorganisation and complement
> | | | | | * c0059e18 Note: connection description
> | | | | | * 90554edf Personnal documentation file to follow the flow of incomming connections
> | | | |_|/  
> | | |/| |   
> | | | | | * 6b45dc08 (origin/abate/prevalidator-rpc-functor) Prevalidator: add RPC error in case of unknown protocol
> | | | | | * 0b28588c Prevalidation: remove now useless protocol_data encoding/decoding
> | | | | | * 8dea2773 Prevalidation: remove lazy computation for rpc directory
> | | | | |/  
> | | | | | * 99563a41 (origin/abate/error_monad_docstring) Error Monad: add docstring to register_error_kind
> | | | | |/  
> | | | | | * 80460c77 (origin/identity_backward_compat) bin_node: sanity check on node identity file
> | | | | | * 128b5ef4 lib_crypto:  Adding pretty printer for public keys
> | | | | | * 1417981a lib_crypto: export neuterize and public_key equality
> | | | | | * 85f64afc Fix bc37fde73eb4a1df3783822256433881e1c0bf59 : Restore compatibily with (old) identity.json that does not contain a `peer_id`
> | | | |_|/  
> | | |/| |   
> | | | | | * 8ba122c9 (origin/michelson_test_contracts) Client: sort Michelson test contracts
> | | | |_|/  
> | | |/| |   
> | | * | | 14b1ba2a Stdlib/Ring: fix ring's semantics
> | | | | | * 5708131b (origin/galfour/benchmark-dirty) dirty test
> | | |_|_|/  
> | |/| | |   
> | * | | | e7cb7d9a added constant run overhead
> | * | | | 69311ea5 add calibration to tests
> | * | | | 5d3dd36c Benchmark: unify the workflow in executable
> | * | | | 153541ba Benchmark: fix indent
> | * | | | 94be1c78 write_lp: addition fix
> | * | | | f56493ff Benchmark: reworked directory hierarchy
> | * | | | d0f1f592 empty trace case added
> | * | | | 7a6ebacb wtf
> | * | | | cd88283c generic benchmarks
> | * | | | d0e36c25 indentation
> | * | | | 81708e9e Generic interp
> | * | | | 017eb9c8 TMP:start clean-up
> | * | | | e807d0aa Bench: moved cp to run.sh
> | * | | | a2ad9d25 cleaning
> | * | | | 93109917 Packaging
> | * | | | 4c73688e better benchmarks
> | * | | | 548f978b all weights, bugfixes, minor improvements
> | * | | | 3973a504 it works
> | * | | | d623f116 save
> | * | | | d2321323 refactoring
> | * | | | 370f4572 refactoring
> | * | | | 1356aedc last basic contract, start refactoring and debugging
> | * | | | 54a05119 save
> | * | | | 5daa0e40 save
> | * | | | 1bb2432b bug
> | * | | | 51f84269 benchmark for force_decode
> | * | | | acf8c92d folder architecture
> | | |_|/  
> | |/| |   
> | | | | * 7de447d3 (origin/plaforgue/annotation_assert) Add annotations for inspecting values with ASSERT_SOME, ASSERT_LEFT, ASSERT_RIGHT
> | | | | * fa7c9997 Alpha, client: unexpand macros when displaying scripts
> | | | |/  
> | | |/|   
> | | * | f898062f Signer: add `handler.mli`
> | | * | 203c212b Micheline: fix printer for code that exceeds 80 columns
> | |/ /  
> | | | * 995f67a8 (origin/state_migration) Shell: fix minor typo in error name
> | | | * 0b7fcc80 Shell: fix indentation
> | | | * 5b23f6f9 Shell: use "atomic" upgrade of disk storage
> | | | * 346459b9 Shell: add missing disk upgrade
> | | | * 44d9e5f4 Shell: store the full block header of the checkpoint
> | | | * c788d8f2 Node: prepare for storage upgrades
> | | | * 919c34cf Shell: use private type for `State.Block.Header.t`
> | | | * 8f436206 Shell/RPC: export a Base58Check representation of block headers
> | | |/  
> | |/|   
> | | | * b1343b4e (origin/abate/prevalidator-preapply-refactor) Prevalidator: advertisement -> pending_advertisement
> | | | * a2f4fea3 Prevalidator: add a couple of comments in the code
> | | | * a6b59f53 Prevalidator: refactor preapply
> | | | * ac179c85 Prevalidator: clarify OutdatedOrNotInBranch
> | | | * f367be85 Prevalidator: remove live_block and live_operations from prevalidator state
> | | | * 32ad6984 Prevalidator: use prevalidation live_block in already_handled
> | | | * 6c9214bf Prevalidation: add two new accessor for live_blocks and live_operations
> | | | * 5a8d6840 Prevalidators: already_handled now returns a tzresult
> | | | * b3067125 Prevalidators: remove polymorphic variant from advertisement
> | | | * 75dff99b Prevalidation: Explain Duplicated, Outdated and Alien operation results
> | | | * cf3980c8 Prevalidation: add parse_list to parse a list of operations
> | | | * cbe3e8b4 Prevalidation: minor preapply refactor
> | | | * 886b9b85 Prevalidator: Remove mempool from the worker state
> | | |/  
> | |/|   
> | * | 03922847 Shell/Peer_metadata: change counters to aribtrary precision integers
> | * | 9d34bd6f RPC: minor changes and add genesis+N
> | * | 60a6b762 RPC: add a hash+N and a hash-N notations
> | * | 2352a783 RPC: add a way to access a given block using its level
> | * | bb698359 Shell: fix notification of new operations in the mempool
> |/ /  
> * | 1272b11e Shell: first batch of statistics in the DistributedDB
> * | ee640c86 Shell: Extract the block-application function into a separate module
> |/  
> | * 2be8be73 (origin/quick-mempool-fix) Stdlib/Ring: fix ring's semantics
> | * c38e0614 Mempool: quick limits fix
> | * 6fb3812e (origin/julien/kick-wth-lst-of-peer) lib_p2p/test Adding a test for the "kick with list of pairs" functionality
> | * 817043b8 lib_stdlib/option Adding a pretty printer for option types
> | * 2d4d5a51 lib_p2p/test/process Adding wait_almost_all
> | * b6fbfa90 lib_p2p/test Adding comment to explain wait_all behaviour
> | * d7a8024d Exporting the `equal` fonction of Point.Id
> | * 848d2b40 Politely rejecting connections by sending the list of known points.
> | * 51265a81 (origin/alphanet) Merge remote-tracking branch 'origin/mainnet-staging' into alphanet
> | * 1a163c37 (origin/pirbo/mainnet, origin/mainnet-staging) Merge commit 'e9668843816af0b61906b2bcdd97cea88072625b' into mainnet-staging
> | * 80f203b7 (origin/galfour/benchmark-wtf) wtf
> | * fdd26204 generic benchmarks
> | *   acbbbbea Merge branch 'galfour/benchmark' of gitlab.com:tezos/tezos into galfour/benchmark
> | |\  
> | | * 67861b0c TMP:start clean-up
> | * | 38fe6f3c indentation
> | * | 2617b0c1 Generic interp
> | |/  
> | * cb22be0d Bench: moved cp to run.sh
> | * 805c5ddd cleaning
> | * 68bbf56a Packaging
> | * e9b284c5 better benchmarks
> | * e296a420 all weights, bugfixes, minor improvements
> | * 95d3f8cd it works
> | * 06e2d5ed save
> | * ec4e4154 refactoring
> | * 3aa1f16f refactoring
> | * ab1f0f57 last basic contract, start refactoring and debugging
> | * 08b4010f save
> | * 053404b7 save
> | * 3db5ab1f bug
> | * 1f1138c6 benchmark for force_decode
> | * a644ac88 folder architecture
> |/  
> | * 15dd35b0 (origin/raphael/prototype-batch-scheduler) PROTOTYPE: batch scheduler
> |/  
> * 7cbfcfa6 Shell: simplify the signature of `Prevalidation`
> * 02bc43b0 Target only USB ledger with interface number 0
> ```
> 
> ###### On `master`
> 
> - Target only USB ledger with interface number 0.  
> - Shell: simplify the signature of `Prevalidation`.  
> Co-authored-by: Raphaël Proust <code@bnwr.net>
> Co-authored-by: Pietro Abate <pietro.abate@tezcore.com>
> Co-authored-by: Grégoire Henry <gregoire@tezcore.com>
> - Shell: Extract the block-application function into a separate module.  
> - Shell: first batch of statistics in the DistributedDB.  
> Co-authored-by: Pietro Abate <pietro.abate@tezcore.com>
> Co-authored-by: Mathias Bourgoin <mathias.bourgoin@tezcore.com>
> - Shell: fix notification of new operations in the mempool.  
> - RPC: add a way to access a given block using its level.  
> - RPC: add a hash+N and a hash-N notations.  
> - RPC: minor changes and add genesis+N.  
> - Shell/Peer_metadata: change counters to aribtrary precision integers.  
> - Micheline: fix printer for code that exceeds 80 columns.  
> - Signer: add `handler.mli`.  
> - Stdlib/Ring: fix ring's semantics.  
> - doc: add support page.  


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
      new-branch-for-the-example -t master

````````````````````````````````````````````````````````````````````````ok-output
    M	README.md
    Branch 'new-branch-for-the-example' set up to track local branch 'master'.
````````````````````````````````````````````````````````````````````````

    $ git -C /tmp/git-repos-example/hammerlab/ketrew/ commit -a -m 'Add \
      greatness to the README'

````````````````````````````````````````````````````````````````````````ok-output
    [new-branch-for-the-example de76a74] Add greatness to the README
     1 file changed, 1 insertion(+)
````````````````````````````````````````````````````````````````````````



Now in the multi-status we can see the modified files, the untracked
counts, and one branch is “ahead” (since we used `-t master` while
creating, it has a remote to define it):

    $ git multi-status --show-modified --no-config \
      /tmp/git-repos-example/hammerlab /tmp/git-repos-example/smondet \
      /tmp/git-repos-example/tezos

````````````````````````````````````````````````````````````````````````ok-output
    #=== /tmp/git-repos-example/hammerlab:=======================================
                                             | Untrk | Modf | Ahd | Behd | Umrg |
    GHub::biokepi........................... | 0     | 2    | 0   | 0    | 0    |
      |- Modified:
      |    - LICENSE
      |    - README.md
    GHub::coclobas.......................... | 1     | 1    | 0   | 0    | 0    |
      |- Modified:
      |    - README.md
    GHub::genspio........................... | 0     | 0    | 0   | 0    | 0    |
    GHub::ketrew............................ | 0     | 0    | 1   | 0    | 0    |
    #=== /tmp/git-repos-example/smondet:=========================================
                                             | Untrk | Modf | Ahd | Behd | Umrg |
    GLab::genspio-doc....................... | 0     | 0    | 0   | 0    | 0    |
    GLab::vecosek........................... | 0     | 0    | 0   | 0    | 0    |
    #=== /tmp/git-repos-example/tezos:===========================================
                                             | Untrk | Modf | Ahd | Behd | Umrg |
    GLab::tezos............................. | 0     | 0    | 0   | 0    | 0    |
````````````````````````````````````````````````````````````````````````



Let's concentrate the activity-report on
`/tmp/git-repos-example/hammerlab` and on the past 3 days:

    $ git activity-report --no-config --since $(date -d '-3 days' \
      +%Y-%m-%d) /tmp/git-repos-example/hammerlab

````````````````````````````````````````````````````````````````````````ok-output
    
    ### In `/tmp/git-repos-example/hammerlab`
    
    #### GHub: genspio
    
    ```
    * 95d94bb (origin/sm@improve-multigit-display) Fix README generation
    * ce0ea82 Improve display of multi-status table
    * 746c0ef Improve display of the activity report
    * f499d0b Fix git alias in multigit documentation
    * 241dbc0 Improve multi-status display
    *   f28085d (HEAD -> master, origin/master, origin/HEAD) Merge `sm@multigit-readme` (PR #99)
    |\  
    | * 417432a (origin/sm@multigit-readme) Add blob about limitations of activity-report
    | * e7a76cb Fix typo in multi-git README
    | * 2c9f192 Add example/demo to multi-git's `README.md`
    | *   d26da12 Merge `master` into `sm@multigit-readme`
    | |\  
    | |/  
    |/|   
    * | 59ccac7 Merge `sm@add-activity-report` (PR #98, @smondet)
    | * 0b6daf6 Add an example session
    | * 09f3fd4 Add `README.md` generation for mutli-git
    |/  
    * 5274995 (origin/sm@add-activity-report) Add more tests of `git-activity-report`
    * 33065aa Improve markdown output of `git-activity-report`
    * b3f9f43 Compute default `--since` to “Last Sunday”
    * c53b2fa Add `--section-base` to `git-activity_report`
    ```
    
    ##### On `master`
    
    - Add `--section-base` to `git-activity_report`.  
    - Compute default `--since` to “Last Sunday”.  
    - Improve markdown output of `git-activity-report`.  
    - Add more tests of `git-activity-report`.  
    - Add `README.md` generation for mutli-git.  
    - Add an example session.  
    - Merge `sm@add-activity-report` (PR #98, @smondet).  
    - Merge `master` into `sm@multigit-readme`.  
    - Add example/demo to multi-git's `README.md`.  
    - Fix typo in multi-git README.  
    - Add blob about limitations of activity-report.  
    - Merge `sm@multigit-readme` (PR #99).  
    
    #### GHub: ketrew
    
    ```
    * de76a74 (HEAD -> new-branch-for-the-example) Add greatness to the README
    ```
    
    ##### On `master`
    
    
    ##### On `new-branch-for-the-example`
    
    - Add greatness to the README.  
````````````````````````````````````````````````````````````````````````



We can see the new commit in the new branch appears in the report ⮵.

And that's all for today ☺ !

License
-------

The code generator is covered by the Apache 2.0
[license](http://www.apache.org/licenses/LICENSE-2.0), the scripts are
ISC [licensed](https://opensource.org/licenses/ISC).

