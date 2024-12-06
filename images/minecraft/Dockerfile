ARG VERSION="1.21.4"

FROM  eclipse-temurin:21.0.5_11-jdk-noble as builder
ARG VERSION
ENV VERSION=$VERSION
WORKDIR /app

RUN apt-get update && apt-get install -y git

ADD https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar ./
RUN java -jar ./BuildTools.jar --rev ${VERSION}

RUN mv ./spigot-${VERSION}.jar spigot.jar
RUN echo "eula=true" > eula.txt

FROM gcr.io/distroless/java21-debian12

ARG VERSION
COPY --from=builder /app/ ./

ENTRYPOINT ["java","-jar", "spigot.jar"]