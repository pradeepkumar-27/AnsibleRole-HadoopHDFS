HadoopHDFS
=========

Ansible role to configure Hadoop Distributed File System.

Requirements
------------

I have created this role to configure my HDFS servers on top of Amazom Web Services (AWS), hence I have created my custom Elastic Cloud Compute (EC2) Amazon Machine Image (AMI) on top of Amazon Linux 2 with JDK 8u171 and Hadoop 1.2.1 pre-installed. You can find my custom AMI ID "ami-01bb2347b233b5110" on AWS.

Role Variables
--------------

This has three variables namely "nn_dir" which depicts the NameNode directory on the master node, "dn_dir" which depicts the DataNode directory on the slave nodes and "hdfs_port" which represents the port number on which the cluster works

ansibele.cfg
------------

Ansible configuration file to run the role

    [defaults]
    interpreter_python=auto_silent
    inventory      = ./hosts
    roles_path    = ./yourRolesPath (i.e the path where you have downloaded this role)
    host_key_checking = False
    remote_user = ec2-user
    private_key_file = ./yourKey.pem
    
    [privilege_escalation]
    become=True
    become_method=sudo
    become_user=root
    become_ask_pass=False

hosts
------------

Ansible inventory file where you have to put the IP of the servers. Since I'm setting up the HDFS cluster with the intention to integrate it with Hadoop MapReduce cluster for data analysis on the stored BigData. Hence I'm configuring JobTracker and Client systems also.

    [NameNode]
    namenode
    
    [DataNodes]
    datanode1
    datanode2
    datanode3
    
    [JobTracker]
    jobtracker
    
    [Client]
    client
    
    [HDFS:children]
    NameNode
    DataNodes
    Client
    JobTracker

Example Playbook
----------------

    - hosts: HDFS
      roles:
         - role: HadoopHDFS
           vars:
             nn_dir: /nn
             dn_dir: /dn
             hdfs_port: 7001
