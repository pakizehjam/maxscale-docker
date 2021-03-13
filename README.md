## Real World Project: Database Shard Github
The project is done on lubuntu-20.10-desktop

# Requirements:

VM with Linux os
Install clone
Install docker-compose
Install mariadb-client
# To Install Docker:
sudo apt update
sudo apt upgrade
sudo apt install docker.io
sudo systemctl enable --now docker
sudo systemctl status docker
# To Install Docker Compose:
sudo apt install docker-compose
# To Install Mariadb:
sudo apt install mariadb-client
# Clonning maxscale-docker repository
sudo apt install git
https://github.com/Zohan/maxscale-docker

# Next we go on the maxscale directory:
cd maxscale-docker/maxscale/

# Then we should runthe containers up or down:
sudo docker-compose up -d
sudo docker-compose down -v

# For the cheking the containers:
docker-compose ps -a

maxscale_master2_1    docker-entrypoint.sh mysql ...   Up      0.0.0.0:4003->3306/tcp                               
maxscale_master_1     docker-entrypoint.sh mysql ...   Up      0.0.0.0:4001->3306/tcp                               
maxscale_maxscale_1   /usr/bin/tini -- docker-en ...   Up      3306/tcp, 0.0.0.0:4000->4000/tcp,                    
                                                              		              0.0.0.0:8989->8989/tcp                               
phpmyadmin            /docker-entrypoint.sh apac ...   Up      0.0.0.0:8080->80/tcp                                 

# For cheking the servers:
sudo docker-compose exec maxscale maxctrl list servers

┌────────────────┬─────────┬──────┬─────────────┬─────────────────┬───────────┐
│ Server         │ Address │ Port │ Connections │ State           │ GTID      │
├────────────────┼─────────┼──────┼─────────────┼─────────────────┼───────────┤
│ zip_master_one │ master  │ 3306 │ 0           │ Master, Running │ 0-3000-32 │
├────────────────┼─────────┼──────┼─────────────┼─────────────────┼───────────┤
│ zip_master_two │ master2 │ 3306 │ 0           │ Running         │ 0-3000-31 │
└────────────────┴─────────┴──────┴─────────────┴─────────────────┴───────────┘
# Ther is connection to mariadb using username(maxuser) and password(maxpwd) with port 4000:
mariadb -umaxuser -pmaxpwd -h 127.0.0.1 -P 4000

hasan@Hasan:~/maxscale-docker/maxscale$ mariadb -umaxuser -pmaxpwd -h 127.0.0.1 -P 4000
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 1
Server version: 10.5.8-MariaDB-1:10.5.8+maria~focal-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| zipcodes_one       |
| zipcodes_two       |
+--------------------+
5 rows in set (0.001 sec)
MariaDB {(none)]> ctrl-c --exit!

# Then in the Pycharm we run the SQL queries on the pyton file:
pyton script for the last 10 rows of zipcodes_one:
""print('The last 10 rows of zipcodes_one are:')
cursor.execute("SELECT * FROM zipcodes_one.zipcodes_one LIMIT 9990,10;")
results = cursor.fetchall()
for result in results:
    print(result)""
Result for the last 10 rows of zipcodes_one:

SELECT * FROM zipcodes_one.zipcodes_one LIMIT 9990,10

(40843, 'STANDARD', 'HOLMES MILL', 'KY', 'PRIMARY', '36.86', '-83', 'NA-US-KY-HOLMES MILL', 'FALSE', '', '', '')
(41425, 'STANDARD', 'EZEL', 'KY', 'PRIMARY', '37.89', '-83.44', 'NA-US-KY-EZEL', 'FALSE', '390', '801', '10204009')
(40118, 'STANDARD', 'FAIRDALE', 'KY', 'PRIMARY', '38.11', '-85.75', 'NA-US-KY-FAIRDALE', 'FALSE', '4398', '7635', '122449930')
(40020, 'PO BOX', 'FAIRFIELD', 'KY', 'PRIMARY', '37.93', '-85.38', 'NA-US-KY-FAIRFIELD', 'FALSE', '', '', '')
(42221, 'PO BOX', 'FAIRVIEW', 'KY', 'PRIMARY', '36.84', '-87.31', 'NA-US-KY-FAIRVIEW', 'FALSE', '', '', '')
(41426, 'PO BOX', 'FALCON', 'KY', 'PRIMARY', '37.78', '-83', 'NA-US-KY-FALCON', 'FALSE', '', '', '')
(40932, 'PO BOX', 'FALL ROCK', 'KY', 'PRIMARY', '37.22', '-83.78', 'NA-US-KY-FALL ROCK', 'FALSE', '', '', '')
(40119, 'STANDARD', 'FALLS OF ROUGH', 'KY', 'PRIMARY', '37.6', '-86.55', 'NA-US-KY-FALLS OF ROUGH', 'FALSE', '760', '1468', '20771670')
(42039, 'STANDARD', 'FANCY FARM', 'KY', 'PRIMARY', '36.75', '-88.79', 'NA-US-KY-FANCY FARM', 'FALSE', '696', '1317', '20643485')
(40319, 'PO BOX', 'FARMERS', 'KY', 'PRIMARY', '38.14', '-83.54', 'NA-US-KY-FARMERS', 'FALSE', '', '', '')


