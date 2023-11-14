FROM ghcr.io/oracle/oraclelinux:8-slim

ARG user=webhook

RUN microdnf -y update && \
    microdnf -y clean all

RUN echo "$user:x:1000:1000::/home/$user:/bin/bash" >> /etc/passwd && \
    echo "$user:x:1000:" >> /etc/group && \
    mkdir /home/$user && \
    chown -R $user:$user /home/$user

COPY bin/webhook /usr/bin/
COPY LICENSE README.md THIRD_PARTY_LICENSES.txt SECURITY.md /licenses/

USER $user

CMD ["webhook"]
