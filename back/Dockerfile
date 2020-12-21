FROM maven:3-openjdk-8 AS back-end-build
WORKDIR /tmp/spring-petclinic-reactjs
COPY spring-petclinic-reactjs/pom.xml .
RUN mvn -B -f pom.xml -s /usr/share/maven/ref/settings-docker.xml dependency:resolve
COPY spring-petclinic-reactjs .
RUN mvn -B -s /usr/share/maven/ref/settings-docker.xml package -DskipTests

FROM java:8-jdk-alpine AS back-end
USER 1000
ARG APP_VALUE
ENV APP_ARG=$APP_VALUE
HEALTHCHECK --interval=10s --timeout=3s CMD wget -q -t1 -s\
 'http://localhost:8080/manage/health' || exit 1
WORKDIR /app
COPY --from=back-end-build /tmp/spring-petclinic-reactjs/target/petclinic.jar .
ENTRYPOINT ["java", "-jar", "/app/petclinic.jar"]
EXPOSE 8080/TCP
