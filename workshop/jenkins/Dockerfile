FROM jenkins/jenkins

USER root

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    software-properties-common

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"

RUN apt-get update && apt-get install -y \
    docker-ce

# ENV MAVEN_HOME /opt/maven
ENV MAVEN_NAME apache-maven-3.3.9
ENV MAVEN_HOME /opt/maven/

# get maven 3.3.9
RUN wget --no-verbose -O /tmp/$MAVEN_NAME.tar.gz http://archive.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz \
    # install maven
    && mkdir /opt/maven \
    && tar -zxf /tmp/$MAVEN_NAME.tar.gz -C /opt/maven --strip-components=1 \
    && ln -s /opt/maven/bin/mvn /usr/local/bin \
    && rm -f /tmp/$MAVEN_NAME.tar.gz

# install inspec - chef
RUN apt-get -y install ruby ruby-dev gcc g++ make
RUN gem install inspec

RUN usermod -aG docker,staff jenkins

USER jenkins

# Install Jenkins plugins
RUN install-plugins.sh \
    blueocean \
    docker-workflow \
    locale \
    workflow-aggregator \
    pipeline-stage-view \
    git \
    cloudbees-bitbucket-branch-source \
    github-organization-folder
