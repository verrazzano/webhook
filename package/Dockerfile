# Use distroless as minimal base image to package the manager binary
FROM ghcr.io/verrazzano/ol8-static:v0.0.1-20231102152128-e7afc807

ARG user=webhook

RUN echo "$user:x:1000:1000::/home/$user:/bin/bash" >> /etc/passwd && \
    echo "$user:x:1000:" >> /etc/group && \
    mkdir /home/$user && \
    chown -R $user:$user /home/$user

COPY bin/webhook /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

USER $user

CMD ["webhook"]
