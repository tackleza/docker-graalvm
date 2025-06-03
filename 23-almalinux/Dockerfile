# Use the official Almalinux base image
FROM almalinux:9.6

# Set environment variables for GraalVM
ENV GRAALVM_TAR_URL https://download.oracle.com/graalvm/23/latest/graalvm-jdk-23_linux-x64_bin.tar.gz
ENV GRAALVM_DIR /usr/lib/graalvm

RUN mkdir $GRAALVM_DIR && curl -L $GRAALVM_TAR_URL | tar -xz --strip-components=1 -C $GRAALVM_DIR

# Set environment variables for GraalVM
ENV PATH="$PATH:$GRAALVM_DIR/bin"
ENV JAVA_HOME=$GRAALVM_DIR

# Verify installation
RUN java -version

# Default command to run when the container starts
CMD ["java", "-version"]
