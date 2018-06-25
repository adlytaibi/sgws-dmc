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
