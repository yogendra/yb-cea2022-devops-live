# Local Demos

## Docker based DB Setup

1. Create a cluster

    ```bash
    yb-docker-ctl create --rf 3 --tag 2.15.2.1-b1 \
      --tserver_flags="cql_nodelist_refresh_interval_secs=1" \
      --master_flags="tserver_unresponsive_timeout_ms=1000"
    ```

1. Cluster Status

    ```bash
    yb-docker-ctl status
    ```

1. Cluster UI [http://localhost:7000](http://localhost:7000)

1. Run SQL Shell

    ```bash
    docker exec -it yb-tserver-n1 ysqlsh -h yb-tserver-n1
    ```

1. Let's play [SQL](https://docs.yugabyte.com/preview/quick-start/explore/ysql/) -- Choose **Kubernetes**

    To exit type `exit` or `\q` and `enter/return` OR `Ctrl+D`

1. Run Sample App

      1. Workload List

          ```bash
          docker run --rm  -it \
            --name yb-sample-app-workload-list \
            --net yb-net \
            yogendra/yb-sample-apps \
            --help

          ```

      2. Run Sample App - SQL Insert

            ```bash
            docker run --rm  -it \
              --name yb-sample-app-sqlinsert \
              --net yb-net \
              yogendra/yb-sample-apps \
              --workload SqlInserts \
              --nodes yb-tserver-n1:5433,yb-tserver-n2:5433,yb-tserver-n3:5433

            ```

      3. Run Sample App - Cassandra

          ```bash
          docker run --rm  -it \
            --name yb-sample-app-sqlinsert \
            --net yb-net \
            yogendra/yb-sample-apps \
            --workload CassandraKeyValue \
            --nodes yb-tserver-n1:5433,yb-tserver-n2:5433,yb-tserver-n3:5433
          ```

      4. (Optional) Run Sample App - Custom Run from Shell

           ```bash
           docker run --rm  -it \
             --name sample-app-shell \
             --net yb-net \
             -e YB_TSERVERS=yb-tserver-n1:5433,yb-tserver-n2:5433,yb-tserver-n3:5433 \
             -e YB_MASTERS=yb-master-n1:7100,yb-master-n2:7100,yb-master-n3:7100 \
             --entrypoint sh \
             yogendra/yb-sample-apps
           ```

1. Remove a Node

    ```bash
    yb-docker-ctl stop_node 3
    ```

    (Observe Application)

1. Restore Node

    ```bash
    yb-docker-ctl start_node 3
    ```

    (Observe Application)

1. Add a Node

    ```bash
    yb-docker-ctl add_node
    ```

    (Admin UI)
