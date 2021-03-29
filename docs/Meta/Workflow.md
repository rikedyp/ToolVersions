# Workflow

## Version information
For any given commit to a repository that is involved in this process, the following information needs to be available.

-	A minimum, and optionally a maximum, APL version that the commit is compatible with.
-	The tool's own version number: Major, Minor and optionally a “Variant” which will identify versions of the code that target abnormal build targets. 

In this example, the version information is found in the `Version` function of the tool which is a single scripted namespace.

```APL
    ∇ VERSION_STRING←Version;APL_VERSION_RANGE
      APL_VERSION_RANGE ← '170-181'
      VERSION_STRING ← '0.0.0'
    ∇
```

See the [tool versions tool](ToolVersions.md) for more information about Dyalog-tool version compatibility and naming schemes.

## Release notes
The first line of the release notes **must** follow a strict pattern to allow the automated Tool Versions API to match Dyalog versions with tool versions.

The first line begins with the text `Works with Dyalog versions: ` (the colon `:` is followed by a single space `⎕UCS 32` character). That text is followed by one or more numbers with the following specification:

|Number format|Compatibility|Example|
|---|---|---|
|A single Dyalog version number | This tool version is compatible with that Dyalog version only | `Works with Dyalog versions: 17.0`
|A single Dyalog version number followed by a comma | This tool version is compatible with that Dyalog version or higher | `Works with Dyalog versions: 17.1,`
|Two Dyalog version numbers separated by a dash `⎕UCS 45` | This tool version is compatible with all Dyalog versions from the lowest to the highest specified inclusive | `Works with Dyalog versions: 17.1-19.0`
|Two or more Dyalog version numbers separated by commas `⎕UCS 44` | This tool version is compatible with the specified Dyalog versions only | `Works with Dyalog versions: 17.1,19.0`

### Examples:
```text
Works with Dyalog versions: 17.0         ⍝ v17.0 only
Works with Dyalog versions: 17.1,        ⍝ v17.1 or higher
Works with Dyalog versions: 17.0-18.1    ⍝ v17.0,v17.1,v18.0,v18.1
Works with Dyalog versions: 16.0,17.1    ⍝ v16.0 and v17.1 only
```

## Definition of Done
It is a good idea to decide on these criteria as early as possible in a development cycle. The definition of done must be met before a release is made public ("released").

## Pre-flight checklists
Details to be included later. For now, see the [Example](Example.md).

### Increment version number
When incrementing the major or minor version number:

### Dyalog has a release
What to include in Dyalog source control (svn) external sources:

### Backporting a bug fix
When a bug deemed critical needs a fix to support an old Dyalog version

### Making a change to old documentation
This process is a little bit involved, but should be a rare occurance.
