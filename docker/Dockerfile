ARG BASE_IMAGE
FROM ${BASE_IMAGE}
ENV HOME=/home/omar
COPY *.jar $HOME/
EXPOSE 8888
WORKDIR $HOME
USER root
RUN chmod 755 /home/omar && chmod 644 /home/omar/*.jar
USER 1001
CMD $HOME/run.sh