pyton script for the first 10 rows of zipcodes_tow:

""print('The first 10 rows of zipcodes_two are:')
cursor.execute("SELECT * FROM zipcodes_two.zipcodes_two LIMIT 10")
results = cursor.fetchall()
for result in results:
    print(result)""

Result for the first 10 rows of zipcodes_tow:
(42040, 'STANDARD', 'FARMINGTON', 'KY', 'PRIMARY', '36.67', '-88.53', 'NA-US-KY-FARMINGTON', 'FALSE', '465', '896', '11562973')
(41524, 'STANDARD', 'FEDSCREEK', 'KY', 'PRIMARY', '37.4', '-82.24', 'NA-US-KY-FEDSCREEK', 'FALSE', '', '', '')
(42533, 'STANDARD', 'FERGUSON', 'KY', 'PRIMARY', '37.06', '-84.59', 'NA-US-KY-FERGUSON', 'FALSE', '429', '761', '9555412')
(40022, 'STANDARD', 'FINCHVILLE', 'KY', 'PRIMARY', '38.15', '-85.31', 'NA-US-KY-FINCHVILLE', 'FALSE', '437', '839', '19909942')
(40023, 'STANDARD', 'FISHERVILLE', 'KY', 'PRIMARY', '38.16', '-85.42', 'NA-US-KY-FISHERVILLE', 'FALSE', '1884', '3733', '113020684')
(41743, 'PO BOX', 'FISTY', 'KY', 'PRIMARY', '37.33', '-83.1', 'NA-US-KY-FISTY', 'FALSE', '', '', '')
(41219, 'STANDARD', 'FLATGAP', 'KY', 'PRIMARY', '37.93', '-82.88', 'NA-US-KY-FLATGAP', 'FALSE', '708', '1397', '20395667')
(40935, 'STANDARD', 'FLAT LICK', 'KY', 'PRIMARY', '36.82', '-83.76', 'NA-US-KY-FLAT LICK', 'FALSE', '752', '1477', '14267237')
(40997, 'STANDARD', 'WALKER', 'KY', 'PRIMARY', '36.88', '-83.71', 'NA-US-KY-WALKER', 'FALSE', '', '', '')
(41139, 'STANDARD', 'FLATWOODS', 'KY', 'PRIMARY', '38.51', '-82.72', 'NA-US-KY-FLATWOODS', 'FALSE', '3692', '6748', '121902277')

pyton script for the largest zipcode number in zipcodes_one:

""print('The largest zipcode number in zipcodes_one is:')
cursor = db.cursor()
cursor.execute("SELECT Zipcode FROM zipcodes_one.zipcodes_one ORDER BY Zipcode DESC LIMIT 1;")
results = cursor.fetchall()
for result in results:
    print(result)""

Result for the largest zipcode number in zipcodes_one is:
(47750,)

pyton script for the smallest zipcode number in zipcodes_two is:

""print('The smallest zipcode number in zipcodes_two is:')
cursor = db.cursor()
cursor.execute("SELECT Zipcode FROM zipcodes_two.zipcodes_two ORDER BY Zipcode ASC LIMIT 1")
results = cursor.fetchall()
for result in results:
    print(result)""

Result for smallest zipcode number in zipcodes_two is:

(38257,)




# Sources:
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-10
https://mariadb.com/kb/en/mariadb-maxscale-25-simple-sharding-with-two-servers/ 
https://docs.docker.com/engine/install/ubuntu/  
https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-20-04 
https://docs.github.com/en/github/writing-on-github/basic-writing-and-formatting-syntax
https://docs.docker.com/compose/install/
I worked with Luma, Saeid, Veldo, Igor, Abdi.
