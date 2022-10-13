# CEA 2022 - Yugabyte on Kubernetes

## Pre-req

1. Kubernetes cluster (minikube should be okay)
2. [k9s](https://k9scli.io/)
3. [kubectl](https://kubernetes.io/docs/tasks/tools/)
4. [docker cli](https://docs.docker.com/get-docker/)

## Run a Kubernetes Cluster on  Minikube

1. Create minikube cluster

    ```bash
    minikube start --memory=8192 --cpus=2 --disk-size=40g --driver=docker  -n=4
    ```

    - `-n=3` - Runs 3 node cluster so that we can deploy multi node YugabyteDB cluster

## Deploy YugabyteDB

1. Setup Helm

    ```bash
    helm repo add yugabytedb https://charts.yugabyte.com
    helm repo update
    ```

2. Search for latest version

  ```bash
  helm search repo yugabytedb/yugabyte -l
  ```

3. Create a namespace


    ```bash
    kubectl create namespace yb-demo
    ```

4. Create 3 node cluster

    ```bash
      helm install yb-demo yugabytedb/yugabyte \
        --version 2.14.3 \
        --set resource.master.requests.cpu=0.5 \
        --set resource.master.requests.memory=0.5Gi \
        --set resource.tserver.requests.cpu=0.5 \
        --set resource.tserver.requests.memory=0.5Gi \
        --set replicas.master=3 \
        --set replicas.tserver=3 \
        --namespace yb-demo
    ```
5. Get UI address

    ```bash
     kubectl  -n yb-demo get svc
     ```

6. Run SQL Shell

    ```bash
    kubectl exec -it  -n yb-demo yb-tserver-0 -c yb-tserver -- ysqlsh -h yb-tserver-0
    ```

7. Let's play [SQL](https://docs.yugabyte.com/preview/quick-start/explore/ysql/) -- Choose **Kubernetes**

    To exit type `exit` or `\q` and `enter/return` OR `Ctrl+D`

8. Run Sample SQL

    ```bash
     kubectl run yb-sample-apps \
      -n yb-demo \
      --rm \
      -it \
      --restart=Never \
      --image=yogendra/yb-sample-apps \
      -- \
      --workload SqlInserts \
      --nodes yb-tserver-0.yb-tservers:5433,yb-tserver-1.yb-tservers:5433,yb-tserver-0.yb-tservers:5433
    ```

9.  Lets cause an node outage

    ```bash
    kubectl scale statefulset -n yb-demo yb-tserver --replicas 2
    ```

    (Observe app)

10. Lets restore the cluter

    ```bash
    kubectl scale statefulset -n yb-demo yb-tserver --replicas 3
    ```

    (Observe app)

11. Let upgrade our cluster

    ```bash

    ```
