FROM registry.drycc.cc/drycc/base:bullseye

ENV GOSU_VERSION="1.14" \
    MC_VERSION="2022.06.17.02.52.50"

COPY prebuildfs /

RUN install-packages libnss-wrapper psmisc \
  && install-stack gosu $GOSU_VERSION \
  && install-stack mc $MC_VERSION \
  && echo "export NSS_WRAPPER_LIB=/usr/lib/`echo $(uname -m)`-linux-gnu/libnss_wrapper.so" >> /opt/drycc/mc/profile.d/mc.sh \
  && . init-stack \
  && rm -rf \
    /usr/share/doc \
    /usr/share/man \
    /usr/share/info \
    /usr/share/locale \
    /var/lib/apt/lists/* \
    /var/log/* \
    /var/cache/debconf/* \
    /etc/systemd \
    /lib/lsb \
    /lib/udev \
    /usr/lib/`echo $(uname -m)`-linux-gnu/gconv/IBM* \
    /usr/lib/`echo $(uname -m)`-linux-gnu/gconv/EBC* \
  && mkdir -p /usr/share/man/man{1..8} \
  && chmod g+rwX /opt/drycc

COPY rootfs /
RUN /opt/drycc/scripts/minio-client/postunpack.sh

WORKDIR /opt/drycc/mc
USER 1001
ENTRYPOINT [ "init-stack", "/opt/drycc/scripts/minio-client/entrypoint.sh" ]
CMD [ "/opt/drycc/scripts/minio-client/run.sh" ]
