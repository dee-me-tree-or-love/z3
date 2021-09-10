# Docker images for Z3

This contains Dockerfile's usable for creating Z3 images.  

> _**Why?**_
>
> Instead of installing Z3 as binaries (or otherwise), run it using docker.  
>
> ```bash
> # create a file in, say, examples/task1.smt2
> $ cat ./examples/task1.smt2
>
> (set-logic QF_LIA)
> (declare-fun y () Int)
> (declare-fun x () Int)
> (assert (> (+ (mod x 4) (* 3 (div y 2))) (- x y)))
> (check-sat)
>
> # -i flag allows to connect to standard input
> $ cat examples/task1.smt2 | docker run --rm -i deemetree/> z3-fork:ubuntu-20.04-bare-z3-edge -in -smt2
>
> sat
> ```

## Build context

The image source is kept up to date with the [upstream Z3](https://github.com/Z3Prover/z3).  
The builds are executed weekly
(see
[`../.github/workflows/publish-docker.yml`](../.github/workflows/publish-docker.yml)
) for details.  

The available images are stored in this DockerHub repo:  
<https://hub.docker.com/r/deemetree/z3-fork>

## Tags

Tags use the following pattern:  

```plaintext
<base-os-image: e.g ubuntu-20.04>-<z3-flavor: e.g. bare-z3|python-z3|java-z3 etc>-<build type: e.g. sha-xyz|tag|edge|date>
```

The full list of available tags is available on Docker Hub:
[deemetree/z3-fork/tags](https://hub.docker.com/r/deemetree/z3-fork/tags?page=1&ordering=last_updated).

> NB: `edge` and weekly images are built using the latest source code, so can technically be unstable!
> Consult the [official Z3 releases](https://github.com/Z3Prover/z3/releases) for more information.

## Supported images

### ubuntu-20.04

1. Pull the image:

    ```bash
    $ docker pull deemetree/z3-fork:ubuntu-20.04-bare-z3-edge
    # for other tags see https://hub.docker.com/r/deemetree/z3-fork/tags
    ```

2. Execute help command:

    ```bash
    $ docker run --rm deemetree/z3-fork:ubuntu-20.04-bare-z3-edge -h
    Z3 [version 4.8.13 - 64 bit]. (C) Copyright 2006-2016 Microsoft Corp.
    Usage: z3 [options] [-file:]file

    Input format:
    -smt2       use parser for SMT 2 input format.
    -dl         use parser for Datalog input format.
    ...
    ```

3. Run z3 on a SMTLIB2 file (with volume mapping):

    ```bash
    # create a file in, say, examples/task1.smt2
    $ cat ./examples/task1.smt2

    (set-logic QF_LIA)
    (declare-fun y () Int)
    (declare-fun x () Int)
    (assert (> (+ (mod x 4) (* 3 (div y 2))) (- x y)))
    (check-sat)

    # /.z3-wb/ is an arbitrary directory choice
    $ docker run --rm -v `pwd`:/.z3-wb/ deemetree/z3-fork:ubuntu-20.04-bare-z3-edge -smt2 /.z3-wb/examples/task1.smt2

    sat
    ```

4. Run z3 on a SMTLIB2 file (using pipes):

    ```bash
    # create a file in, say, examples/task1.smt2
    $ cat ./examples/task1.smt2

    (set-logic QF_LIA)
    (declare-fun y () Int)
    (declare-fun x () Int)
    (assert (> (+ (mod x 4) (* 3 (div y 2))) (- x y)))
    (check-sat)

    # -i flag allows to connect to standard input
    $ cat examples/task1.smt2 | docker run --rm -i deemetree/z3-fork:ubuntu-20.04-bare-z3-edge -in -smt2

    sat
    ```

## More information

General details about Z3 and how to use can be found on the upstream repository: [Z3Prover/z3 -> README.md](https://github.com/Z3Prover/z3/blob/master/README.md).

## Remarks

- Support for Python, Java and .Net bindings are still to be provided.
- Feel free to submit PRs to this repository if you'd like to add extra docker images for z3!