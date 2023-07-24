# legacyMySQL5
chown -R 1000:1000 /srv/docker/mecdb/data/
docker run -d --name mysql5_mec -p 3306:3306 -v /srv/docker/mecdb/data/:/usr/local/mysql/data/ --restart=unless-stopped mysql_prod

#backup db
docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root mec > /backups/mec.sql

docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/all.sql

#crontab
5 4 * * 1 docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_mon.sql
5 4 * * 2 docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_tue.sql
5 4 * * 3 docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_wed.sql
5 4 * * 4 docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_thu.sql
5 4 * * 5 docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_fri.sql
5 4 * * 6 docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_sat.sql
5 4 * * 0 docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_sun.sql

@hourly docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_hourly.sql
@daily docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_daily.sql
@weekly docker exec mysql5_mec /usr/local/mysql/bin/mysqldump -u root --all-databases > /backups/alldb/all_weekly.sql

0 5 * * 1 tar -cvpzf /backups/mysqldatafolder.tgz /srv/docker/mecdb/
