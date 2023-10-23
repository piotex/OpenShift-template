FROM centos:latest

USER root

ENV GRADLE_VERSION=8.4
ENV GRADLE_HOME=/gradle/gradle-${GRADLE_VERSION}
ENV PATH=$PATH:$GRADLE_HOME/bin

RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
RUN yum update -y

RUN yum install -y unzip
RUN yum install -y wget

RUN wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.rpm
RUN yum -y install ./jdk-17_linux-x64_bin.rpm

RUN mkdir /gradle
WORKDIR /gradle
RUN wget -O ./gradle-${GRADLE_VERSION}-bin.zip https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip
RUN unzip gradle-${GRADLE_VERSION}-bin.zip -d ./

RUN chown 1001:0 $GRADLE_HOME && chmod -R g+rw $GRADLE_HOME
USER 1001

CMD ["gradle", "-v"]
EXPOSE 8080

