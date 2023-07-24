# legacyMySQL5
chown -R 1000:1000 /srv/docker/mecdb/data/
docker run -d --name mysql5_mec -p 3306:3306 -v /srv/docker/mecdb/data/:/usr/local/mysql/data/ --restart=unless-stopped mysql_prod
