## Guidelines for creating pull requests.

### Issue

Please create an issue for general discuss regarding the changes in the pull request. Ideally this is done before the pull request is submitted. Pull requests are always welcome and encouraged, but coordinating efforts saves time for everyone.

### Branches

Please target the devt branch with your pull requests. All new changes and features are staged in the devt branch before merging to the master branch. 

For experimental features, it is also acceptible to target other, non master, branches.

### Build Date

Any time changes affect the compiled firmware, change the build date in grbl.h to the date of of the pull request. 
$define GRBL_VERSION_BUILD "YYYYMMDD" ... Example: #define GRBL_VERSION_BUILD "20190517".

**Note:** This date may be changed if other changes, with later dates are merged first.

### File names

If files need to be renamed from standard grbl file names due to conflicts with libraries, etc, please prefix the old filename with "grbl_". **Example:** limits.h has a conflict, so limits.h and limits.cpp were renamed to grbl_limits.h and grbl_limits.cpp





