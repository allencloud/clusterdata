
# 1 Introduction

> TODO

# 2 Data Files for The Trace

## 2.1 About

Cluster-trace-v2018 includes about 4000 machines in a perids of 8 days and is consisted of 6 tables (each is a file) of about 130 GB in tatal (after extraction). Here is a brief introduction of the tables and the detail of each table is provided in section *2.2 Schema*.

* machine_meta.csv：the meta info and event infor of machines, about 1MB
* machine_usage.csv: the resource usage of each machine, about 5.02GB
* container_meta.csv：the meta info and event infor of containers, about 2.49MB
* container_usage.csv：the resource usage of each container, about 87.09GB
* batch_instance.csv：inforamtion about instances in the batch workloads, about 37.67GB
* batch_task.csv：inforamtion about instances in the batch workloads, about 0.26GB. Note that the DAG information of each job's tasks is described in the task_name field.

### Downloading the data

> Last updated: 2018-11-05

The download link of the trace will be provided soon (we are running through some final verification ...).

## 2.2 Schema

* machine_meta.csv

| column name | type | explanation | comments |
| -------- | -------- | -------- | -------- |
| machine_id     | string  | The unique ID (uid) of a machine     |  |
| time_stamp     |  int  | time index  | 0 means the time stamp is before or after the 8-day time span |
| disaster_level_1     |  string  | 1st level of failure domain  |  we have multiple levels of failure domains of which two are provided in this version of trace |
| disaster_level_2     |  string  | 2nd level of failure domain    | we have multiple levels of failure domains of which two are provided in this version of trace  |
| cpu_num     |  int  | num of cpu of a machine     | unit is "core", e.g. 4 means 4 cores on the machine |
| mem_size     |  int  | memory size of a machine     |   |
| disk_size     |  int  | disk size of a machine     |   |
| status     |  string  | status of a machine at given time_stamp    | status of the machine, see the state machine |

* machine_usage.csv

| column name | type | explanation | comments |
| -------- | -------- | -------- | -------- |
| machine_id     | string  | The unique ID (uid) of a machine     |  |
| time_stamp     |  int  | time stamp  | 0 means the time stamp is before or after the 8-day time span |
| cpu_util_percent     |  int  | cpu utilization percentage  | both user and system are included, between [0, 100] |
| mem_util_percent     |  int  | memory utilization percentage    | both user and system are included, between [0, 100]  |
| mem_gps     |  int  | memory bandwidth in Gb  |  |
| cpu_mpki     |  int  | cache miss per thousand instruction |   |
| net_in     |  int  | number of incoming network packages  | noramlized to the maximum of this column |
| net_out     |  int  | number of outgoing network packages  | noramlized to the maximum of this column |
| disk_usage_percent     |  int  | disk space utilization percentage    | [0, 100]  |
| disk_io_percent     |  int  | disk io utilization percentage  | [0, 100] |

* container_meta.csv

| column name | type | explanation | comments |
| -------- | -------- | -------- | -------- |
| container_id     | string  | The unique ID (uid) of a container     |   |
| machine_id     | string  | machine uid of a container's host     |   |
| deploy_unit | string  | deploy group of a container  | containers belong to the same deploy unit provides one service, typically, they should be spread across failure domain |
| time_stamp     |  int  | time stamp  | 0 means the time stamp is before or after the 8-day time span |
| cpu_request     |  int  | planned cpushare request  | 100 means 1 core  |
| cpu_limit     |  int  | planned cpushare request    | 100 means 1 core  |
| mem_size     |  int  | planned memory size  |  |
| disk_size     |  int  | planned disk size |   |
| status     |  string  | status of a container at given time_stamp  | status of a container at given time stamp, see the state machine for details  |

* container_usage.csv

| column name | type | explanation | comments |
| -------- | -------- | -------- | -------- |
| container_id     | string  | The unique ID (uid) of a container     |   |
| machine_id     | string  | machine uid of a container's host     |   |
| time_stamp     |  int  | time stamp  | 0 means the time stamp is before or after the 8-day time span |
| cpu_util_percent     |  int  | cpu utilization percentage  | [0, 100]  |
| mpki     |  int  | cache miss per thousand instruction  |   |
| cpi     |  int  | cpi of a container at given time_stamp  |  |
| mem_util_percent     |  int  | memory utilization percentage | [0, 100]  |
| mem_gps  |  int  | memory bandwidth in Gb |   |
| disk_usage_percent     |  int  | disk space utilization percentage | [0, 100]  |
| disk_io_percent     |  int  | disk io utilization percentage | [0, 100]  |
| net_in     |  int  | number of incoming network packages  |  noramlized to the maximum of this column |
| net_out     |  int  | number of outgoing network packages  | noramlized to the maximum of this column |


* batch_instance.csv

| column name | type | explanation | comments |
| -------- | -------- | -------- | -------- |
| inst_name  | string  | The unique ID (uid) of a batch instance     | instance name is unique within a (job, task) pair |
| task_name  | string  | The task name to which an instance belongs    | task name is uniqe within a job; note task name indicates the DAG information, see the explanation of batch workloads |
| task_type  | string  | type of the task     |  |
| job_name  | string  | job_name of the task that an instance belongs to  | a job is consisted of many tasks. See the explanation of job-task-instance |
| status  | string  | status of an instance  | status of the instance  |
| start_time  | int  | start time of an instance  | 0 means the time stamp is before or after the 8-day time span  |
| end_time  | int  | end time of an instance  | 0 means the time stamp is before or after the 8-day time span  |
| machine_id  | string  | the machine id on which an instance runs  |  |
| seq_no  | int  | seq no of an instance     |   |
| total_seq_no  | int  | total seq no of an instance  |   |
| cpu_avg  | int  | average cpu utilization of cpu of an instance  | 100 means 1 core  |
| cpu_max  | int  | max cpu utilization of cpu of an instance  | 100 means 1 core  |
| mem_avg  | int  | average memory utilization of cpu of an instance  |   |
| mem_max  | int  | max memory utilization of cpu of an instance  |   |

* batch_task.csv

| column name | type | explanation | comments |
| -------- | -------- | -------- | -------- |
| task_name  | string  | The task name to which an instance belongs    | note task name indicates the DAG information, see the explanation of batch workloads  |
| inst_num  | int  | number of instances a task has   |  |
| task_type  | string  | type of the task     |   |
| job_name  | string  | job_name of a task  |   |
| status  | string  | status of an instance  | status of the task  |
| start_time  | int  | start time of an instance  | 0 means the time stamp is before or after the 8-day time span  |
| end_time  | int  | end time of an instance  | 0 means the time stamp is before or after the 8-day time span  |
| plan_cpu  | int  | plan cpu of a task | 100 means 1 core |
| plan_mem  | int  | plan memory of a task |  |

## 2.3 State machines

> TODO

## 2.4 Explaning batch workloads: job, task and instance

> TODO

# 3 Known Issues

> Empty for now.

# 4 Frequent Questions

> Empty for now.