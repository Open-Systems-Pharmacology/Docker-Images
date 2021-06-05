ARG r_version
ARG os_version
FROM ghcr.io/yuri05/base-r${r_version}-${os_version}:main

# Args are resetted by FROM - thus need to redeclare! s. https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG r_version
ARG os_version

ENTRYPOINT ["sh"]

# Copy installation scripts
RUN mkdir -p /tmp_setup 
COPY .github/workflows/*.R /tmp_setup/
COPY .github/workflows/*.sh /tmp_setup/
RUN chmod ugo+x /tmp_setup/*.sh

# Install OSP R packages
RUN /tmp_setup/install_packages_r${r_version}_${os_version}.sh

# Cleanup
RUN rm -rf /tmp_setup

# run package tests
RUN R --no-save -e "setwd(system.file('tests', package = 'tlf')); source('testthat.R')"
RUN R --no-save -e "setwd(system.file('tests', package = 'ospsuite')); source('testthat.R')"
RUN R --no-save -e "setwd(system.file('tests', package = 'ospsuite.reportingengine')); source('testthat.R')"
