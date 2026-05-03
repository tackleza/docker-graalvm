# Use the official Almalinux base image
FROM almalinux:9.6

# Set environment variables for GraalVM
ENV GRAALVM_TAR_URL https://download.oracle.com/graalvm/25/latest/graalvm-jdk-25_linux-x64_bin.tar.gz
ENV GRAALVM_DIR /usr/lib/graalvm

RUN mkdir $GRAALVM_DIR && curl -L --retry 5 --retry-delay 5 --retry-connrefused -f $GRAALVM_TAR_URL | tar -xz --strip-components=1 -C $GRAALVM_DIR

# Set environment variables for GraalVM
ENV JAVA_HOME=$GRAALVM_DIR
ENV PATH=$GRAALVM_DIR/bin:$PATH

# Verify installation
RUN $GRAALVM_DIR/bin/java -version

# Run container by default
CMD ["bash"]
