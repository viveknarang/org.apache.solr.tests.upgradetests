# Solr Upgrade Tests
Introduction
------------

The purpose of this program is to test rolling upgrades for Solr Cloud.

Current Test Results
--------------------

| Upgraded version >> | 5.3.0 | 5.3.1 | 5.3.2 | 5.4.0 | 5.4.1 |
|---------------------|-------|-------|-------|-------|-------|
| 5.2.1               | PASS  | PASS  |       |       |       |
| 5.3.0               |       | PASS  |       | FAIL  | FAIL  |
| 5.3.1               |       |       | PASS  | FAIL  | FAIL  |
| 5.3.2               |       |       |       | FAIL  | FAIL  |
| 5.4.0               |       |       |       |       | PASS  |

To Run
------
    
Use the following command to run this program on server

     mvn clean compile assembly:single
     java -jar target/org.apache.solr.tests.upgradetests-0.0.1-SNAPSHOT-jar-with-dependencies.jar -v1 5.3.0 -v2 5.4.1 -Nodes 3 -Shards 2 -Replicas 3


Program parameters
------------------

      -v1           {Version One}                   Example 5.4.0
      -v2           {Version Two}                   Example 5.4.1
      -Nodes        {Number of nodes}               Example 3
      -Shards       {Number of shards}              Example 2
      -Replicas     {Number of Replicas}            Example 3

Steps
-----

Following is the summary of the steps that the program follows to test the rolling upgrade
    
1. Start ZK
2. Start Solr nodes 
3. Create a Test collection
4. Insert a set of 1000 documents
5. Stop each node one by one, upgrade each node by replacing lib folder on server and then start each node
6. Check if the /live_nodes section list the upgraded node
7. Check if all the documents are available from the collection
8. If 5 and 6 passes, the program identifies the test as successful
9. Upon failure of either 5 or 6, the program declares the test as failed
10. If test passed, shutdown the nodes and cleaning zookeeper data


Todo
----

1. Running against unreleased (master/branch_5x)
2. Making it easier to add more tests to this framework
3. Code cleanup.
