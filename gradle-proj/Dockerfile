FROM centos:latest

USER root

ENV GRADLE_VERSION=8.4
ENV GRADLE_USER_HOME=/tmp

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

RUN touch /etc/profile.d/gradle.sh
RUN echo "export GRADLE_HOME=/gradle/gradle-8.4" >> /etc/profile.d/gradle.sh
RUN echo "export PATH=/gradle/gradle-8.4/bin:${PATH}" >> /etc/profile.d/gradle.sh
RUN chmod +x /etc/profile.d/gradle.sh
RUN source /etc/profile.d/gradle.sh

RUN mkdir /my-app
WORKDIR /my-app
COPY . .

RUN /gradle/gradle-8.4/bin/gradle -v
RUN /gradle/gradle-8.4/bin/gradle clean build install
CMD ["/gradle/gradle-8.4/bin/gradle", "run"]

EXPOSE 8080

