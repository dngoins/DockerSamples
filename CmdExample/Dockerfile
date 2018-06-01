FROM debian:jessie-slim

RUN apt-get update          && \
    apt-get install -y --no-install-recommends cowsay screenfetch curl && \
    rm -rf /var/lib/apt/lists/*

#HEALTHCHECK --interval=3s CMD curl --fail -m 2 http://localhost:90/ || exit 1

ENV PATH "$PATH:/usr/games"

CMD ["-f", "tux", "To improve is to change; to be perfect is to change often"]
ENTRYPOINT [ "cowsay" ]
