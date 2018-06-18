# SSL Termination for StorageGRID Data Management Console (DMC) with nginx

docker-compose will build two containers:

* StorageGRID Data Management Console (DMC) will be running in a container listening on port 8080 by default
* nginx listening on port 8443 with SSL (which can be modified as you wish)

## Pre-requisites

* git
* docker
* docker-compose

## Installation

1. Clone this:

    ```
    git clone https://github.com/adlytaibi/sgws-dmc
    ```

    ```
    cd sgws-dmc
    ```

2. Clone storagegrid-dmc into dmc directory

    ```
    git clone https://github.com/NetApp-StorageGRID/storagegrid-dmc.git dmc/storagegrid-dmc
    ```

3. SSL certificates

    ```
    mkdir ndmc/sslkeys
    ```

* Copy your host.pem and host.key certificate files to ndmc/sslkeys

* (Optionally) Self-sign your own certificates (modify `hostname` to match your server)

    ```
    openssl req -x509 -nodes -newkey rsa:4096 -keyout ndmc/sslkeys/host.key -out ndmc/sslkeys/host.pem -days 365 -subj "/C=CA/ST=Ontario/L=Toronto/O=Storage/OU=Team/CN=hostname"
    ```

4. docker-compose

    ```
    docker-compose up -d
    ```

5. StorageGrid DMC service with SSL can be accessed using below URL:

    ```
    https://<IP_address>:8443
    ```
	(or if accessing from the same guest https://localhost:8443)

