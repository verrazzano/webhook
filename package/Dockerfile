FROM ghcr.io/oracle/oraclelinux:8-slim as build
ARG ARCH
ENV ARCH=$ARCH

COPY . /go/src/github.com/rancher/webhook/

RUN microdnf update -y --setopt=install_weak_deps=0 --setopt=tsflags=nodocs && \
    microdnf install -y golang-1.20.10-1.module+el8.9.0+90068+08775a1f.x86_64 && \
    microdnf install -y git && \
    cd /go/src/github.com/rancher/webhook/scripts && \
    ./build && \
    ./test

ARG user=webhook

RUN echo "$user:x:1000:1000::/home/$user:/bin/bash" >> /etc/passwd && \
    echo "$user:x:1000:" >> /etc/group && \
    mkdir /home/$user && \
    chown -R $user:$user /home/$user

FROM ghcr.io/verrazzano/ol8-static:v0.0.1-20231102152128-e7afc807

ARG user=webhook

COPY --from=build /etc/passwd /etc/passwd
COPY --from=build /etc/group /etc/group

COPY --from=build /go/src/github.com/rancher/webhook/bin/webhook /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

USER $user

CMD ["webhook"]
