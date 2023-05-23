FROM ubuntu:rolling

ARG dovitoollink="https://github.com/quietvoid/dovi_tool/releases/download/2.0.3/dovi_tool-2.0.3-x86_64-unknown-linux-musl.tar.gz"
ARG hdr10plustoollink="https://github.com/quietvoid/hdr10plus_tool/releases/download/1.6.0/hdr10plus_tool-1.6.0-x86_64-unknown-linux-musl.tar.gz"
ARG pgs2srtlink="https://github.com/Tentacule/PgsToSrt/releases/download/v1.4.2/PgsToSrt-1.4.2.zip"
ARG tesseractlink="https://github.com/tesseract-ocr/tessdata.git"

COPY /dvmkv2mp4 /usr/local/bin

RUN chmod a+x /usr/local/bin/dvmkv2mp4

RUN \
  printf "\n---Install OS dependencies ---\n\n" && \
  apt update && \
  apt install -y \
    wget lsb-release git unzip

RUN \
  printf "\n---Install ffmpeg, mediainfo, mkvtoolnix, gpac, libtesseract5, dotnet-sdk-6.0, jq, bc---\n\n" && \
  wget -qO /etc/apt/trusted.gpg.d/mediaarea.asc https://mediaarea.net/repo/deb/ubuntu/pubkey.gpg && \
  echo "deb https://mediaarea.net/repo/deb/ubuntu $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/mediaarea.list && \
  apt install -y \
    ffmpeg \
    mediainfo \
    mkvtoolnix \
    gpac \
    libtesseract5 \
    dotnet-sdk-6.0 \
    jq \
    bc

# DOVI_TOOL
RUN \
  printf "\n---Download dovi_tool---\n\n" && \
  wget -O - ${dovitoollink} | \
  tar -zx -C /usr/local/bin/

# HDR10PLUS_TOOL
RUN \
  printf "\n---Download hdr10plus_tool---\n\n" && \
  wget -O - ${hdr10plustoollink} | \
  tar -zx -C /usr/local/bin/

# PGS2SRT
RUN \
  printf "\n---Download pgs2srt---\n\n" && \
  mkdir /opt/PgsToSrt && \
  wget ${pgs2srtlink} -O temp.zip && \
  unzip -d /opt/PgsToSrt/ temp.zip && \
  rm temp.zip && \
  printf "\n---Download tesseract models---\n\n" && \
  cd /opt/PgsToSrt/net6 && \
  git clone ${tesseractlink}

# Cleanup
RUN \
  printf "\n---Uninstall unzip and git and cleanup apt cache---\n\n" && \
  apt-get purge -y software-properties-common build-essential pkg-config git zlib1g-dev wget unzip git && \
  apt-get autoremove -y && \
  rm -rf \ 
    /var/lib/apt/lists/* \
    /var/tmp/*

RUN \
  printf "\n---Create \"convert\" directory for volume attachment---\n\n" && \
  mkdir /convert

VOLUME ["/convert"]

WORKDIR /convert

ENTRYPOINT ["dvmkv2mp4"]
