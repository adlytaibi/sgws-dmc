FROM nginx:latest
COPY nginx.conf /etc/nginx/nginx.conf
COPY sslkeys/host.key /etc/nginx/certs/
COPY sslkeys/host.pem /etc/nginx/certs/
EXPOSE 8443
