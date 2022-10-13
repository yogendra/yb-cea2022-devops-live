# Local Demos

## Docker based DB Setup

1. Create a cluster

    ```bash
    yb-docker-ctl create --rf 3 --tag 2.15.2.1-b1 \
      --tserver_flags="cql_nodelist_refresh_interval_secs=1" \
      --master_flags="tserver_unresponsive_timeout_ms=1000"
    ```

1. Run Sample App

      1. Workload List

          ```bash
          docker run --rm  -it \
            --name yb-sample-app-workload-list \
            --net yb-net \
            yogendra/yb-sample-apps \
            --help

          ```

      1. Run Sample App - SQL Insert

            ```bash
            docker run --rm  -it \
              --name yb-sample-app-sqlinsert \
              --net yb-net \
              yogendra/yb-sample-apps \
              --workload SqlInserts \
              --nodes yb-tserver-n1:5433,yb-tserver-n2:5433,yb-tserver-n3:5433

            ```

      1. Run Sample App - Cassandra

          ```bash
          docker run --rm  -it \
            --name yb-sample-app-sqlinsert \
            --net yb-net \
            yogendra/yb-sample-apps \
            --workload CassandraKeyValue \
            --nodes yb-tserver-n1:5433,yb-tserver-n2:5433,yb-tserver-n3:5433
          ```

      1. (Optional) Run Sample App - Custom Run from Shell

           ```bash
           docker run --rm  -it \
             --name sample-app-shell \
             --net yb-net \
             -e YB_TSERVERS=yb-tserver-n1:5433,yb-tserver-n2:5433,yb-tserver-n3:5433 \
             -e YB_MASTERS=yb-master-n1:7100,yb-master-n2:7100,yb-master-n3:7100 \
             --entrypoint sh \
             yogendra/yb-sample-apps
           ```

1. Cluster Status

    ```bash
    yb-docker-ctl status
    ```

1. Cluster UI

    ```bash

    ```

1. Add Node

      ```bash
      yb-docker-ctl add_node

      ```
      (Observe Application)

1.

## Kubernetes (Minikube) Deploy

1. Create minikube cluster

    ```bash
    minikube start --memory=8192 --cpus=2 --disk-size=40g --vm-driver=docker  -n=3
    ```


1. Deploy yugabyte


    1. Setup HELM
    ```bash
    helm repo add yugabytedb https://charts.yugabyte.com
    helm repo update
    ```

    1. Search for latest version

      ```bash
      helm search repo yugabytedb/yugabyte --version 2.15.2
      ```

    1. Create a namespace

        ```bash
        kubectl create namespace yb-demo
        ```

    1.  Create 3 node cluster

          ```bash
          helm install yb-demo yugabytedb/yugabyte \
              --version 2.15.2 \
              --set resource.master.requests.cpu=0.5,resource.master.requests.memory=0.5Gi,\
                resource.tserver.requests.cpu=0.5,resource.tserver.requests.memory=0.5Gi,\
                replicas.master=1,replicas.tserver=1 --namespace yb-demo
        ```

Run SQL Shell

```bash

```

Run Sample SQL
```bash

