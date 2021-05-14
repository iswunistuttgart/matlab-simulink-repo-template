# Template for a version controlled MATLAB-Simulink Repository

Simple basic template to clone and used for MATLAB-Simulink projects.
Essentially provides 

- a `.gitignore` file,
- m `.gitattributes` file for merging (binary) Simulink models
- and some instructions to set up and use MATLAB Simulink for use with Git


## Setup MATLAB once for use with Git

According to [MATLAB's documentation](https://de.mathworks.com/help/simulink/ug/customize-external-source-control-to-use-matlab-for-comparison-and-merge.html):

1. seup MATLAB (from MATLAB console)

    ```matlab
    comparisons.ExternalSCMLink.setup()
    ```

2. Add MATLAB diff to your `.gitconfig`. This can be done from within MATLAB with

    ```
    comparisons.ExternalSCMLink.setupGitConfig()
    ```

    > **Note for Windows users:** In some cases MATLAB writes `$PWD/` before the path in sections `[mergetool "mlMerge"]` and `[difftool "mlDiff"]`. This may fail using the git command line, as mlDiff writes the model versions to your `Temp` folder and then use the  wrong path (`$PWD/C:/Users/.../Temp/*.slx`). Just delete the `$PWD/` parts. Your `.gitconfig` sections should look like

    ```ini
    [mergetool "mlMerge"]
        cmd = \"C:/Program Files/MATLAB/R2020b/bin/win64/mlMerge.exe\" $BASE $LOCAL $REMOTE $MERGED

    [difftool "mlDiff"]
        cmd = \"C:/Program Files/MATLAB/R2020b/bin/win64/mlDiff.exe\" $LOCAL $REMOTE
    ```

    >You need to replace your MATLAB path in the above example.

3. The `.gitattributes` file of this directory automatically dispatches merges on Simulink models to MATLAB. I.e., models will be truly merged instead of only applying the newer version. 

## Use Git to show diffs on Simulink models

Again, following [MATLAB's documentation](https://de.mathworks.com/help/simulink/ug/customize-external-source-control-to-use-matlab-for-comparison-and-merge.html): diff (from command line) with

```sh
git difftool -t mlDiff <revisonID1> <revisionID2> myModel.slx

# e.g. diff with previous version
git difftool -t mlDiff HEAD HEAD~1 myModel.slx
```
