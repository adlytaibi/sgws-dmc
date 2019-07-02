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

    1. Create private key, generate a certificate signing request

        ```
        openssl genrsa -out ndmc/sslkeys/host.key 2048
        ```

    2. Self-sign your own certificates: (modify `web` to match your server)

        ```
        openssl req -x509 -nodes -newkey rsa:4096 -keyout ndmc/sslkeys/host.key -out ndmc/sslkeys/host.pem -days 365 -subj "/C=CA/ST=Ontario/L=Toronto/O=Storage/OU=Team/CN=web"
        ```

    3. Or sign your SSL certificate with a CA:

        1. Create a Subject Alternate Name configuration file `san.cnf` in `ndmc/sslkeys/`

            ```
            [req]
            distinguished_name = req_distinguished_name
            req_extensions = v3_req
            prompt = no
            default_md = sha256
            [req_distinguished_name]
            C = CA
            ST = Ontario
            L = Toronto
            O = Storage
            OU = Storage
            CN = dmc
            [v3_req]
            keyUsage = keyEncipherment, dataEncipherment
            extendedKeyUsage = serverAuth
            subjectAltName = @alt_names
            [alt_names]
            DNS.1 = dmc
            DNS.2 = dmc.acme.net
            IP.1 = 1.2.3.4
            ```

        2. Generate a certificate signing request

            ```
            cd ndmc/sslkeys/
            openssl req -new -sha256 -nodes -key host.key -out dmc.csr -config san.cnf
            ```

        3. In your CA portal use the `dmc.csr` output and the following SAN entry to sign the certificate, you should get a `certnew.pem` that can be saved as `host.pem`

            ```
            san:dns=dmc.acme.net&ipaddress=1.2.3.4
            ```

        4. Copy your `host.pem` certificate files to `ndmc/sslkeys`

4. (Optional) In case of an internal CA, you can add your root public certificate to `dmc/caroot/root.pem`

5. docker-compose

    ```
    docker-compose up -d
    ```

6. StorageGrid DMC service with SSL can be accessed using below URL:

    ```
    https://<IP_address>:8443
    ```
	(or if accessing from the same guest https://localhost:8443)

## Further reading
* [Docker Compose](https://docs.docker.com/compose/)
* [NGINX](https://www.nginx.com/)

## Notes
This is not an official NetApp repository. NetApp Inc. is not affiliated with the posted examples in any way.

```
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
