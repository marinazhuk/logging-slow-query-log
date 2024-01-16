# Logging for Mysql Slow Query Log

* Set up MySQL with slow query log
* Configure ELK to work with mysql slow query log
* Configure GrayLog2 to work with mysql slow query log
* Set different thresholds for long_query_time ( 0, 1 , 10 ) and compare performance

## Slow query log file example [slow.log](mysql%2Flogs%2Fslow%2Fslow.log)

## Kibana logs example:
![kibana_dashboard.png](screenshots%2Fkibana_dashboard.png)
![kibana.png](screenshots%2Fkibana.png)

## Graylog logs example:
![graylog.png](screenshots%2Fgraylog.png)

For logs need to add Filebeat sidecar to Graylog (System > Inputs > Beats, port=5044), see https://go2docs.graylog.org/5-2/getting_in_log_data/graylog_sidecar.html

## Performance testing
Run Siege against simple Web application, which does 'SELECT SLEEP([sec])' query:  

```shell
siege -c1000 -t30s -d1 -f urls.txt
```

[urls.txt](urls.txt) with content:
```
http://localhost:8080/sleep?sec=0
http://localhost:8080/sleep?sec=1
```

### Results:

| long_query_time         | 10 sec          | 1 sec           | 0 sec           |
|-------------------------|-----------------|-----------------|-----------------|
| Transactions            | 1296 hits       | 1256 hits       | 1287 hits       |
| Availability            | 100.00 %        | 99.92 %         | 100.00 %        |
| Elapsed time            | 30.09 secs      | 30.04 secs      | 30.72 secs      |
| Data transferred        | 0.00 MB         | 0.00 MB         | 0.00 MB         |
| Response time           | 5.90 secs       | 7.09 secs       | 6.92 secs       |
| Transaction rate        | 43.07 trans/sec | 41.81 trans/sec | 41.89 trans/sec |
| Throughput              | 0.00 MB/sec     | 0.00 MB/sec     | 0.00 MB/sec     |
| Concurrency             | 254.15          | 296.59          | 289.89          |
| Successful transactions | 1296            | 1256            | 1287            |
| Failed transactions     | 0               | 1               | 0               |
| Longest transaction     | 28.46           | 29.59           | 29.19           |
| Shortest transaction    | 0.01            | 0.36            | 0.02            |