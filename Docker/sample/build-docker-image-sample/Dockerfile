FROM --platform=linux/amd64 openjdk:17-jdk-alpine

# 设置工作目录
WORKDIR /app

# 将 jar 包复制到容器中
COPY build/libs/build-docker-image-sample-0.0.1-SNAPSHOT.jar /app/build-docker-image-sample-0.0.1-SNAPSHOT.jar

# 暴露应用程序将监听的端口 (根据实际情况调整)
EXPOSE 8080

# 运行 jar 包
ENTRYPOINT ["java", "-jar", "build-docker-image-sample-0.0.1-SNAPSHOT.jar"]