FROM haskell:8.4.3
LABEL maintainer="Jannis Limperg <jannis@limperg.de>"
RUN stack --resolver lts-12.4 install Agda
RUN mkdir -p /root/.agda/lib && \
    git clone --depth 1 -b v0.16 https://github.com/agda/agda-stdlib.git /root/.agda/lib/standard-library
COPY libraries /root/.agda/
CMD agda
