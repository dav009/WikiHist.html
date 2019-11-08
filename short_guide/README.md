The long guide contain all the steps needed from downloading the files, until running the docker images, and it is useful for recreating the whole process.
Here, in the short guide, it is explained how to run the transformation from wikitext to HTML by downloading the docker image and the MySQL database.

1. Download the MySQL database and store it in folder called mysql_data

2. Pull the mysql and mediawiki images
sudo docker pull ic-registry.epfl.ch/mediawiki_docker/mysql-server:latest
sudo docker pull ic-registry.epfl.ch/mediawiki_docker/mediawiki_final:latest

3. Initialize and start up the mysql database
sudo docker network create my_network
- before running the following command, change the shell working directory to mysql_data
docker run --name=mysql1 --network my_network -v $(pwd)/my.cnf:/etc/my.cnf -v $(pwd)/mysql_datadir:/var/lib/mysql -d ic-registry.epfl.ch/mediawiki_docker/mysql-server

4. Download a XML sample file from here and store the file in directory called data, also inside the data directory create:
- a directory named results (data/results)
- a directory inside results named sample.xml (data/results/sample.xml)

5. Run the mediawiki image
docker run --name mediawiki --network my_network -v $(pwd)/data:/var/www/html/data -dit ic-registry.epfl.ch/mediawiki_docker/mediawiki_final

6. Run the main process
docker exec -it mediawiki /bin/bash
php main_parsing_process.php /var/www/html/data/sample.xml /var/www/html/data/results/sample.xml/ 1

7. When the processing is done:
- inside the results/sample.xml directory, there will be directory named _SUCCES
- inside the results directory, there will be file named log.txt with the following content - The job for file SUCCEEDED: sample.xml