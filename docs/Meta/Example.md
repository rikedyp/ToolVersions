# Example
This repository serves as a demonstration.  
The rough philosophy of the process is as follows:  
The main branch (trunk) is core and stable. Only important changes deemed suitable for all builds of compatible Dyalog versions are to be made most of the time. If it is determined that new development (a new feature, or set of not-so-critical bug fixes) is desired, then at that point a support branch is created to freeze the state of the tool for the sake of future builds of old but compatible Dyalog versions.

All described changes are made to the main branch (trunk) unless otherwise specified.

## Set up automated draft releases
Using GitHub actions, create an action "draft-release.yml"
```YAML
name: "Draft Release"

on:
  push:
    branches:
      - "trunk"
  workflow_dispatch:   # allows triggering from GitHub interface

jobs:
# Obtain major.minor.[variant-] version number
  draft_release:
    runs-on: ubuntu-latest
    steps:
      - name: Triggered by which branch
        run: echo "$GITHUB_REF"
      - name: Checkout code
        uses: actions/checkout@v2      
      - name: Read major.minor
        id: majmin
        run: |
          VER=$( cat MyTool.apln | grep "VERSION_STRING←'[0-9]\+.[0-9]\+..*-\?0'" | cut -c 25- | sed "s/...$//" )
          echo ${VER}
          echo "::set-output name=version::${VER}"
# Create or update Draft release
      - name: Build and test
        run: |
          echo "done!"
      - name: Insert patch number
      # Replaces e.g. VERSION_STRING ← 0.3.0       with   VERSION_STRING ← 0.3.[nruns]-[git-hash]
      #            or VERSION_STRING ← 0.3.dev-0   with   VERSION_STRING ← 0.3.dev-[nruns]-[git-hash]
        run: |
          sed -i "/VERSION_STRING←'[0-9]\+.[0-9]\+..*-\?0'/s/0'/${{ github.run_number }}-${{ github.sha }}'/" MyTool.apln 
      - name: Draft release
        uses: "marvinpinto/action-automatic-releases@latest"        
        with:
          repo_token: "${{ secrets.GITHUB_TOKEN }}"
          automatic_release_tag: "${{ steps.majmin.outputs.version }}.${{ github.run_number }}-${{ github.sha }}"
          prerelease: false
          draft: true
          title: "v${{ steps.majmin.outputs.version }}.${{ github.run_number }}"
          files: |
            MyTool.apln
```

In the real system, this should delete the previous unpublished draft release. With the above action, draft releases must be manually removed. Furthermore, an organisation's repository should be developed using forks and pull requests, but here commits are made directly to the main repository.

In this system, supported Dyalog versions are manually input in release notes upon publication of releases. In the real system, this part may be automated.

## Initial commits (v0.0)
Compatible with Dyalog v17.0 to latest  
Dyalog v17.0 is released including this (patch) version of the tool.  
From this point onwards Dyalog v17.1 is the actively developed version.

1. Create `README.md`
1. Create `MyTool.apln` with embedded version function (0th feature)
1. Document MyTool v0.0
1. Commit to the trunk

        git add .  
        git commit -m "Version function"  
        git push

1. Set default docs to latest

        mike set-default --push latest

1. Publish latest docs  

        mike deploy --push --update-aliases 0.0 latest

1. Publish release

    First line of notes:  
    `Works with Dyalog versions: 17.0- `

## Set up GitHub Pages
On the GitHub website, go to the repository settings (e.g. `https://github.com/rikedyp/MyTool/settings`) and enable GitHub pages using the `gh-pages` branch.

## Post-release bug fixes
Shortly after the release of Dyalog v17.0, bugs were found for which a fixed version of MyTool is to be included in future builds of v17.1 (active development) and v17.0.

1. Fix bug
1. Commit change
1. Push
1. Publish release

    Presumably once a few of these bugs have been fixed, publish the fixes together, rather than one release per bug fix.

## Add a feature (v0.1)
Compatible with Dyalog v17.0 to latest. However, this new development will only be included with the Dyalog version currently in active development (v17.1).

1. Create branch from the head named `0.0.170`
	1. Update `VERSION_STRING`

        In MyTool, `VERSION_STRING ← '0.0.170-0'`  
        In a real tool, possibly `VERSION_STRING ← '0.0.0'` 

	1. Tag commit `0.0.170-1-[git-hash]`

        In a real tool, this could be automated

1. Create a draft release from branch `0.0.170`

    This draft release is now picked up by future builds of Dyalog v17.0

1. Checkout the trunk
1. Add feature `ListReleases` to `MyTool.apln`
1. Document `ListReleases`
1. Create "Old Versions" page in docs
1. Commit changes 4 and 5 to trunk
1. Commit "old versions page" change to trunk
1. For each previous minor version (with docs for that particular version)
	1. Check out commit with tag of previous minor release (corresponding to current docs for that version)
	1. Cherry pick "old versions page" commit to previous release
	1. Publish old docs version

        mike deploy --push 0.0

1. Publish new latest docs version

        mike deploy --push --update-aliases 0.1 latest

## Fix a bug
Dyalog v17.1 is released including this (patch) version of the tool.  
From this point onwards Dyalog v18.0 is the actively developed version.

1. Add comment ("bug fix") to `MyTool.apln`
1. Commit to trunk
1. Publish release

    First line of notes:  
    `Works with Dyalog versions: 17.0-`

## Period of special bug fixes
A set of [post-release bug fixes](#post-release-bug-fixes), but for Dyalog versions v17.1 and v18.0.

## Add a feature (v0.2)
Compatible with Dyalog v18.0 to latest  

1. Create a branch from the head named `171`
1. Create a draft release from branch `171`

    This draft release is now picked up by future builds of Dyalog v17.1

1. Checkout the head of the trunk
1. Add `F2` to `MyTool.apln`
1. Document `F2`
1. Commit to trunk
1. Publish new docs version
1. Update "old versions" page of previous docs versions (see [Add a feature (v0.1)](#add-a-feature-v01))

## Add a feature (v0.3)
Compatible with Dyalog v18.0 to latest  
Dyalog v18.0 is released with this (patch) version of the tool, including the new feature `F3`.  
From this point onwards Dyalog v18.1 is the actively developed version.

1. Add `F3` to `MyTool.apln`
1. Document `F3`
1. Commit to trunk
1. Publish new docs version
1. Update "old versions" page of previous docs version (see [Add a feature (v0.1)](#add-a-feature-v01))

## Make change to v0.1 docs
1. Checkout commit of 0.1 release
1. Make change to doc
1. Publish old docs version
1. Copy docs folder somewhere on your file system
1. Checkout head
1. Copy locally stored, modified old docs folder to `docs/0.1`
1. Modify mkdocs.yml to exclude `docs/0.1`
1. Commit to trunk and push

## Backport a critical bug fix
This bug has been deemed severe enough to warrant cherry-picked backporting to old versions of the tool which are compatible with supported versions of Dyalog

The fix will be backported to MyTool versions included in builds of v17.0, v17.1 and v18.0. However, the bug requires different fixes for v17s and v18s. A new branch 171 will be created.

1. Fix bug in the trunk
1. Commit to trunk
1. Determine commits corresponding to old releases of tool (using [the tools versions tool](ToolVersions.md), which need the patch applied and for each commit:
	1. Branch the commit, giving it the appropriate name
	1. Cherry pick the fix
	1. Push the new branch to GitHub

