R installation
==============

Recent release of R can be obtained from The [CRAN web
site](https://cran.r-project.org) (for Windows choose *base*
distribution). There is rather no point to install both 32bit and 64bit
versions - simply **use the 64bit** one (unless you have very, very old
computer or [really know that you need
this](https://cran.r-project.org/bin/windows/base/rw-FAQ.html#Should-I-run-32_002dbit-or-64_002dbit-R_003f)).

Installers of the RStudio IDE are available on its [web
site](https://www.rstudio.com/products/rstudio/download/#download).

You will need some additional programs if you want to build your own R
packages or perform some more advanced package management (see section
about installing packages from sources other than CRAN below). Unless
you don’t, R and RStudio will be sufficient.

Packages
========

Functions you use to manipulate data and perform analysis in R are
usually organized into libraries, that are called *packages* in the R
ecosystem. Some packages are ready to use just as you launch R. However
other you will probably want to use aren’t. So what do you need to do?
First, you will have to install packages on your computer. Second, you
will have to load package to your current session of R.

Installing packages
-------------------

### From CRAN

Most packages you will want to use are available through official R
packages repository called [CRAN](https://cran.r-project.org)
(Comprehensive R Archive Network). Perhaps you will want to look at
[Task Views](https://cran.r-project.org/web/views) when searching for a
proper package to perform some specific operations.

If you know the name of a package you want to install from CRAN, use
function `install.packages()` providing it a string with a package name.
For example to install package *tidyr*:

``` r
install.packages("tidyr")
```

You can provide many package names at once, separating each name given
as a string with comma and surrounding all the provided names with `c(`
and `)`:

``` r
install.packages(c("dplyr", "readr", "readxl", "writexl"))
```

### From other sources

**This is an advanced topic** - you may skip it.

Sometimes you may want to get packages that aren’t on CRAN, typically
because you want to test a development version (that hasn’t been already
sent to CRAN) with some new interesting feature or a patch you badly
need. Most often you will have to install such a package (versions) from
GitHub or GitLab.

Installing packages from sources other than CRAN **on Windows require
that you have [MiKTeX](https://miktex.org) and
[Rtools](https://cloud.r-project.org/bin/windows/Rtools) installed** on
your machine (on Linux you probably have all the needed software already
available). When installing MiKTeX remember to choose that TeX packages
missing in your local installation should be installed on the fly. When
installing Rtools choose to edit PATH variable and assure that it
contains paths to:

-   (…)\\Rtools\\bin\\;
-   (…)\\Rtools\\mingw\_64\\bin\\;
-   (…)\\R-\[version\]\\bin\\x64\\;
-   (…)\\MikTeX 2.9\\miktex\\bin\\x64\\;

where “(…)” stands for a directories where respectively Rtools or R or
MiKTex folders has been placed during installation and “\[version\]” is
a version number of installed R release (i.e. 3.5.3).

**Also on Mac there is some additional work needed** to enable
installation from other sources - see [R for Mac OS
X](https://cran.r-project.org/bin/macosx/) web page on CRAN.

Assuming you have all the needed additional software, you should use the
*remotes* R package (so install it with: `install.packages("remotes")`).
It provides functions `install_github()` and `install_gitlab()` that
allows easy installation of packages from GitHub and GitLab respectively
(and in fact also some other functions to install packages from even
other sources). See [package website](https://github.com/r-lib/remotes)
and function documentation (run: `?install_github()` or
`?install_gitlab()` in R console) to get know how to use them.

Loading packages
----------------

When you have a package installed on your machine, you still need to
load it to make its functions available in the running R session. To do
so, use function `library()`. For a somewhat complicated reasons:

-   you can load only one package at once (so if you want to load more
    packages, you need to call `library()` separately for each of them);
-   package name provided to `library()` need not be quoted.

For example to load package *tidyr* use:

``` r
library(tidyr)
```

Dependencies and conflicts
--------------------------

As you perhaps noticed, installing a package with `install.packages()`
(or `install_github()` or `install_gitlab()`) often results also in
installation of other packages. These are packages that the package you
wanted to install depend on, i.e. it uses some functions from these
other packages and if they aren’t installed, it won’t work.

Having many packages on your machine may sometimes cause troubles.
Because R at its roots is not an objective language (and the oldest and
still very widely used objective model in R is somewhat weird - at least
to typical programmers) there may be *conflicts* between packages if
there are functions with the same name in both of them. For example when
we load package *dplyr*:

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

We get informed that there are functions named `filter` and `lag` in
*dplyr* that exists also in package *stats* and there are functions
named `intersect`, `setdiff`, `setequal` and `union` that exists also in
package *base*. These two packages: *base* and *stats* are a part of
basic R installation and are loaded by default at the beginning of every
R session. Loading package *dplyr* makes functions from packages *base*
and *stats* listed above been *masked* by functions from package
*dplyr*. From now on calling for example `filter()` will invoke function
defined in *dplyr*, not in *stats*.

Possibility of such conflicts is one of the reasons why R doesn’t load
all the installed packages at the begging of a session, but lets you
decide yourself which one you need this time.

Luckily functions that have been *masked*, have not been actually
*overwritten* - they still exists, however it is harder to get to them.
If you want to use them, you must explicitly state that this function is
defined within the specific package. To do so, use `::` operator. For
example calling `filter()` from package *stats* looks like this:

``` r
stats::filter()
```

In the same manner you can call functions from packages that are
installed on your machine, but hasn’t been loaded yet using `library()`.
For example you can call function `read_excel()` from package *readxl*
with:

``` r
readxl::read_excel("file name.xlsx")
```

Managing packages
-----------------

### Updating packages

Packages get updated from time to time patching bugs and adding some new
features (if you want to follow the changes in the whole R ecosystem
[CRANberries](http://dirk.eddelbuettel.com/cranberries) site may be of
your interest). You can update all the packages installed on your
machine by calling:

``` r
update.packages()
```

By default you will be asked to confirm (or decline) update of every
package for which a newer version is available. Also you may be asked
whether you want to build some packages from source - if you’re not sure
what’s this about, decline.

**It is very good to read NEWS files of the updated packages** to get
know, what has changed. I find it most convenient to do this using CRAN
website of a package (for example
[dplyr](https://cran.r-project.org/web/packages/dplyr/index.html) -
search this site for “NEWS”). You may find such a website for example
using CRAN page with [list of available
packages](https://cran.r-project.org/web/packages/available_packages_by_name.html).

In principle update is a change for better. However…

![xkcd.com:1172 “Workflow”](https://imgs.xkcd.com/comics/workflow.png)

You should be aware, that **sometimes things that have worked before may
fail after package update**. More probably some day you may notice that
some function you used to use has been marked as *depreciated*. Such a
function will still be available and working (at least for some time
on), however this means that the package developers want to discourage
you from using this function (typically wanting you to use some other
function they’ve introduced instead). Generally, the more *matured* the
package you use (compare [this
site](https://www.tidyverse.org/lifecycle/)), and the more people use
it, the less the probability of breaking changes.

If you want to **downgrade to some previous version of a package**, you
may use function `install_version()` from package *remotes*. For
example:

``` r
# at the moment 0.8.3 is the most recent version of the package on CRAN
remotes::install_version("tidyr", "0.8.2")
# let's return to the most recent version
update.packages()
```

However be aware, that it may need additional software to be installed
on your computer (see section about installing packages from other
sources above).

### R updates

R itself is also updated. *Big* releases (having .0 at the end of the
version number, i.e. 3.5.0), introducing more important and potential
breaking changes are prepared annually, at the turn of April and May.
*Small* releases (i.e. 3.5.3), containing mostly bug fixes and minor
improvements are released 2-3 times a year. You may watch announced
dates of release on [R Homepage](https://www.r-project.org/) or on [R
Foundation Tweeter](https://twitter.com/_R_Foundation). Installation
files (as you probably know) can be downloaded from
[CRAN](https://cran.r-project.org).

*Important:* After updating your R with *small* releases, you don’t need
to reinstall your packages, however ***big* releases require
reinstallation of all the packages.** So it is convenient to have a
script with `install.packages()` call prepared listing all the packages
you (often) use, so you can run it after updating to *big* release.

It is good to wait at least a week (in case of *big* releases even
longer) after release of the new R version before installing it. You
should give package developers some time so they’re able to assure that
their packages work well with this new version. Otherwise you may
encounter an error trying to install a package, that will say something
like *package “some package” is unavailable for R version X.X.X*.

### Good practices

Given you’re working in a *production environment* it can be advised
that you:

-   adapt a department-wide (or even wider) policy regarding package
    updates;
-   perhaps not update packages too often;
-   rather update packages and R itself everybody at the same time;
-   perhaps introduce some procedures to test if update doesn’t brake
    your workflow before making decision whether everybody should
    update;
-   if you have some scripts you won’t be using for some time but it’s
    probable you may want to reuse them in the more distant future, you
    should note down within this scripts what were the versions of
    packages the script was running on.

You may also note that there exists some tools that helps to manage
package versions (and keeping them stable within a given project), like
[packrat](https://rstudio.github.io/packrat/). It may be worth to use it
in the future.

Working environment
===================

Getting help
------------

If you want to get help about how to use some function, type its name in
a console proceeded by ‘?’:

``` r
?cumsum
# works also when ? proceeds actual function call (without resolving a call)
?cumsum(1:10)
```

This work also for operators like `+` or `%/%`, however you must enclose
it with backticks:

``` r
?cumsum
?`%/%`
# with actual call backticks are no longer needed
?7 %/% 3
```

You may also want to look for package *vignettes* - documents attached
to some packages describing typical (or specific) ways it can be used:

``` r
browseVignettes("dplyr")
```

RStudio pane “Help” also should be useful.

Working directory
-----------------

If you want to read something from or write something to disc, you need
to describe the location of a file. You can do this in two ways:

-   using *absolute* path to file,
    -   every path that starts from something pointing at the particular
        drive (in a given operating system), for example: “D:/my
        favorite docs/some document.txt”;
-   using *relative* path to file,
    -   every path that is not *absolute*, for example: “../some other
        folder/my file.csv”.

Using *absolute* paths is not recommended because:

-   they’re usually very long (but remember that “\~/” stands for your
    system *home directory*),
-   they’re not *portable*, that is they almost always point at a
    nonexistent place if you (or somebody else) try to use your script
    on a different computer,
-   they may contain some information regarding where exactly you store
    data on your computer, you may don’t want to share with others (and
    having this information written down in a script rises probability
    of unintentionally sharing it).

So you should use relative paths and that’s the point, where concept of
a *working directory* becomes important. When R sees *relative* path it
tries to follow it starting from current *working directory*. You can
always check what is this directory with:

``` r
# this function returns absolute path
getwd()
```

    ## [1] "D:/Prace/Socjologia cyfrowa/zajecia/3502-SCC-ADR/dodatkowe"

You can also change your *working directory* using function `setwd()`
(providing it either *absolute* or *relative* path):

``` r
setwd("data")
getwd()
# "../" stands for going to a parrent directory
setwd("../")
# so we're back again
getwd()
# you can also provide absolute path
setwd("D:/some folder/data")
```

However there are some limitations of using `setwd()` in R notebooks -
if you try, you could read about them in a warning that will show.

Sometimes you may also want to use functions `list.files()` or
`list.dirs()` to look up for files/directories in your *working
directory* (or in fact any other directory you want).

But what is R’s default working directory? It depends on situation:

-   If you work with R notebook working directory is always a directory
    in which notebook is stored.
    -   You can change working directory in a code chunk but it will be
        restored to directory in which notebook is stored as soon as a
        chunk stops running.
-   If you start RStudio without opening R (alike) file: directory set
    in RStudio options: *Tools-\>Global Options…-\>General-\>Default
    working directory (when not in project)*.
-   If you open R (alike) file causing RStudio to start (to open it):
    directory in which this file is located.
    -   A special case is opening a RStudio project file. Note that
        opening this file may cause some script files be restored in
        your *Source* pane and these files may be placed in some other
        (sub)directories than the project *root* directory; nevertheless
        *working directory* will be set to the project *root* directory.

Saving and loading working environment between sessions
-------------------------------------------------------

When you close R/RStudio it typically (by default) ask you, whether you
want to save workspace image to “.RData” file (in your *working
directory*). This is a feature that enables you to continue your work in
the future *starting where you left off* - by default just as you start
R it checks its *working directory* and if it finds file “.RData” it
loads its content into working environment, restoring the previous
session (however note, that this doesn’t apply to loading packages).
Also a history of command lines command is saved/loaded in the same time
(but it is stored in a separate file “.Rhistory”). It may look like a
convenient feature, however I wouldn’t recommend using it. That’s
because:

-   nowadays you may turn your computer into sleep mode without closing
    your applications, so there’s really little need to store workspace
    content between R sessions (you can simply don’t terminate a
    session);
-   working very long using the same R session is a habit that kills
    reproducibility:
    -   almost always there are some things you wrote manually into
        console but not put into the script (or things that were
        executed but then deleted from the script) and too often it
        turns out in the future, that your script doesn’t work properly
        without some of these objects you created at some point, but
        your final script doesn’t create them when run on *fresh* R
        session
    -   so in practice you need rather to clear your workspace from time
        to time during one session (you can do this using *Clear
        Workspace…* option from the *Build* menu in RStudio or
        equivalently executing `rm(list = ls())` in the console), than
        save and restore workspace between different sessions;
    -   and if your script is supposed to save some results it produces,
        it should do it explicitly (for example invoking `save()`)
        rather then relying on something happens when user closing R
        session.

Interestingly you can turn this feature off completely or make it
perform automatically without asking you at the end of each session
using general RStudio options (*Tools-\>Global Options…-\>General*) and
project options (*Tools-\>Project Options…-\>General*). For the reasons
outlined above I won’t recommend turning it *fully on*. Leaving the
default option have some advantage - it gives you a protection against
closing R session by accident (you can click “cancel” when asked about
saving a workspace causing R not to close). If it’s not important to
you, you should rather turn it off.

Write your code in Unicode!
===========================

For interoperability reasons you should **absolutely always use UTF-8
encoding for your R script files** (if you use Linux or OS X operating
system, typically this will be a default option, however with Microsoft
Windows not). To assure this, set RStudio options accordingly: change
*Tools*-\>*Global Options…*-\>*Code*-\>*Saving* tab-\>*Default text
encoding* to UTF-8.
