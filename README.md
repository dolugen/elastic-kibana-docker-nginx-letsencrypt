# Elastic - Kibana - Docker - Nginx - Letsencrypt
---
### Introduction 
Setup a Elastic + Kibana stack in seconds! Ready for public use with TLS enabled between nodes, and automatic SSL/TLS certificates + renewal with certbot and Nginx. 

Docker-compose follows Elastic's official documentation for creating a Elastic Stack on Docker. More information can be found on their official site.
https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html
https://www.elastic.co/guide/en/elasticsearch/reference/current/configuring-tls-docker.html

### Instructions

1. Create TLS certificates for encrypted communications between nodes
    `docker-compose -f create-certs.yml run --rm create_certs`

2. Edit nginx/config.conf and init-letsencrypt.sh and replace **DOMAINNAME.com** with your actual domain.

3. Execute the init-letsencrypt.sh script to generate LetsEncrypt certificates for nginx.
    ```
    chmod +x init-letencrypt.sh
    sudo ./init-letsencrypt.sh
    ```
4. Run the *elasticsearch-generate-passwords* tool on es01 to generate passwords for all built-in users and kibana_system. Make note of these passwords.
    ```
    docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \ 
    auto --batch --url https://es01:9200"
    ```
5. Rename .env.example to .env and edit it. Replace **ELASTIC_PASSWORD** with the randomly generated password for *kibana_system*. You'll also want to replace **KIBANA_ENCRYPTION_KEY** with a randomly generated, 32 character alphanumeric value. This is used for encrypting API keys for Elastic Agent fleets.

6. Restart your stack, and you should have a fully working elastic stack with HTTPS enabled!
    ```
    docker-compose stop
    docker-compose up -d
    ```