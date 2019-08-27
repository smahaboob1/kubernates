git clone https://github.com/smahaboob1/employee-management.git

mvn install

Create docker image :
----------------------
smahaboob/app

    FROM tomcat:8-jre11
    RUN rm -rf /usr/local/tomcat/webapps/*
    COPY target/employee-management-0.0.1-SNAPSHOT.war /usr/local/tomcat/webapps/employee-management-0.0.1-SNAPSHOT.war
    EXPOSE 8080
    CMD ["catalina.sh", "run"]
    
    docker build -t smahaboob/app:v1 .
    
smahaboob/db

    FROM mysql:5.7.25
    ENV MYSQL_ROOT_PASSWORD="mysql"
    ENV MYSQL_DATABASE="mysqldb"
    ENV MYSQL_USER="mysql"
    ENV MYSQL_PASSWORD="mysql"
    ADD employeemanagementsystemDB1.sql docker-entrypoint-initdb.d/employeemanagementsystemDB1.sql
    
    docker build -t smahaboob/db:v1 .
    docker build -t smahaboob/db:v2 .
    docker build -t smahaboob/db:v3 .
    
   https://cloud.docker.com/repository/docker/smahaboob/db/general      ------> Refer for more details
    
Setup using Docker link:
------------------------
    docker pull smahaboob/db
    docker pull smahaboob/app
    docker run --name db -e MYSQL_ROOT_PASSWORD=mysql -p 3306:3306 -d smahaboob/db
    docker run --name app --link db:mysql -d -p 8080:8080 smahaboob/app
    http://localhost:8080/employee-management-0.0.1-SNAPSHOT/departments

Create docker image from running container
-----------------------------------------

    docker pull smahaboob/db
    docker run -dti --name mysqldb smahaboob/db
    docker exec -it mysqldb /bin/bash 
    mysql -u mysql -p
    use mysqldb 
    insert new records
    INSERT INTO `department` VALUES (7,'V1','for Version 1 images');
    INSERT INTO `employee` VALUES (5,'V1Lname1','V1Fname1',5),(6,'V1Lname2','V1Fname2',6);
    docker commit mysqldb smahaboob/db:v1
    docker images smahaboob/db:v1
    docker login
    docker push smahaboob/db:v1


POD and Service Example
------------------------

    git clone github.com/smahaboob1/kubernates.git
    kubectl apply -f app_pod_svc.yaml
    kubectl apply -f db_pod_svc.yaml
    kubectl get all -o wide
    kmaster:30020/employee-management-0.0.1-SNAPSHOT/employees

Replicaion Controller Example
-----------------------------

    git clone github.com/smahaboob1/kubernates.git
    kubectl apply -f replication_controller.yaml
    kubectl apply -f replication_controller_svc.yaml
    kubectl get all -o wide
    kmaster:30020/employee-management-0.0.1-SNAPSHOT/employees
    
Replication Contoller and Service Example:
-----------------------------------------
https://github.com/smahaboob1/kubernates/blob/master/services_example.jpg


