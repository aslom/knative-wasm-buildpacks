FROM rust:1.61 AS wagi
RUN git clone https://github.com/deislabs/wagi
RUN cd wagi && cargo build --release

FROM debian:bullseye-slim
#COPY sources.list /etc/apt/sources.list
COPY --from=builder /wagi/target/release/wagi /usr/local/bin/wagi

ENV CNB_USER_ID=1000
ENV CNB_GROUP_ID=1000
ENV CNB_STACK_ID="net.aslom.wasm.wagi"
LABEL io.buildpacks.stack.id="net.aslom.wasm.wagi"

COPY main.wasm /
COPY modules.toml /
RUN ["/usr/local/bin/wagi", "--help"]
CMD [ "wagi", "-l", "0.0.0.0:8080", "-c", "/modules.toml" ]

# 6. Set user and group (as declared in base image)
USER ${CNB_USER_ID}:${CNB_GROUP_ID}

# 1. Set a common base
FROM tinygo/tinygo:0.27.0 as base

# 2. Set required CNB information
ENV CNB_USER_ID=1000
ENV CNB_GROUP_ID=1000
ENV CNB_STACK_ID="net.aslom.wasm.wagi"
LABEL io.buildpacks.stack.id="net.aslom.wasm.wagi"

# # 8. Install packages that we want to make available at build time
# RUN apt-get update && \
#   apt-get install -y git wget jq && \
#   rm -rf /var/lib/apt/lists/* && \
#   wget https://github.com/tinygo-org/tinygo/releases/download/v0.27.0/tinygo_0.27.0_amd64.deb && \
#   sudo dpkg -i tinygo_0.27.0_amd64.deb

COPY --from=wagi /wagi/target/release/wagi /usr/local/bin/wagi

# ========== ADDED ===========
# 9. Set user and group (as declared in base image)
USER ${CNB_USER_ID}:${CNB_GROUP_ID}
CMD [ "wagi", "-l", "0.0.0.0:8080", "-c", "/modules.toml" ]
