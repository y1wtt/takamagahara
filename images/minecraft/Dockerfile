ARG VERSION="1.21.11-69"

FROM  eclipse-temurin:21.0.5_11-jdk-noble as builder
ARG VERSION
ENV VERSION=$VERSION
WORKDIR /app

RUN apt-get update && apt-get install -y git

ADD https://fill-data.papermc.io/v1/objects/cf374f2af9d71dfcc75343f37b722a7abcb091c574131b95e3b13c6fc2cb8fae/paper-${VERSION}.jar paper.jar

RUN mkdir data

FROM gcr.io/distroless/java21-debian12

ARG VERSION
COPY --from=builder /app/paper.jar /app/paper.jar

WORKDIR /app/data/
ENV JAVA_TOOL_OPTIONS="-Xms24576M -Xmx24576M -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+ParallelRefProcEnabled -XX:+PerfDisableSharedMem -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1HeapRegionSize=8M -XX:G1HeapWastePercent=5 -XX:G1MaxNewSizePercent=40 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1NewSizePercent=30 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:G1ReservePercent=20 -XX:InitiatingHeapOccupancyPercent=15 -XX:MaxGCPauseMillis=200 -XX:MaxTenuringThreshold=1 -XX:SurvivorRatio=32"
ENTRYPOINT ["java", "-jar", "../paper.jar", "nogui"]
