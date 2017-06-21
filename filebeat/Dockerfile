FROM debian:jessie

RUN apt-get -y update
RUN apt-get -y install apt-transport-https
RUN apt-get -y install curl

RUN curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.4.2-amd64.deb
RUN dpkg -i filebeat-5.4.2-amd64.deb

CMD filebeat.sh -e -c /usr/share/filebeat/config/filebeat.yml -d "publish"
