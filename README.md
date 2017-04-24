The "copynumber" R package with support for hg38
================================================

This R package is a fork of the 
["copynumber" package](https://bioconductor.org/packages/release/bioc/html/copynumber.html)
with minimal modifications to support human genome build hg38. The default behavior is
unchanged, and it should be safe to use this as a drop-in replacement for the original
package.

Note that the default assembly is still hg19 -- so if you want to use hg38 you must
specify it explicitly, e.g. 

      wins.logR <- winsorize(logR, assembly = "hg38")    
      aspcf.segments <- aspcf(wins.logR,BAF, assembly = "hg38")


Installation
------------------------------


You can install this package directly from GitHub like this:

	library("devtools")
	install_github("aroneklund/copynumber")


How did I create the hg38 object?
------------------------------

    ## download cytoband file for hg38
    ##   http://hgdownload.cse.ucsc.edu/goldenpath/hg38/database/cytoBand.txt.gz
    
    a <- read.delim("cytoBand.txt.gz", header = FALSE)
    a2 <- a[a$V1 %in% c('chrX', 'chrY', paste0('chr', 1:22)), ]
    a3 <- a2
    a3$V1 <- factor(a3$V1)
    a3$V4 <- factor(a3$V4)
    rownames(a3) <- seq(1:nrow(a3))
    hg38 <- a3
    
    ## the cytoband data for various genome builds are in this file
    oldthings <- load('sysdata.rda')
    
    ## we just add hg38 and leave the rest as it was
    save(list = c(oldthings, "hg38"), file = 'sysdata.rda')
