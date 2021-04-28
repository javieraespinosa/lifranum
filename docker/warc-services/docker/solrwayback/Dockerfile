
FROM tomcat:9-jre11-slim

RUN apt update && apt install -y wget

RUN wget https://github.com/netarchivesuite/solrwayback/releases/download/4.0.6/solrwayback_package_4.0.6.zip 

RUN unzip solrwayback_package_4.0.6.zip \
 && rm -r webapps \
 && mv solrwayback_package_4.0.6/apache-tomcat-8.5.60/webapps  . \
 && rm -r solrwayback_package_4*

