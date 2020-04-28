# IREE Documentation

This is the source for IREE's
[documentation hub](https://google.github.io/iree).

## Documentation Structure

The documentation website hosts both directly authored markdown files copied
from the [docs/ folder](https://github.com/google/iree/tree/master/docs) and
files generated by the build system, such as tablegen dialect pages.

The
[build_tools/cmake/build_docs.sh](https://github.com/google/iree/blob/master/build_tools/cmake/build_docs.sh)
script builds generated documentation and copies other documents into the `doc/`
subdirectory for the build folder.

The
[scripts/prepare_doc_publication.py](https://github.com/google/iree/blob/master/scripts/prepare_doc_publication.py)
script processes these documents, adding additional metadata for rendering HTML.

The `Publish Documentation` GitHub Action defined by
[.github/workflows/publish_docs.yml](https://github.com/google/iree/blob/master/.github/workflows/publish_docs.yml)
automatically runs these scripts and commits the resulting files to the
`gh-pages` branch, whenever changes are made to the `master` branch.

## How to Update Documentation

### Making Simple Changes

Updates to already published pages can be made directly to markdown files on the
`master` branch, where they will be picked up by the `Publish Documentation`
GitHub Action. These markdown files can generally be authored using regular
GitHub Flavored Markdown, but note that these files will be rendered both on
GitHub and GitHub pages, so there may be some differences.

### Making Complex Changes

To publish new pages or make structural changes to the website, edit the scripts
mentioned above or the supporting files on the `gh-pages` branch. These sorts of
changes should be tested by building the full website, either by pushing to a
(publicly visible) remote fork or by setting up Jekyll for local development.

To build the website yourself, mimic the `Publish Documentation` action:

1.  On the `master` branch, run `bash ./build_tools/cmake/build_docs.sh`
2.  On the `master` branch, run `python3 ./scripts/prepare_doc_publication.py ./build-docs/doc`
3.  Switch to the `gh-pages` branch (or clone the repo again to make concurrent changes)
4.  Copy the contents of `build-docs/doc/` to the `gh-pages` branch: `cp -rf build-docs/doc/* ./docs/`
5.  Move `index.md` to the root: `mv -f ./docs/index.md .`
6.  Push to a fork, or serve the website locally

#### Pushing to a Fork

Pushing to the `gh-pages` branch on a fork will automatically publish a GitHub
Pages website at `https://[NAME].github.io/iree/` within 20 minutes. This is
convenient, and will map 1:1 to what the website will look like on the main
IREE repository, but it can be slow to iterate on and publishes content that
might not be ready for sharing.

#### Working Locally

[Jekyll](https://jekyllrb.com/) can be used to build the website and serve it
on a local webserver. See GitHub's
[Testing your GitHub Pages site locally with Jekyll](https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll)
help page for instructions on setting up Jekyll on your machine, then run
`bundle exec jekyll serve` and load the server address that it outputs in your
web browser.
