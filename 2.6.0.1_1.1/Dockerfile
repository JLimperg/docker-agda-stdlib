FROM haskell:8.6.5 AS builder

# Don't forget to set the same ARGs below!
ARG agda_version=2.6.0.1
ARG ghc_version=8.6.5
ARG stackage_snapshot=13.21

# install Agda
RUN git clone --depth 1 -b "v${agda_version}" https://github.com/agda/agda.git /root/.agda/src
RUN stack --stack-yaml /root/.agda/src/stack-"${ghc_version}".yaml install
RUN stack --stack-yaml /root/.agda/src/stack-"${ghc_version}".yaml clean

# Don't forget to set the same ARG below!
ARG agda_stdlib_version=1.1

# get agda-stdlib sources
RUN mkdir -p /root/.agda/lib
RUN git clone --depth 1 -b "v${agda_stdlib_version}" https://github.com/agda/agda-stdlib.git /root/.agda/lib/standard-library

# generate agda-stdlib's Everything.hs
WORKDIR /root/.agda/lib/standard-library
RUN stack --resolver "lts-${stackage_snapshot}" script --package filemanip --package filepath --system-ghc -- GenerateEverything.hs

# generate .agdai files for agda-stdlib
WORKDIR /root/.agda/lib/standard-library
RUN mv Everything.agda src/
RUN agda --verbose=0 src/Everything.agda
RUN mv src/Everything.agda src/Everything.agdai .



FROM haskell:8.6.5

ARG agda_version=2.6.0.1
ARG agda_stdlib_version=1.1
ARG ghc_version=8.6.5
ARG stackage_snapshot=13.21

# copy Agda binaries
COPY --from=builder /root/.local/bin/ /root/.local/bin/

# copy library files for Agda primitives
COPY --from=builder \
  /root/.agda/src/.stack-work/install/x86_64-linux/lts-${stackage_snapshot}/${ghc_version}/share/ \
  /root/.agda/src/.stack-work/install/x86_64-linux/lts-${stackage_snapshot}/${ghc_version}/share/

# copy libraries
COPY --from=builder /root/.agda/lib/ /root/.agda/lib/

# register agda-stdlib
COPY libraries /root/.agda/

# test-run Agda
CMD agda
