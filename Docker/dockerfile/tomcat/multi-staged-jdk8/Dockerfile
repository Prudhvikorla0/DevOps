FROM amazonlinux:2 AS build
RUN yum install wget -y && \
    yum install gzip -y && \
    yum install tar -y && \
    yum install git -y
RUN yum install java-1.8.0-openjdk-devel -y

RUN wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz && \
    tar zxf apache-maven-3.9.6-bin.tar.gz && \
    mv apache-maven-3.9.6 /opt/maven

RUN git clone https://dptrealtime@bitbucket.org/dptrealtime/java-login-app.git /opt/app
WORKDIR /opt/app
RUN /opt/maven/bin/mvn package

FROM amazonlinux:2
RUN yum install wget -y && \
    yum install gzip -y && \
    yum install tar -y
RUN yum install java-1.8.0-openjdk-devel -y
RUN wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.100/bin/apache-tomcat-8.5.100.tar.gz && \
    tar zxf apache-tomcat-8.5.100.tar.gz && \
    mv apache-tomcat-8.5.100 /opt/tomcat
COPY --from=build /opt/app/target/dptweb-1.0.war /opt/tomcat/webapps/
CMD  ["/opt/tomcat/bin/catalina.sh", "run"]