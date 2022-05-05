# Matrix (Synapse) + Element + Traefik (reverse proxy)
## Customize configuration files
You should detect instances of ulysseus.eu and replace it with your own domain in docker-compose.yml and element-config.json
```bash
export $MYDOMAIN=<yourdomain>
sed 's|ulysseus.eu|$MYDOMAIN|g' docker-compose.yml element-config.json
```
## Generate synapse configuration files
```bash
docker run -it --rm \  
    -v "`pwd`/synapse:/data" \
    -e SYNAPSE_SERVER_NAME=matrix.$MYDOMAIN \
    -e SYNAPSE_REPORT_STATS=yes \
    matrixdotorg/synapse:latest generate
sudo mv synapse/homeserver.yaml homeserver.yaml.bak
cat synapse/homeserver-template.yaml | sed 's|ulysseus.eu|$MYDOMAIN|g' > synapse/homeserver.yaml
```
## Create admin
```bash
sudo docker exec -it matrix_synapse_1 bash
register_new_matrix_user -c /data/homeserver.yaml http://localhost:8008
# reply to interactive questions (admin => yes)
exit
```
## Test
Your matrix server should be available at matrix.$MYDOMAIN  
Your element UI should be available at chat.$MYDOMAIN  
Your Traefik UI should be available at localhost:8080
