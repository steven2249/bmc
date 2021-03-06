bmc
---------



[![Build Status](https://api.travis-ci.org/ropensci/bmc.png)](https://travis-ci.org/ropensci/bmc)
[![Build status](https://ci.appveyor.com/api/projects/status/fitnci67m76iy0bg/branch/master)](https://ci.appveyor.com/project/sckott/bmc/branch/master)

**An R interface to BMC search API and full text XML**

API DOCS: [http://www.biomedcentral.com/about/api](http://www.biomedcentral.com/about/api)

No API key is required to use the BMC API.

## Install


```r
install.packages("devtools")
devtools::install_github("ropensci/bmc")
```


```r
library("bmc")
```

## Search


```r
out <- bmc_search(terms = 'fire', limit=2)
out@results$entries[[1]]
#> $arxId
#> [1] "s12960-015-0005-7"
#> 
#> $blurbTitle
#> [1] ""
...
```

The object returned from `bmc_search` is an object of class _bmc_. The default print gives back a list of length _N_, where each element has the contents for the article in question. We can inspect further elements of the _bmc_ object with the `@` symbol. We can get the _urls_ element...


```r
out@urls
#> [1] "http://www.human-resources-health.com/content/13/1/14"
#> [2] "http://www.biomedcentral.com/1471-2458/15/302"
```

...which has the urls you can use to go the paper in a browser


```r
browseURL(out@urls[1])
```

_which opens the paper in your default browser_

We can also inspect the _ids_ element, which has a list equal to the number you requested, where each element is of length 2, with a _arxId_, and a _url_. These two are used to construct the download url if you use `bmc_xml`.


```r
out@ids
#> [[1]]
#> [[1]]$arxId
#> [1] "s12960-015-0005-7"
#> 
#> [[1]]$url
#> [1] "http://www.human-resources-health.com/content/13/1/14"
#> 
#> 
#> [[2]]
#> [[2]]$arxId
#> [1] "s12889-015-1630-8"
#> 
#> [[2]]$url
#> [1] "http://www.biomedcentral.com/1471-2458/15/302"
```

## Get full text XML

You can either pass in a url to the `uris` parameter in the `bmc_xml` function, or pass in the output of the `bmc_search` function to `bmc_xml` using the first parameter `obj`. First, passing in a url:


```r
uri <- 'http://www.biomedcentral.com/content/download/xml/1471-2393-14-71.xml'
bmc_xml(uris=uri)
#> [[1]]
#> <?xml version="1.0"?>
#> <!DOCTYPE art SYSTEM "http://www.biomedcentral.com/xml/article.dtd">
#> <art>
#>   <ui>1471-2393-14-71</ui>
#>   <ji>1471-2393</ji>
#>   <fm>
#>     <dochead>Research article</dochead>
#>     <bibl>
#>       <title>
...
```

Now the output from `bmc_search()`


```r
out <- bmc_search(terms = 'science', limit=5)
dat <- bmc_xml(out)
length(dat)
#> [1] 5
```

Inspect the xml


```r
dat[[1]]
#> <?xml version="1.0"?>
#> <!DOCTYPE art SYSTEM "http://www.biomedcentral.com/xml/article.dtd">
#> <art>
#>   <ui>s12989-015-0083-7</ui>
#>   <ji>1743-8977</ji>
#>   <fm>
#>     <dochead>Research</dochead>
#>     <bibl>
#>       <title>
#>         <p>Elevated particle number concentrations induce immediate changes in heart rate variability: a panel study in individuals with impaired glucose metabolism or diabetes</p>
...
```

## Parse and search XML

Once you have `XML` content, you can go to work with e.g., `xpath`.


```r
uri <- 'http://www.biomedcentral.com/content/download/xml/1471-2393-14-71.xml'
xml <- bmc_xml(uris=uri)
library("XML")
xpathApply(xml[[1]], "//abs", xmlValue)
#> [[1]]
#> [1] "AbstractBackgroundIn pregnancy, violence can have serious health consequences that could affect both mother and child. In Ghana there are limited data on this subject. We sought to assess the relationship between physical violence during pregnancy and pregnancy outcomes (early pregnancy loss, perinatal mortality and neonatal mortality) in Ghana.MethodThe 2008 Ghana Demographic and Health Survey data were used. For the domestic violence module, 2563 women were approached of whom 2442 women completed the module. After excluding missing values and applying the weight factor, 1745 women remained. Logistic regression analysis was performed to assess the relationship between physical violence in pregnancy and adverse pregnancy outcomes with adjustments for potential confounders.ResultsAbout five percent of the women experienced violence during their pregnancy. Physical violence in pregnancy was positively associated with perinatal mortality and neonatal mortality, but not with early pregnancy loss. The differences remained largely unchanged after adjustment for age, parity, education level, wealth status, marital status and place of residence: adjusted odds ratios were 2.32; 95% CI: 1.34-4.01 for perinatal mortality, 1.86; 95% CI: 1.05-3.30 for neonatal mortality and 1.16; 95% CI: 0.60-2.24 for early pregnancy loss.ConclusionOur findings suggest that violence during pregnancy is related to adverse pregnancy outcomes in Ghana. Major efforts are needed to tackle violence during pregnancy. This can be achieved through measures that are directed towards the right target groups. Measures should include education, empowerment and improving socio-economic status of women."
```

## Meta

* Please [report any issues or bugs](https://github.com/ropensci/bmc/issues).
* License: MIT
* Get citation information for `bmc` in R doing `citation(package = 'bmc')`

---

This package is part of a richer suite called [fulltext](https://github.com/ropensci/fulltext), along with several other packages, that provides the ability to search for and retrieve full text of open access scholarly articles. We recommend using `fulltext` as the primary R interface to `bmc` unless your needs are limited to this single source.

---

[![ropensci_footer](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)
