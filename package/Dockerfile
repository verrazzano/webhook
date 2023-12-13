FROM ghcr.io/verrazzano/golang:v1.20.12 as build
ARG ARCH
ENV ARCH=$ARCH

COPY . /go/src/github.com/rancher/webhook/

RUN dnf config-manager --enable ol8_appstream && \
    dnf install -y git && \
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
