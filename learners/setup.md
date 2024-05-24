---
title: Setup
---


::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Setup instructions live in this document. Please specify the tools and the data sets the learner needs to have installed. If you want to hide different setup instructions, you can use a `solution` tag.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

### Install R and RStudio

R and RStudio are two separate pieces of software:

* **R** is a programming language and software used to run code written in R.
* **RStudio** is an integrated development environment (IDE) that makes using R easier.

If you don't already have R and RStudio installed, follow the instructions for your operating system below. You have to install R before you install RStudio.

<br>

:::::::::::::::: solution

## For Windows

* Download R from the [CRAN website](https://cran.r-project.org/bin/windows/base/release.htm).
* Run the `.exe` file that was just downloaded
* Go to the [RStudio download page](https://www.rstudio.com/products/rstudio/download/#download)
* Under *Installers* select **RStudio x.yy.zzz - Windows Vista/7/8/10** (where x, y, and z represent version numbers)
* Double click the file to install it
* Once it's installed, open RStudio to make sure it works and you don't get any error messages.

:::::::::::::::::::::::::

:::::::::::::::: solution

## For MacOS

* Download R from the [CRAN website](https://cran.r-project.org/bin/macosx/).
* Select the `.pkg` file for the latest R version
* Double click on the downloaded file to install R
* Go to the [RStudio download page](https://www.rstudio.com/products/rstudio/download/#download)
* Under *Installers* select **RStudio x.yy.zzz - Mac OS X 10.6+ (64-bit)** (where x, y, and z represent version numbers)
* Double click the file to install RStudio
* Once it's installed, open RStudio to make sure it works and you don't get any error messages.

:::::::::::::::::::::::::

:::::::::::::::: solution

## For Linux

* Download R from the [CRAN website](https://cran.r-project.org/bin/macosx/).
* Select the `.pkg` file for the latest R version
* Double click on the downloaded file to install R
* Go to the [RStudio download page](https://www.rstudio.com/products/rstudio/download/#download)
* Under *Installers* select **RStudio x.yy.zzz - Mac OS X 10.6+ (64-bit)** (where x, y, and z represent version numbers)
* Double click the file to install RStudio
* Once it's installed, open RStudio to make sure it works and you don't get any error messages.

:::::::::::::::::::::::::


### Install required R packages

During the course we will need a number of R packages. Packages contain useful R code written by other people.

To install these packages, open RStudio and copy and paste the following command into the console window (look for a blinking cursor on the bottom left), then press the <kbd>Enter</kbd> (Windows and Linux) or <kbd>Return</kbd> (MacOS) to execute the command.

```{r}
install.packages(c("tidyverse", "lubridate", "mapview", "sf"))
```

R will download and install the packages on your machine.

When the installation has finished, you can try to load the packages by pasting the following code into the console:

```r
library(readr)
library(ggplot2)

```

If you do not see an error like `there is no package called ‘...’` you are good to go!
