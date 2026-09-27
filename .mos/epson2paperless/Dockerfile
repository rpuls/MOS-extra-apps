# The upstream project publishes a multi-architecture image, so this package
# pins that image by its index digest rather than rebuilding the application.
# The digest is v0.12.0, which publishes linux/amd64 and linux/arm64 — the two
# architectures manifest.json declares.
FROM ghcr.io/mtheuma/epson2paperless@sha256:143c773f2b71f0bb27da12297982efbd10ae641923fd934bd24e5f813de47b91

# OUTPUT_DIR is a named volume's mount point. The upstream image runs as the
# unprivileged `node` user, which cannot create a directory at the filesystem
# root, so it is created here instead: Docker seeds a fresh named volume from
# the image, and the volume inherits this ownership with it.
USER root
RUN mkdir -p /output && chown node:node /output
USER node
