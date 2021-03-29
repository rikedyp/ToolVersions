# The Tool Versions Tool
A web API is proposed to facilitate:

- including a GitHub-hosted tool in Dyalog installations.
- being able to reproduce a particular build of Dyalog from the past.
- find the latest "stable" (support) version of a tool, that may contain critical bug fixes, which is compatible with an old version of Dyalog
- find the latest version of a tool which is compatible with any particular version of Dyalog

It would allow URL queries to obtain releases, for example:

- `https://toolversions.dyalog.com/toolname/180` to obtain the latest release of `toolname` compatible with Dyalog v18.0.  
- `https://toolversions.dyalog.com/toolname/180?date=2021-01-13` to obtain the release of `toolname` which was included with the Dyalog build on the 13<sup>th</sup> January 2021.

It would also provide a GUI at `https://toolversions.dyalog.com` to allow users to find that information.

See details of the [tool versions formats](../toolversions.md)