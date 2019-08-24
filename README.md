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
