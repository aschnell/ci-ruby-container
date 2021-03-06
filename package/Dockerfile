
#!BuildTag: yast-ruby

FROM opensuse/tumbleweed

# do not install the files marked as documentation (use "rpm --excludedocs")
RUN sed -i -e "s/^.*rpm.install.excludedocs.*/rpm.install.excludedocs = yes/" /etc/zypp/zypp.conf

# import the YaST OBS GPG key
COPY YaST:Head.pub /usr/share/gpg-keys/
RUN rpm --import /usr/share/gpg-keys/YaST:Head.pub

# Prefer the packages from the YaST:Head repository
# TODO -p 50 does not work, but should be usless anyway, just like --refresh
RUN zypper addrepo --refresh http://download.opensuse.org/repositories/YaST:/Head/openSUSE_Tumbleweed yast

RUN zypper --non-interactive install --no-recommends \
  brp-check-suse \
  brp-extract-appdata \
  aspell-en \
  fdupes \
  git \
  grep \
  rpm-build \
  update-desktop-files \
  which \
  libxml2-tools \
  libxslt-tools \
  "rubygem(abstract_method)" \
  "rubygem(cfa)" \
  "rubygem(cheetah)" \
  "rubygem(coveralls)" \
  "rubygem(gettext)" \
  "rubygem(parallel)" \
  "rubygem(parallel_tests)" \
  "rubygem(raspell)" \
  "rubygem(rspec)" \
  "rubygem(rubocop)" \
  "rubygem(simplecov)" \
  "rubygem(simpleidn)" \
  "rubygem(suse-connect)" \
  "rubygem(yard)" \
  "rubygem(yast-rake)" \
  build \
  obs-service-source_validator \
  openSUSE-release-ftp \
  patterns-rpm-macros \
  ShellCheck \
  yast2 \
  yast2-add-on \
  yast2-bootloader \
  yast2-core \
  yast2-country \
  yast2-devtools \
  yast2-hardware-detection \
  yast2-inetd \
  yast2-installation \
  yast2-installation-control \
  yast2-ldap \
  yast2-network \
  yast2-nfs-server \
  yast2-packager \
  yast2-pam \
  yast2-perl-bindings \
  yast2-pkg-bindings \
  yast2-proxy \
  yast2-ruby-bindings \
  yast2-security \
  yast2-services-manager \
  yast2-slp \
  yast2-storage-ng \
  yast2-testsuite \
  yast2-transfer \
  yast2-update \
  yast2-users \
  yast2-xml \
  yast2-ycp-ui-bindings \
  && zypper clean --all \
  && rm -rf /usr/lib*/ruby/gems/*/cache/ \
  && rm -rf /usr/share/doc/

COPY yast-travis-ruby /usr/local/bin/
RUN chmod a+x /usr/local/bin/yast-travis-ruby
ENV LC_ALL=en_US.UTF-8
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# just some smoke tests, make sure rake and YaST work properly,
# ensure there is no leftover in the working directory
RUN rake -r yast/rake -V && TERM=xterm yast2 proxy summary && \
  rm -rf /var/log/YaST2/y2log && rm -rf /usr/src/app/*
