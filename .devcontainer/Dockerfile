FROM espressif/idf

ARG DEBIAN_FRONTEND=nointeractive

RUN apt-get update \
  && apt install -y -q \
  cmake \
  git \
  hwdata \
  libglib2.0-0 \
  libnuma1 \
  libpixman-1-0 \
  linux-tools-virtual \
  && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/local/bin/usbip usbip `ls /usr/lib/linux-tools/*/usbip | tail -n1` 20

# QEMU
ENV QEMU_REL=esp-develop-20220919
ENV QEMU_SHA256=f6565d3f0d1e463a63a7f81aec94cce62df662bd42fc7606de4b4418ed55f870
ENV QEMU_DIST=qemu-${QEMU_REL}.tar.bz2
ENV QEMU_URL=https://github.com/espressif/qemu/releases/download/${QEMU_REL}/${QEMU_DIST}

ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

RUN wget --no-verbose ${QEMU_URL} \
  && echo "${QEMU_SHA256} *${QEMU_DIST}" | sha256sum --check --strict - \
  && tar -xf $QEMU_DIST -C /opt \
  && rm ${QEMU_DIST}

ENV PATH=/opt/qemu/bin:${PATH}

ENV ADF_REL=2.4.1
ENV ADF_SHA256=51528b3c60991184da4455d57f8f372ed2c57bf4d0ae981ac5b94eef0a3d2457
ENV ADF_DIST=v${ADF_REL}.tar.gz
ENV ADF_URL=https://github.com/espressif/esp-adf/archive/refs/tags/${ADF_DIST}

RUN wget --no-verbose ${ADF_URL} \
  && echo "${ADF_SHA256} ${ADF_DIST}" | sha256sum --check --strict - \
  && tar -xf $ADF_DIST -C /opt

ENV ADF_PATH=/opt/esp-adf-$ADF_REL

RUN echo "source /opt/esp/idf/export.sh > /dev/null 2>&1" >> ~/.bashrc

ENTRYPOINT [ "/opt/esp/entrypoint.sh" ]

CMD ["/bin/bash"]