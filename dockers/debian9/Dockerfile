FROM rocker/r-ver:3.6
MAINTAINER Peter Kharchenko "peter_kharchenko@hms.harvard.edu"

RUN apt-get update --yes && apt-get install --no-install-recommends --yes \
  build-essential \
  cmake \
  git \
  libbamtools-dev \
  libboost-dev \
  libboost-iostreams-dev \
  libboost-log-dev \
  libboost-system-dev \
  libboost-test-dev \
  libssl-dev \
  libcurl4-openssl-dev \
  libz-dev \
  curl \
  libhdf5-cpp-100 \ 
  libarmadillo7 \
  libarmadillo-dev

RUN \
  R -e 'chooseCRANmirror(ind=52); install.packages(c("devtools", "Rcpp","RcppArmadillo", "Matrix", "mgcv", "abind","igraph","hdf5r","Rtsne","cluster","data.table"))'

FROM bioconductor/bioconductor_docker:devel

RUN \
  R -e 'BiocManager::install(c("pcaMethods","edgeR","Rsamtools","GenomicAlignments","GenomeInfoDb","Biostrings"),suppressAutoUpdate=TRUE,ask=FALSE,suppressUpdates=TRUE)'


RUN useradd -m user
USER user
ENTRYPOINT ["/bin/bash"]
WORKDIR "/home/user"

RUN \
  git clone https://github.com/Agnes-team/velocyto.R && \
  mkdir -p ~/R/x86_64-redhat-linux-gnu-library/3.6

RUN \
  echo '.libPaths(c("~/R/x86_64-redhat-linux-gnu-library/3.6", .libPaths()))' > .Rprofile && \
  R -e 'devtools::install_local("~/velocyto.R/",dep=T,upgrade_dependencies=F)'


ENV  LD_LIBRARY_PATH=/usr/local/lib/R/lib/:$LD_LIBRARY_PATH \
  R_PROFILE=~/.Rprofile
