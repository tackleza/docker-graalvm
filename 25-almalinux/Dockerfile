FROM almalinux:9
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN dnf upgrade -y \
    && dnf install -y tzdata curl-minimal tar gzip \
    && dnf clean all
ENV TZ=UTC
ENV GRAALVM_DIR=/usr/lib/graalvm
ENV JAVA_HOME=/usr/lib/graalvm
ENV PATH=/usr/lib/graalvm/bin:$PATH
# This is Oracle GraalVM, not GraalVM Community Edition.
ARG GRAALVM_TAR_URL=https://download.oracle.com/graalvm/25/latest/graalvm-jdk-25_linux-x64_bin.tar.gz
RUN curl -fSL --retry 5 --retry-delay 5 "$GRAALVM_TAR_URL" -o /tmp/graalvm.tar.gz \
    && curl -fSL --retry 5 "$GRAALVM_TAR_URL.sha256" -o /tmp/graalvm.sha256 \
    && echo "$(cat /tmp/graalvm.sha256)  /tmp/graalvm.tar.gz" | sha256sum -c - \
    && mkdir -p "$GRAALVM_DIR" \
    && tar -xzf /tmp/graalvm.tar.gz --strip-components=1 -C "$GRAALVM_DIR" \
    && rm /tmp/graalvm.tar.gz /tmp/graalvm.sha256
COPY VerifyRuntime.java /tmp/VerifyRuntime.java
RUN java /tmp/VerifyRuntime.java && rm /tmp/VerifyRuntime.java
CMD ["bash"]
