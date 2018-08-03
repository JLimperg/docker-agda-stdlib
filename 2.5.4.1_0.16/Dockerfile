FROM haskell:8.4.3
LABEL maintainer="Jannis Limperg <jannis@limperg.de>"
RUN stack --resolver lts-12.4 install Agda && \
    stack clean --full && \
    rm -rf /root/.stack/build-plan /root/.stack/build-plan-cache /root/.stack/indices /root/.stack/loaded-snapshot-cache /root/.stack/setup-exe-cache /root/.stack/setup-exe-src
RUN mkdir -p /root/.agda/lib && \
    git clone --depth 1 -b v0.16 https://github.com/agda/agda-stdlib.git /root/.agda/lib/standard-library
COPY libraries /root/.agda/
CMD agda
