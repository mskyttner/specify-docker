FROM maven:3-jdk-7

RUN printf "\ndeb http://httpredir.debian.org/debian jessie main contrib\n" >> /etc/apt/sources.list.d/contrib.list && \
	apt-get update && apt-get install -y --no-install-recommends \
	git ttf-mscorefonts-installer

COPY report-runner-service /usr/src/app

WORKDIR /usr/src/app

# add a font extension jar to src/main/webapp/WEB-INF/lib/
# COPY fonts.jar src/main/webapp/WEB-INF/lib/

RUN mkdir -p /root/.m2 && cp /usr/share/maven/ref/settings-docker.xml /root/.m2/settings.xml \
	&& git clone https://github.com/barbecue-sf/barbecue.git \
	&& cd barbecue/barbecue \
	&& mvn compile \
	&& mvn install \
#	&& mvn install:install-file -Dfile=target/barbecue-1.9-dev.jar -DpomFile=pom.xml \
#	&& mvn install:install-file -Dfile=target/barbecue-1.9-dev.jar -DgroupId=net.sourceforge.barbecue -DartifactId=barbecue -Dversion=1.9-dev -Dpackaging=jar \
	&& cd ../.. && mvn compile

#VOLUME /root/.m2

EXPOSE 8080

CMD ["mvn", "jetty:run", "-Djetty.host=0.0.0.0"]
