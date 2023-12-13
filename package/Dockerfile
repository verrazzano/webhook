FROM ghcr.io/verrazzano/golang:v1.20.12 as build
ARG ARCH
ENV ARCH=$ARCH

COPY . /go/src/github.com/rancher/webhook/

RUN dnf config-manager --enable ol8_appstream && \
    dnf install -y git && \
    cd /go/src/github.com/rancher/webhook/scripts && \
    ./build && \
    ./test

FROM ghcr.io/oracle/oraclelinux:8-slim

ARG user=webhook

RUN microdnf -y update && \
    microdnf -y clean all

RUN echo "$user:x:1000:1000::/home/$user:/bin/bash" >> /etc/passwd && \
    echo "$user:x:1000:" >> /etc/group && \
    mkdir /home/$user && \
    chown -R $user:$user /home/$user

COPY --from=build /go/src/github.com/rancher/webhook/bin/webhook /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

USER $user

CMD ["webhook"]
