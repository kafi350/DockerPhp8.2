# Docker with PHP 8.0 and later versions for MAC with M1/M2

This repository is consisting of a docker images of PHP version 8.2 (you can change it in the docker-compose file),
Mysql latest, Apache and phpmyAdmin


## Prerequisites:

Please install docker for mac, or docker desktop for mac with Apple Chip.
https://docs.docker.com/desktop/install/mac-install/

## Accessing https://

Please add SSL Certificate. 

If you want to run your project in https:// and you want to access htttps://localhost then please add your SSL certificates into the SSL directory inside of docker/apache/ folder.
Please rename your Certificate files to -> mycert.key and mycert.csr
I mentioned the file names in the docker-compose.yml and Dockerfile of apache. However, you can also modify the files.

## How to use:

- Clone the repository
- Enter the repository folder
- Run the `docker-compose up` command

It will build the docker images and run the container.
Now if you want to put your php files, it will be in the html folder. html folder is the root directory and is set in the apache environment as root directory.

For phpmyAdmin, you can use the localhost:8080

## Inserting Database from a sql file
Just go to docker/mysql/dbdata and put you sql file there.

You can acess this sql file now in the terminal of your container. Just navigate to var/lib/mysql/ and you can find your file.

