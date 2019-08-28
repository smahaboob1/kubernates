Kubernetes Intallation :
------------------------
    Install Vagrant
    https://shaikperformancedevops.blogspot.com/2019/05/5-install-vagrant.html

    git pull https://github.com/smahaboob1/kubernates.git
    cd vagrent_provision
    vagrant up

Install Kubernetes Dashboard Web UI
------------------------------------
    kubectl cluster-info
    git clone https://github.com/justmeandopensource/kubernetes.git
    cd kubernetes/dashboard

    Install Influxdb
    -----------------
    kubectl -n kube-system get pods -o wide
    kubectl create -f influxdb.yaml
    kubectl -n kube-system get pods -o wide

    Install heapster
    -----------------
    kubectl -n kube-system get pods -o wide
    kubectl create -f heapster.yaml
    kubectl -n kube-system get pods -o wide

    Install Dashboard
    -----------------
    kubectl -n kube-system get pods -o wide
    kubectl create -f dashboard.yaml
    kubectl -n kube-system get pods -o wide
    kubectl cluster-info

    kubectl create -f sa_cluster_admin.yaml
    kubectl describe sa dashboard-admin -n kube-system
    Mountable secrets:   dashboard-admin-token-5bnv7
    kubectl describe secret dashboard-admin-token-5bnv7 -n kube-system

    https://kmaster:32323
    Enter key value

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
Service Types:
    ClusterIP
    NodePort
    LoadBalancer
    
https://github.com/smahaboob1/kubernates/blob/master/services_example.jpg

    Using NodePort, POD and Service:
    --------------------------------

    kubectl create -f rc_svc_example_pod.yaml
    kubectl create -f rc_svc_example_svc.yaml

    http://kmaster:31001/employee-management-0.0.1-SNAPSHOT/employees
    http://kmaster:31002/employee-management-0.0.1-SNAPSHOT/employees
    http://kmaster:31003/employee-management-0.0.1-SNAPSHOT/employees

    http://kmaster:31001/employee-management-0.0.1-SNAPSHOT/departments
    http://kmaster:31002/employee-management-0.0.1-SNAPSHOT/departments
    http://kmaster:31003/employee-management-0.0.1-SNAPSHOT/departments
    
    Using NodePort, POD, Service and Replication Controller:
    -------------------------------------------------------
    kubectl create -f rc_svc_example_pod.yaml
    kubectl create -f rc_svc_example_svc.yaml
    kubectl create -f rc_svc_example_rc.yaml

    http://kmaster:31001/employee-management-0.0.1-SNAPSHOT/employees
    http://kmaster:31002/employee-management-0.0.1-SNAPSHOT/employees
    http://kmaster:31003/employee-management-0.0.1-SNAPSHOT/employees

    http://kmaster:31001/employee-management-0.0.1-SNAPSHOT/departments
    http://kmaster:31002/employee-management-0.0.1-SNAPSHOT/departments
    http://kmaster:31003/employee-management-0.0.1-SNAPSHOT/departments

    Using NodePort, POD, Service,  Replication Controller and LoadBalencer:
    -----------------------------------------------------------------------
    kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.1/manifests/metallb.yaml
    kubectl get all -n metallb-system
    kubectl create -f loadbalancer.yaml                         --------> Change IP range according to the Kubernetes cluster IP range
    kubectl describe configmap config -n metallb-system
    
    kubectl create -f rc_svc_example_pod.yaml
    kubectl create -f rc_svc_example_svc.yaml
    kubectl create -f rc_svc_example_rc.yaml
    
    http://172.42.42.200/employee-management-0.0.1-SNAPSHOT/departments
    
Deployments Example:
---------------------
    create deployment
    update deployment
    perform rolling update
    perform rollback
    pause and resume a deployment

    kubectl apply -f deploy_app_pod_svc_v1.yaml
    kubectl apply -f deploy_db_pod_svc_v1.yaml
    http://kmaster:<port>/employee-management-0.0.1-SNAPSHOT/employees
    kubectl apply -f deploy_db_pod_svc_v2.yaml
    http://kmaster:<port>/employee-management-0.0.1-SNAPSHOT/employees
    kubectl apply -f deploy_db_pod_svc_v1.yaml
    http://kmaster:<port>/employee-management-0.0.1-SNAPSHOT/employees
    
Storage and Persistence
-----------------------
    kubectl create -f app_storage_pod_svc.yaml
    kubectl create -f db_storage_pod_svc.yaml
    kubectl describe pod tomcat-pod
       
    vim app_storage_pod_svc.yaml
    mountPath: /usr/local/tomcat/logs/              ---> Path in the tomcat container
    path: /home/vagrant/tomcat-storage/             ---> Host Path
    
    kubectl describe pod tomcat-pod | grep Node:
    ssh to node
    cd /home/vagrant/tomcat-storage/ 
    touch abc{1..3}.txt                             
    kubectl exec -ti tomcat-pod --container tomcat bash
    cd /usr/local/tomcat/logs/                      ---> abc{1..3}.txt and all tomcat logs should be accessible
    touch abc{4..6}.txt 
    exit from container
    cd /home/vagrant/tomcat-storage/                ---> abc{4..6}.txt and all tomcat logs should be accessible
    
    kubectl delete -f app_storage_pod_svc.yaml
    kubectl delete -f db_storage_pod_svc.yaml
    /home/vagrant/tomcat-storage                    ---> Data (tomcat logs and abc...txt files should be available)
    
    
    
    
