# For build automation - Allows building from any ai-dock base image
# Use a *cuda*base* image as default because pytorch brings the libs
ARG IMAGE_BASE="ghcr.io/ai-dock/pytorch:2.0.1-py3.10-cuda-11.8.0-base-22.04"
FROM ${IMAGE_BASE}

LABEL org.opencontainers.image.source https://github.com/ai-dock/stable-diffusion-webui

LABEL org.opencontainers.image.description "Automatic1111 Stable Diffusion Web UI"

LABEL maintainer="Rob Ballantyne <rob@dynamedia.uk>"

ENV IMAGE_SLUG="stable-diffusion-webui"
# Copy early so we can use scripts in the build - Changes to these files will invalidate the cache and cause a rebuild.
COPY ./COPY_ROOT/ /

# Use build scripts to ensure we can build all targets from one Dockerfile in a single layer.
# Don't put anything heavy in here - We can use multi-stage building above if necessary.

ARG IMAGE_BASE
RUN /opt/ai-dock/bin/build/layer0/init.sh | tee /var/log/build.log

ENV OPT_SYNC=stable-diffusion-webui:$OPT_SYNC
ENV MAMBA_DEFAULT_ENV=webui
ENV MAMBA_DEFAULT_RUN="micromamba run -n ${MAMBA_DEFAULT_ENV}"

# Set false when building specific versions
ARG AUTO_UPDATE=true
ENV AUTO_UPDATE=$AUTO_UPDATE
# Keep init.sh as-is and place additional logic in /opt/ai-dock/bin/preflight.sh
CMD ["init.sh"]
