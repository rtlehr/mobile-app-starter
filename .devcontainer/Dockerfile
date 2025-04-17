# .devcontainer/Dockerfile
FROM mcr.microsoft.com/devcontainers/javascript-node:16

# Install OpenJDK, Android SDK command‑line tools, unzip
RUN apt-get update && \
    apt-get install -y openjdk-11-jdk unzip wget && \
    mkdir -p /opt/android-sdk/cmdline-tools && cd /opt/android-sdk/cmdline-tools && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip -O tools.zip && \
    unzip tools.zip && rm tools.zip

# Accept licenses (you can expand this if needed)
RUN yes | /opt/android-sdk/cmdline-tools/tools/bin/sdkmanager --licenses

# Update PATH
ENV ANDROID_HOME=/opt/android-sdk
ENV PATH=$ANDROID_HOME/cmdline-tools/tools/bin:$ANDROID_HOME/platform-tools:$PATH
