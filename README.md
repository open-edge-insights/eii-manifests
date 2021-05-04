# EII Manifests

This repo hosts the manifest files (also known as recipes) needed to demonstrate multiple use cases of
Edge Insights for Industrial

## Steps to choose a particular recipe/manifest using `repo` tool

The `repo` command is an executable python script, predominantly used in Android
development workflow to simiplify the work across multiple repositories.
More details on `repo` and it's usage can be found at below links:

* [Repo command reference](https://source.android.com/setup/develop/repo)
* [Using Repo and Git](https://wladimir-tm4pda.github.io/source/git-repo.html)

For EII code base in multi-repo, below are the commands one needs to run which
in essence is similar to git cloning of mono-repo to pull out the required
source code.

1. **Installing repo tool**

   ```sh
   $ curl https://storage.googleapis.com/git-repo-downloads/repo > repo
   $ sudo mv repo /bin/repo
   $ sudo chmod a+x /bin/repo
   ```

2. **Pulling in the eii-manifests repo and using a manifest file**

   * Create a working directory

     ```sh
     $ mkdir -p <work-dir>
     ```

   * Initialize the repository using `repo` tool.

     ```sh
     $ cd <work-dir>
     $ repo init -u "git@github.com:open-edge-insights/eii-manifests.git"
     ```

     This will create a .repo folder with `eii-manifests` source code
     pulled at `.repo/manifests` under the current directory. Since no
     manifest branch has been specified, in the below command with `-b`
     switch, it will pull in the `master` branch source code at
     `.repo/manifests` folder. Please note, the branch name will be
     `default` here and not master but once can confirm that it
     is pointing to master by `git log` command.

     Also, as no manifest/recipe file has been chosen with `-m` switch in the
     above `repo init` command, by default, the `default.xml` manifest will be
     selected. Please note, the `default.xml` corresponds to recipe/manifest
     where all the EII module and it's ingredient projects gets pulled in.

     ----
     **NOTE**:
     1. If one wants to test the changes of individual repos lying in a **developer branch**,
        the best way to have this tested is to have a branch pushed to `eii-manifests` repo
        with the required manifests file/s updated, pointing to that **developer branch**.
        Eg:
        `Using a developer manifest branch to point to developer branch/s in multi-repo

        ```sh
        $ repo init -u "git@github.com:open-edge-insights/eii-manifests.git" -b <manifest_branch_name>
        ```

        In above command, `manifest_branch_name` refers to remote branch in `eii-manifests`
        repo which could be used by other developers or the validation team to validate the
        manifests/recipes coming out of this `manifest_branch_name`.

     2. If one wants to fetch the latest master branch of `eii-manifests` repo, please
        run the below command:

        ```sh
        $ repo init -u "git@github.com:open-edge-insights/eii-manifests.git"
        ```
    ----

   * To validate the manifest that's in play (the below command should show
     the contents of `default.xml` as that is been chosen by default)

     ```sh
     $ repo manifest
     ```

   * Choosing a different manifest file

     ```sh
     $ repo init -m <manifest_file>
     ```

     Below are the various `manifest_file` values for the above command:

     * [default.xml](./default.xml): **All recipe (default)**:

       Recipe has Vision + Timeseries + EII ingredient services

     * [core.xml](./core.xml): **Bare minimum recipe**

       Recipe has EII common libs/utils, build tools and sample apps(please check
       the README.md of the Samples to build and run)

     * [video.xml](./video.xml): **Vision recipe**

       Recipe has EII Bare minimum recipe + vision services

     * [time_series.xml](./time_series.xml): **Timeseries recipe**

       Recipe has EII Bare minimum recipe + time series services

      > **NOTE**:
      > One can create his/her own manifest file with the combination of projects
      > **OR** update the existing manifests as per the need and use it
      > accordingly.
      > For more details on manifest xml file syntax and semantics, please refer below links:
      >
      > * https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.md
      > * https://docs.sel4.systems/projects/buildsystem/repo-cheatsheet.html

3. **Pull all the projects mentioned in the manifest xml file with the
   default/specific revision mentioned for each project**

    ```sh
    $ repo sync
    ```

    > **NOTE**:
    > Every repo init command should be followed by repo sync command to
    > pull the source code for that particular recipe/manifest if the
    > intention is to work with that particular recipe

4. **Refer [README.md](https://gitlab.devtools.intel.com/Indu/edge-insights-industrial/eii-core/-/blob/master/README.md) to provision, build and run EII stack**

