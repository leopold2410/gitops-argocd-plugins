ARG DEBIAN_RELEASE=bookworm
FROM debian:${DEBIAN_RELEASE}-slim AS builder

ARG ARGOCD_VAULT_PLUGIN_VERSION=1.17.0

RUN mkdir -p /downloads
WORKDIR /downloads

RUN apt-get update
RUN apt-get install -y wget

RUN wget -O argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_amd64 https://github.com/argoproj-labs/argocd-vault-plugin/releases/download/v${ARGOCD_VAULT_PLUGIN_VERSION}/argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_amd64
RUN wget -O avp-checksum.txt https://github.com/argoproj-labs/argocd-vault-plugin/releases/download/v${ARGOCD_VAULT_PLUGIN_VERSION}/argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_checksums.txt
RUN sha256sum -c --ignore-missing avp-checksum.txt

FROM debian:${DEBIAN_RELEASE}-slim

RUN mkdir -p /plugins
COPY --from=builder /downloads/argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_amd64 /plugins/argocd-vault-plugin
RUN chmod a+x /plugins/*

CMD [“sh”, “-c”, “cp -rv /plugins/* /custom-tools/”]