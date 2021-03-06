FROM azul/zulu-openjdk:8u192

RUN apt-get update && apt-get -y upgrade && apt-get -y install bash curl unzip

# Create dirs
RUN mkdir -p /opt/corda/cordapps
RUN mkdir -p /opt/corda/persistence
RUN mkdir -p /opt/corda/certificates
RUN mkdir -p /opt/corda/drivers
RUN mkdir -p /opt/corda/logs
RUN mkdir -p /opt/corda/bin
RUN mkdir -p /opt/corda/additional-node-infos
RUN mkdir -p /etc/corda

# Create corda user
RUN addgroup corda && \
    useradd corda -g corda -m -d /opt/corda

WORKDIR /opt/corda

ENV CORDAPPS_FOLDER="/opt/corda/cordapps"
ENV PERSISTENCE_FOLDER="/opt/corda/persistence"
ENV CERTIFICATES_FOLDER="/opt/corda/certificates"
ENV DRIVERS_FOLDER="/opt/corda/drivers"
ENV CONFIG_FOLDER="/etc/corda"

ENV MY_P2P_PORT=10200
ENV MY_RPC_PORT=10201
ENV MY_RPC_ADMIN_PORT=10202

RUN chown -R corda:corda /opt/corda
RUN chown -R corda:corda /etc/corda

##CORDAPPS FOLDER
VOLUME ["/opt/corda/cordapps"]
##PERSISTENCE FOLDER
VOLUME ["/opt/corda/persistence"]
##CERTS FOLDER
VOLUME ["/opt/corda/certificates"]
##OPTIONAL JDBC DRIVERS FOLDER
VOLUME ["/opt/corda/drivers"]
##LOG FOLDER
VOLUME ["/opt/corda/logs"]
##ADDITIONAL NODE INFOS FOLDER
VOLUME ["/opt/corda/additional-node-infos"]
##CONFIG LOCATION
VOLUME ["/etc/corda"]


##CORDA JAR
ADD --chown=corda:corda corda.jar /opt/corda/bin/corda.jar
##CONFIG MANIPULATOR JAR
ADD --chown=corda:corda config-exporter.jar /opt/corda/config-exporter.jar
##CONFIG GENERATOR SHELL SCRIPT
ADD --chown=corda:corda generate-config.sh /opt/corda/bin/config-generator
##CORDA RUN SCRIPT
ADD --chown=corda:corda run-corda.sh /opt/corda/bin/run-corda
##BASE CONFIG FOR GENERATOR
ADD --chown=corda:corda starting-node.conf /opt/corda/starting-node.conf
##SET EXECUTABLE PERMISSIONS
RUN chmod +x /opt/corda/bin/config-generator
RUN chmod +x /opt/corda/bin/run-corda

ENV PATH=$PATH:/opt/corda/bin

EXPOSE $MY_P2P_PORT
EXPOSE $MY_RPC_PORT

USER "corda"
CMD ["run-corda"]
