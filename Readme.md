# Quick Start

On Centos Stream 9 as `root`:


```sh
tmt -vvv -c distro=centos-stream-9 run --all provision --how=local
```

# Directory Structure

## `plans`

TMT plans.

* `external/`: Plans that import remote tmt tests (e.g., from gitlab.com/redhat/centos-stream/rpms)
* `ng.fmf`: The plan that runs the new generation of tests in `tests/ng/` that fully leverage `tmt`
* `t_functional.fmf`: The plan that runs the t_functional tests in `tests/t_functional`


## `tests`

TMT tests

* `ng`: The new generation tests. These will be organized in one directory per packages and also one directory per integration test comprising multiple packages.
* `t_functional`: Contains the tests imported from `t_functional`.

## Adding a Test to `ng`

Create a folder inside `ng/` named after the package you want to test, e.g., `httpd`. It could also be named after some integration test involving multiple packages (e.g., `httpd-php-postgres`).

Organize your tests inside the folder by features you are testing. Feel free to leverage `tmt` and `beakerlib` to design your test. You can follow the `podman` test as an example.

* `tmt` documentation: https://tmt.readthedocs.io
* `beakerlib` documentation: https://beakerlib.readthedocs.io/en/latest/manual.html#

When finish, ensure the tests pass the linting `tmt lint`.

You should be able to run your tests by simply cd'ing into your test and running shell scripts. Example:
```sh
cd tests/ng/podman/image/ls
./test.sh
```

You can also run it using the `tmt` tool. Example of how to run `podman/image/ls` with `tmt` provisioned locally (run directly on your machine).

```sh
tmt -vv -c distro=centos-stream-9 run -a provision --how=local test --name /ng/podman/image/ls
```

Test outputs will be in the default `tmt` folder, e.g. `/var/tmp/tmt`. If you pass `-vv` to the command as in the example above, you will get the full path for the output of each test. For example:

```yaml
    report
        how: display
            pass /tests/ng/podman/image/ls
                output.txt: /var/tmp/tmt/run-1/plans/ng/execute/data/guest/default-0/tests/ng/podman/image/ls-1/output.txt
                journal.txt: /var/tmp/tmt/run-1/plans/ng/execute/data/guest/default-0/tests/ng/podman/image/ls-1/journal.txt
        summary: 1 test passed
```

## Adding an External Test

Create a `.fmf` file within `plans/external/` named after the package, and follow the `tmt` documentation how to import tmt tests from git (see the `discover` step [Example](https://tmt.readthedocs.io/en/stable/examples.html#plans), and [Specs](https://tmt.readthedocs.io/en/stable/spec/plans.html#spec-plans-discover) ). You can follow the `plans/external/podman.fmf` example.


# Tiers

TODO: define the tiers and what tiers a test should go into.