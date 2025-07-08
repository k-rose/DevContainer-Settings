# ベースイメージとして OpenJDK 21 を使用
FROM mcr.microsoft.com/vscode/devcontainers/java:21

# 作業ディレクトリを設定
WORKDIR /workspace

# Maven のインストール
ARG MAVEN_VERSION=3.9.6
RUN wget https://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
    && tar -xzf apache-maven-${MAVEN_VERSION}-bin.tar.gz -C /usr/local \
    && ln -s /usr/local/apache-maven-${MAVEN_VERSION}/bin/mvn /usr/local/bin/mvn \
    && rm apache-maven-${MAVEN_VERSION}-bin.tar.gz

# google-java-format のインストール
ARG GOOGLE_JAVA_FORMAT_VERSION=1.23.0
RUN wget https://github.com/google/google-java-format/releases/download/v${GOOGLE_JAVA_FORMAT_VERSION}/google-java-format-${GOOGLE_JAVA_FORMAT_VERSION}-all-deps.jar -O /usr/local/google-java-format.jar \
    && chmod +x /usr/local/google-java-format.jar \
    && ln -s /usr/local/google-java-format.jar /usr/local/bin/google-java-format

# ユーザー設定（VS Code の推奨）
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
# グループが既に存在するかチェックし、なければ作成
RUN if ! getent group $USER_GID > /dev/null; then \
        groupadd --gid $USER_GID $USERNAME; \
    fi \
    # ユーザーが既に存在するかチェックし、なければ作成
    && if ! id -u $USER_UID > /dev/null 2>&1; then \
        useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    fi \
    # sudoers 設定を上書き（必要に応じて）
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
