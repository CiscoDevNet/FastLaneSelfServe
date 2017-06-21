FROM debian:jessie

RUN apt-get -y update
RUN apt-get -y install apt-transport-https
RUN apt-get -y install curl

RUN curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.4.2-amd64.deb
RUN dpkg -i filebeat-5.4.2-amd64.deb

RUN DEBIAN_FRONTEND=noninteractive apt-get -y install tshark

#ADD config/fastlane_elastic.template.json /etc/filebeat/fastlane_elastic.template.json
#ADD config/filebeat.yml /etc/filebeat/filebeat.yml

RUN chmod 755 /etc/filebeat/filebeat.yml

RUN filebeat.sh -e -c /etc/filebeat/filebeat.yml -d "publish"
