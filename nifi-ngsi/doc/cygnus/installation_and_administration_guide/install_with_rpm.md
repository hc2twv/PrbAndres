# Installing cygnus from package (Linux)
Simply configure the FIWARE Env variables:
      
      MIRROR=https://repositories.lab.fiware.org/dist \ 
      NIFI_VERSION=1.7.0 \
      NIFI_BASE_DIR=/opt/nifi \
      NIFI_HOME=${NIFI_BASE_DIR}/nifi-${NIFI_VERSION} \ 
      NIFI_BINARY_URL=/nifi/${NIFI_VERSION}/nifi-${NIFI_VERSION}-bin.tar.gz \ 
      NIFI_LOG_DIR=${NIFI_HOME}/logs 
      
Then download and decompress the package in the NIFI_HOME
```
curl -fSL ${MIRROR}/${NIFI_BINARY_URL} -o ${NIFI_BASE_DIR}/nifi-${NIFI_VERSION}-bin.tar.gz \ 
	echo "$(curl https://archive.apache.org/dist/${NIFI_BINARY_URL}.sha256) *${NIFI_BASE_DIR}/nifi-${NIFI_VERSION}-bin.tar.gz" | sha256sum -c - \ 
	tar -xvzf ${NIFI_BASE_DIR}/nifi-${NIFI_VERSION}-bin.tar.gz -C ${NIFI_BASE_DIR} \ 
	rm ${NIFI_BASE_DIR}/nifi-${NIFI_VERSION}-bin.tar.gz
```
To run NiFi in the background, use bin/nifi.sh start. This will initiate the application to begin running.  
    
    cd NIFI_HOME \
    ./bin/nifi.sh start
    
To check the status and see if NiFi is currently running, execute the command .
    
    ./bin/nifi.sh status
    
NiFi can be shutdown by executing the command.
    
    ./bin/nifi.sh stop 

With the service running, you can access to the NiFi GUI using this link (http://localhost:8080/nifi )

