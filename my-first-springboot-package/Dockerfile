FROM localhost:5000/openjdk:11
ENV TZ Asia/Calcutta
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} my-first-springboot-app.jar
COPY target/classes/application.properties application.properties
#OPY target/classes/log4j2.xml log4j2.xml
#COPY target/classes/unlimited.hitachi.jks unlimited.hitachi.jks
ENTRYPOINT ["java","-server","-Xmx1024m","-XX:+HeapDumpOnOutOfMemoryError","-XX:+UseG1GC","-XX:+UnlockExperimentalVMOptions","-XX:+AggressiveOpts","-XX:+DoEscapeAnalysis","-XX:+UseCompressedOops","-XX:MaxGCPauseMillis=400","-XX:GCPauseIntervalMillis=8000","-jar","/my-first-springboot-app.jar”]