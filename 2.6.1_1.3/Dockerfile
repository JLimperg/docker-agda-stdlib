# UPDATE for new version
FROM haskell:8.8.3 AS builder

# UPDATE for new version
ARG agda_version=2.6.1
ARG agda_stdlib_version=1.3
ARG ghc_version=8.8.3

# Workaround for a Happy bug/misfeature. With the default locale, Happy does
# not recognise UTF-8 files.
ENV LC_ALL=C.UTF-8

# install Agda
RUN git clone --depth 1 -b "v${agda_version}" https://github.com/agda/agda.git /root/.agda/src
RUN stack --stack-yaml /root/.agda/src/stack-"${ghc_version}".yaml install
RUN stack --stack-yaml /root/.agda/src/stack-"${ghc_version}".yaml clean

# UPDATE for new version
ARG agda_stdlib_version=1.3

# get agda-stdlib sources
RUN mkdir -p /root/.agda/lib
RUN git clone --depth 1 -b "v${agda_stdlib_version}" https://github.com/agda/agda-stdlib.git /root/.agda/lib/standard-library

# generate agda-stdlib's Everything.hs
WORKDIR /root/.agda/lib/standard-library
RUN stack --resolver "ghc-${ghc_version}" script --package filemanip --package filepath --package unix-compat --package directory --system-ghc -- GenerateEverything.hs

# generate .agdai files for agda-stdlib
WORKDIR /root/.agda/lib/standard-library
RUN mv Everything.agda src/
RUN agda --verbose=0 src/Everything.agda
RUN mv src/Everything.agda .
RUN rm _build/${agda_version}/agda/src/Everything.agdai


# UPDATE for new version
FROM haskell:8.8.3

# UPDATE for new version
ARG agda_version=2.6.1
ARG agda_stdlib_version=1.3
ARG ghc_version=8.8.3
ARG snapshot_hash=1e00b9ab146f000e5939314a3f7d999792a6fde726b96f5a16d953eaba093987

# copy Agda binaries
COPY --from=builder /root/.local/bin/ /root/.local/bin/

# copy library files for Agda primitives
COPY --from=builder \
  /root/.agda/src/.stack-work/install/x86_64-linux/${snapshot_hash}/${ghc_version}/share/ \
  /root/.agda/src/.stack-work/install/x86_64-linux/${snapshot_hash}/${ghc_version}/share/

# copy libraries
COPY --from=builder /root/.agda/lib/ /root/.agda/lib/

# register agda-stdlib
COPY libraries /root/.agda/

# test-run Agda
CMD agda
