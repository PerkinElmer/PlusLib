What is this project ?
----------------------

This project is **NOT** the official PlusLib repository.

It is a fork of PlusLib sources hosted at https://github.com/PlusToolkit/PlusLib.

It is used as staging area to maintain and test patches that will be contributed back to the
official repository.


What is the branch naming convention ?
--------------------------------------

Each branch is named following the pattern `sonoplus-vY.Y.Z-YYYY-MM-DD-SHA{7}`

where:

* `vX.Y.Z` is the version of the forked project
* `YYYY-MM-DD` is the date of the last official commit associated with the branch.
* `SHA{9}` are the first seven characters of the last official commit associated with the branch.

For more details, this was based on conventions at https://www.slicer.org/wiki/Documentation/Nightly/Developers/ProjectForks


How to backport changes to a specific branch ?
----------------------------------------------

1. Fork `SonoVol/PlusLib`

2. Checkout the relevant `sonoplus-vX.Y.Z-YYYY-MM-DD-SHA{9}` branch. See [SuperBuild/CMakeLists.txt](https://github.com/SonoVol/SonoPlus/blob/develop/SuperBuild/CMakeLists.txt#L133-L136) to identify the name of the branch.

3. Add the remote from which to cherry-pick commit from

4. Cherry-pick the commit(s)

5. Amend the commit updating title and message to include `[backport]` and `Cherry-picked commit ...` information.

    Adding these details streamlines the creation of a new `sonoplus-` branch allowing to easily identify which commits are specific to SonoPlus and which ones have been backported.

    ```
    [backport] BUG: vtkPlusDevice does not clean up threads when stopping recording

    Cherry-picked commit https://github.com/PlusToolkit/PlusLib/commit/7d0081f7190b6f341c729f2ef5b48b5f95b2254a from main PlusLib repository.

    ...
    ```

6. Finally, create a pull request against the relevant `sonoplus-vX.Y.Z-YYYY-MM-DD-SHA{9}` branch.


_Note: If you have push access to the `SonoVol/PlusLib` fork, you could directly push commit to the relevant branch_


How to update the version of PlusLib ?
----------------------------------

1. Clone this repository and add a remote to the official project

```
git clone https://github.com/SonoVol/PlusLib
cd PlusLib
git remote add upstream https://github.com/PlusToolkit/PlusLib
git fetch upstream
```

2. Create a new branch following the branch naming convention mentioned in a section above.

3. Cherry-pick the SonoVol specific commits from the last used `sonoplus-vX.Y.Z-YYYY-MM-DD-SHA{9}` branch. Resolve conflicts as needed.

   - the commit hash included in each branch name is the PlusLib *upstream* hash, so
     SonoVol specific commits may be viewed with the git range (`..`) operation:
```
          git log --pretty=format:"%h" {PlusLibSHA}..sonoplus-xyz-{PlusLibSHA}
```

4. To **test the changes**, locally rebuild SonoPlus.

5. Publish the branch. (directly in this repo if you have push rights, or on a fork)

6. Update SonoVol SuperBuild/CMakeLists.txt by submitting a pull request.
