FROM haskell:8.4.4
LABEL maintainer="Jannis Limperg <jannis@limperg.de>"
RUN git clone --depth 1 -b v2.6.0 https://github.com/agda/agda.git /root/.agda/src && \
    stack --stack-yaml /root/.agda/src/stack-8.4.4.yaml install && \
    stack --stack-yaml /root/.agda/src/stack-8.4.4.yaml clean && \
    rm -rf /root/.stack/build-plan /root/.stack/build-plan-cache /root/.stack/indices /root/.stack/loaded-snapshot-cache /root/.stack/setup-exe-cache /root/.stack/setup-exe-src
RUN mkdir -p /root/.agda/lib && \
    git clone --depth 1 -b v1.0 https://github.com/agda/agda-stdlib.git /root/.agda/lib/standard-library
COPY libraries /root/.agda/
CMD agda
