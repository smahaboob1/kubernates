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
