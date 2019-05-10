FROM haskell:8.4.4
LABEL maintainer="Jannis Limperg <jannis@limperg.de>"

# install Agda
RUN git clone --depth 1 -b v2.6.0 https://github.com/agda/agda.git /root/.agda/src && \
    stack --stack-yaml /root/.agda/src/stack-8.4.4.yaml install && \
    stack --stack-yaml /root/.agda/src/stack-8.4.4.yaml clean && \
    rm -rf /root/.stack/build-plan /root/.stack/build-plan-cache /root/.stack/indices /root/.stack/loaded-snapshot-cache /root/.stack/setup-exe-cache /root/.stack/setup-exe-src

# get agda-stdlib sources
RUN mkdir -p /root/.agda/lib && \
    git clone --depth 1 -b v1.0.1 https://github.com/agda/agda-stdlib.git /root/.agda/lib/standard-library

# generate agda-stdlib's Everything.hs
RUN cd /root/.agda/lib/standard-library && \
    stack --resolver lts-12.26 script --package filemanip --package filepath --system-ghc -- GenerateEverything.hs && \
    rm -rf /root/.stack/build-plan /root/.stack/build-plan-cache /root/.stack/indices /root/.stack/loaded-snapshot-cache /root/.stack/setup-exe-cache /root/.stack/setup-exe-src

# generate .agdai files for agda-stdlib
RUN cd /root/.agda/lib/standard-library && \
    mv Everything.agda src/ && \
    agda --verbose=0 src/Everything.agda && \
    mv src/Everything.agda src/Everything.agdai .

# register agda-stdlib
COPY libraries /root/.agda/
CMD agda
