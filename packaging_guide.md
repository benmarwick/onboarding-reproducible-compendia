# DataONE Reproducible Research Compendia Guide

DataONE accepts compendia that meet our guidelines via a streamlined [onboarding process](https://github.com/benmarwick/onboarding-reproducible-compendia). To ensure a consistent style across all of our tools we have developed this concise guide. We strongly recommend that compendia developers read Hadley Wickham's concise but thorough book on package development which is available for [free online](http://r-pkgs.had.co.nz/) (and [print](http://www.amazon.com/dp/1491910593/ref=cm_sw_su_dp?tag=r-pkgs-20)) and Marwick, Boettiger and Mullen's paper [_Packaging data analytical work reproducibly using R (and friends)_](https://peerj.com/preprints/3192/)

## Sections (in order of importance)

* [Research data](#resdata)
* [Licensing](#lic)
* [Compendia naming](#pkgnaming)
* [README](#rme)
* [Code of conduct](#conduct)
* [Documentation](#docs)
* [News](#news)
* [Authorship](#authorship)
* [Dependencies](#deps)
* [Testing](#testing)
* [Continuous integration](#ci)
* [Console messages](#messages)
* [Recommended software scaffolding](#tools)
* [Further guidance](#further)
* [Suggestions and Updates](#suggestions)

## <a href="#resdata" name="resdata"></a> Research data

* Data files should be in open, plain text, cross-platform formats
* Data files are clearly separated from code and other items in the compendium
* Small data files (totalling <100 mb) should be in a `/data` directory, and if data are produced during the analysis, the input data should be clearly distinguished from the derived data (e.g. `/data/raw_data` and `/data/derived_data`) 
* Medium to large data files (>100 mb per file) should be deposited in a trustworthy repository, and referenced in the compendium using a persistent identifier (such as a DOI)
* Data are documented ...somehow...

## <a href="#lic" name="lic"></a> Licensing

We follow Stodden's [Reproducible Research Standard](https://web.stanford.edu/~vcs/papers/ijclp-STODDEN-2009.pdf) which recommends:

* CC-BY for text and figures (e.g. the narrative text of your article or report), declare this in your README
* CC-0 for data files, declare this in your README
* MIT for code, use `devtools::use_mit_license()` to declare this in your DESCRIPTION file

## <a href="#rme" name="rme"></a> README

* All packages should have a README file, named `README.md`, in the root of the repository. The README should include, from top to bottom (see [this example](https://github.com/karthik/badge-test/)):

```
* The compendium name
* Badges for continuous integration and test coverage, the badge for DataONE peer-review once it has started (see below), and any other badges
* Citation information
* Short description of the package
* Installation instructions
* Brief demonstration usage

```

* Once you have submitted a package and it has passed editor checks, add a peer-review badge via

```
[![](https://badges.ropensci.org/<issue_id>_status.svg)](https://github.com/ropensci/onboarding/issues/<issue_id>)
```

where issue_id is the number of the issue in the onboarding repository. For instance, the badge for [`rtimicropem`](https://github.com/ropensci/rtimicropem) review uses the number 126 since it's the [review issue number](https://github.com/ropensci/onboarding/issues/126). The badge will first indicated "under review" and then "peer-reviewed" once your package has been onboarded, and will link to the review issue.

* If your package connects to a data source or online service, or wraps other software,
consider that your package README may be the first point of entry for users.  It should provide enough information for users to understand the nature of the data, service, or software, and provide links to other relevant data and documentation.  For instance,
a README should not merely read, "Provides access to GooberDB," but also include,
"..., an online repository of Goober sightings in South America.  More
information about GooberDB, and documentation of database structure and metadata
can be found at *link*.

* We recommend not creating `README.md` directly, but from a `README.Rmd` file (an Rmarkdown file) if you have any demonstration code. The advantage of the `.Rmd` file is you can combine text with code that can be easily updated whenever your package is updated.

* Extensive examples should be kept for a vignette. If you want to make the vignettes more accessible before installing the package, we suggest creating a website for your package with [`pkgdown`](https://github.com/hadley/pkgdown)

* Consider using `devtools::use_readme_rmd()` to get a template for a `README.Rmd` file
and to automatically set up a pre-commit hook to ensure that `README.md` is always newer than `README.Rmd`.

* _After_ a package is accepted, the rOpenSci footer should be added to the bottom of the README file with the following markdown line:

```
[![ropensci_footer](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)
```

* See the [gistr README](https://github.com/ropensci/gistr#gistr) for a good example README to follow.

## <a href="#conduct" name="conduct"></a> Code of Conduct

* We recommend that you use a code of conduct such as the [Contributor Covenant](http://contributor-covenant.org/) in developing your package.  You can document your code of conduct in a `CODE_OF_CONDUCT.md` or `CONDUCT.md` file in the package root directory, and linking to this file from the `README.md` file.  `devtools::use_code_of_conduct()` will add the Contributor Covenant template to your package.

## <a href="#pkgnaming" name="pkgnaming"></a> Compendium naming

* We strongly recommend short, descriptive names in lower case. If your compendia deals with one or more commercial services, please make sure the name does not violate branding guidelines.

## <a href="#funvar" name="funvar"></a> Function/variable naming & general syntax

* We strongly recommend `snake_case` over all other styles unless you are porting over a package that is already in wide use.

* Avoid function name conflicts with base packages or other popular ones (e.g. `ggplot2`, `dplyr`, `magrittr`, `data.table`)

* Consider an `object_verb()` naming scheme for functions in your package that take a common data type or interact with a common API. `object` refers to the data/API and `verb` the primary action.  This scheme helps avoid namespace conflicts with packages that may have similar verbs, and makes code readable and easy to auto-complete.  For instance, in **stringi**, functions starting with `stri_` manipulate strings (`stri_join()`, `stri_sort()`, and in **googlesheets** functions starting with `gs_` are calls to the Google Sheets API (`gs_auth()`, `gs_user()`, `gs_download()`).

* For functions that manipulate an object/data and return an object/data of the same type, make the object/data the first argument of the function so as to enhance compatibility with the pipe operator (`%>%`)

* For more information on how to style your code, name functions, and R scripts inside the `R/` folder, we recommend reading the [code chapter in Hadley's book](http://r-pkgs.had.co.nz/r.html).

## <a href="#docs" name="docs"></a> Documentation

* If your compendium includes exported package functions, they should be fully documented with examples.

* The package should contain top-level documentation for `?foobar`, (or
`?foobar-package` if there is a naming conflict). Optionally, you can use
	both `?foobar` and `?foobar-package` for the package level manual file,
	using `@aliases` roxygen tag.

* The package should contain at least one vignette providing an introduction to
the primary package functions and use-cases.

* As is the case for a README, top-level documentation or vignettes may
be the first point of entry for users. If your package connects to a data source or online service, or wraps other software, it should provide enough information for users to understand the nature of the data, service, or software, and provide links to other relevant data and documentation.  For instance,
a the vignette intro or documentation should not merely read, "Provides access to GooberDB," but also include,
"..., an online repository of Goober sightings in South America.  More
information about GooberDB, and documentation of database structure and metadata
can be found at *link*.

* We strongly encourage all submissions to use `roxygen2` for documentation.  `roxygen2` is [an R package](http://cran.r-project.org/web/packages/roxygen2/index.html) that automatically compiles `.Rd` files to your `man` folder in your package from simple tags written above each function.

* More information on using roxygen2 [documentation](http://r-pkgs.had.co.nz/man.html) is available on the R packages book.

* One key advantage of using `roxygen2` is that your `NAMESPACE` will always be automatically generated and up to date.

* When using `roxygen2`, add `#' @noRd` to internal functions.

## <a href="#news" name="news"></a> News

* It is mandatory to use a `NEWS` or `NEWS.md` file in the root of your package.
See the sample [NEWS file](news_template.md)
* Update the news file before every CRAN release, with a section with the package name,
version and date of release, like:

```
foobar 0.2.0 (2016-04-01)
=========================
```

* Under that header, put in sections as needed, including: `NEW FEATURES`, `MINOR IMPROVEMENTS`,
`BUG FIXES`, `DEPRECATED AND DEFUNCT`. Under each header list items as needed. For each item give
a description of the new feature, improvement, bug fix, or deprecated function/feature. Link
to any related GitHub issue like `(#12)`. The `(#12)` will resolve on GitHub in Releases to a
link to that issue in the repo.
* After you have added a `git tag` and pushed up to GitHub, add the news items for that
tagged version to a Release on the Releases tab in your GitHub repo with a title like
`pkgname v0.1.0`
* If you use `NEWS`, add it to `.Rbuildignore`, but not if you use `NEWS.md`

## <a href="#authorship" name="authorship"></a> Authorship

The `DESCRIPTION` file of a package should list compendium authors and contributors to a compendium, using the `Authors@R` syntax to indicate their roles (author/creator/contributor etc.) if there is more than one author. See [this section of "Writing R Extensions"](https://cran.rstudio.com/doc/manuals/r-release/R-exts.html#The-DESCRIPTION-file) for details.  If you feel that your reviewers have made a substantial contribution to the development of your compendium, you may list them in the `Authors@R` field with a Reviewer contributor type (`"rev"`), like so:

    person("Lincoln", "Mullen", role = c("rev"), comment = "Lincoln Mullen reviewed the package for rOpenSci, see https://github.com/ropensci/onboarding/issues/45")

Only include reviewers after asking for their consent.

## <a href="#testing" name="testing"></a> Testing

* All compendia should pass `R CMD check`/`devtools::check()` on all major platforms.

* All compendia with exported functions should have a test suite that covers major functionality of
these functions.

* We recommend using `testthat` for writing tests. Strive to write tests as you write each new function. This serves the obvious need to have proper testing for the package, but allows you to think about various ways in which a function can fail, and to _defensively_ code against those. [More information](http://r-pkgs.had.co.nz/tests.html).

* `testthat` has a function `skip_on_cran()` that you can use to not run tests on CRAN. We recommend using this on all functions that are API calls since they are quite likely to fail on CRAN. These tests will still run on Travis.

* Check the extent of your test coverage using the [**covr** package](https://github.com/jimhester/covr). Including a coverage badge in your package's README makes it easy for reviewers to see how well-tested your package is. 100% coverage is not required, but editors
and reviewers will use coverage reports as a starting point to evaluate that
your test suite covers the important functionality of your package.

* Even if you use [continuous integration](#ci), we recommend that you run tests locally prior to submitting your package, as some tests are often skipped. (You may need
to set `Sys.setenv(NOT_CRAN="true")` in order to ensure all tests are run.) In addition, we recommend that prior to submitting your package, you use Gabor Csardi's [**goodpractice**](https://github.com/MangoTheCat/goodpractice/) package to check your package for likely sources of errors, and run `devtools::spell_check()` to 
find spelling errors in documentation.

## <a href="#ver" name="ver"></a> Versioning

* We strongly recommend that DataONE compendia use semantic versioning. A detailed explanation is available on the [description chapter](http://r-pkgs.had.co.nz/description.html#version).

* Git tag each release after every submission relating to the publication/dissemination process (e.g. at initial submission, after revisions arising from peer review, after final submission). [[more info](http://marker.to/ZYd3kZ)]

## <a href="#ci" name="ci"></a> Continuous integration

* All DataONE compendia are recommended to use one form of continuous integration. This ensures that all commits, pull requests, and new branches are run through `R CMD check`. R is now a [natively supported language on Travis-CI](http://blog.travis-ci.com/2015-02-26-test-your-r-applications-on-travis-ci/), making it easier than ever to do continuous integration. See [R Packages](http://marker.to/NEr8Bd) and Julia Silge's [Beginner's Guide to Travis-CI for R](http://juliasilge.com/blog/Beginners-Guide-to-Travis/) for more help. Travis offers continuous integration for Linux and Mac OSX. For continuous integration on Windows, see [R + Appveyor](https://github.com/krlmlr/r-appveyor). R packages that use compiled code or link
to other libraries or languages should have CI for all platforms.

* R packages that use compiled code or link to other libraries or languages should have CI for all platforms.

* Continuous integration should also include reporting of test coverage via
a testing service such [CodeCov](https://codecov.io/) or [Coveralls](https://coveralls.io/).  See the [README for the **covr** package](https://github.com/jimhester/covr) for instructions, as well
as `devtools::use_coverage()`. 

* Both test status and code coverage should be reported via a badge in your
package README.

## <a href="#deps" name="deps"></a> Package dependencies

* Use `Imports` instead of `Depends` for packages providing functions from other
packages. Make sure to list packages used for testing (`testthat`), and documentation (`knitr`, `roxygen2`) in your `Suggests` section of package dependencies. If you use any packages in your examples sections, make sure to list those, if not already listed elsewhere, in `Enhances` section of package dependencies.

* For most cases where you must expose functions from dependencies to the user,
you should import and re-export those individual functions rather than listing
them in the `Depends` fields.  For instance, if functions in your package produce
`raster` objects, you might re-export only printing and plotting functions from the
**raster** package.

## <a href="#tools" name="tools"></a> Recommended scaffolding

* For http requests we strongly recommend using `httr` over `RCurl`.
* For parsing JSON, use `jsonlite` instead of `rjson` or `RJSONIO`.
* For parsing, creating, and manipulating XML, we strongly recommend `xml2` for most cases.

## <a href="#messages" name="messages"></a> Console messages

* Use `message()` and `warning()` to communicate with the user in your functions. Please do not use `print()` or `cat()` unless it's for a `print.*()` method, as these methods of printing messages are harder for the user to suppress.

##  <a href="#misc" name="messages"></a> Miscellaneous

This is a collection of gotchas that are worth avoiding at the outset.


## <a href="#further" name="further"></a> Further guidance

* Hadley Wickham's _R Packages_ is an excellent, readable resource on package development and is available a a [free book on the web](http://r-pkgs.had.co.nz/).

* [Writing R Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html) is the canonical, usually most up-to-date, reference for creating R packages.

* If you are submitting a package to DataONE via the [onboarding repo](https://github.com/ropensci/onboarding), you can direct further questions to the rOpenSci team in the issue tracker, or in our [discussion forum](https://discuss.ropensci.org/).

## <a href="#suggestions" name="suggestions"></a> Suggestions and updates

* These packaging guidelines are a work in progress for packages contributed to the rOpenSci suite. Corrections, suggestions and general improvements are welcome as [issue submissions in this repository](https://github.com/ropensci/onboarding/issues?utf8=%E2%9C%93&q=is%3Aissue%20label%3Ameta%20). Open discussions are welcome in our [forum](https://discuss.ropensci.org/).
