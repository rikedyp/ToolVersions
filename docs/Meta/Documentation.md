# Writing Documentation
Documentation is written as markdown files and rendered using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/getting-started/).  
To create versioned documentation, use [Mike](https://squidfunk.github.io/mkdocs-material/setup/setting-up-versioning/).  
Documentation lives in the `docs/` folder in the root of the repository.

!!! Note
	All commands are from the source root, one level above the `docs/` folder.

## Setting up MkDocs
1. First, initialise the docs folder and configuration `mkdocs.yml` file.

        mkdocs new .

1. Optionally, copy the configuration file, styles and images from this repository (details of differences to come).

1. You can now start writing documentation, and [see how it looks in rendered form](#viewing-rendered-docs).

## Publishing docs
MkDocs generates static websites so the documentation can be made available for public consumption using any web server. For convenience, MkDocs and Mike come with commands which integrate with GitHub.

!!! Warning
	- Using `mike deploy VersionName` will overwrite the existing documentation in the `VersionName` folder.
	- Using `--push` will attempt to push changes to the `gh-pages` branch of your repository.

### Latest
1. Set the default docs to latest (enables redirect from root)

        mike set-default latest

1. Then, deploy the current docs to GitHub with the correct version and alias `latest`

        mike deploy --push --update-alises 0.0 latest

### Any version
Mike simply builds your documentation from the current doc source and commits it to the gh-pages branch in a folder with the same name as the version you give Mike. 

For example,
```
mike deploy 0.3
```
will put the current source in `gh-pages/0.3`. 
```
mike deploy foo
```
will put the current source in `gh-pages/foo`. 

When pushed to GitHub with GitHub Pages enabled for the gh-pages branch, this built version becomes available at the url `https://user.github.io/repo/foo`.

## Viewing rendered docs
Hot reloading means that changes to markdown files are quickly rendered on a locally served web page.
```bash
mkdocs serve
```

## Maintaining latest docs
The markdown files in the `docs` folder at a given commit should reflect the behaviour of the tool at that commit.
