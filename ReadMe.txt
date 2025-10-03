## **Assumptions & Prerequisites**

1. **Hadoop Cluster in Docker**

   * You must have a running Hadoop cluster via Docker, including the containers:
     `namenode`, `resourcemanager`, `datanode`, `nodemanager`, `historyserver`.

2. **Input Files**

   * The dataset files (`q1_dataset.txt` and `q2_dataset.txt`) are **not included in HDFS by default**. Users must upload them to HDFS themselves.
   * For Q1: `/inputA` in HDFS

     ```bash
     docker cp ./q1_dataset.txt namenode:/tmp/q1_dataset.txt
     docker exec -it namenode hdfs dfs -mkdir -p /inputA
     docker exec -it namenode hdfs dfs -put /tmp/q1_dataset.txt /inputA/
     ```
   * For Q2: `/inputB` in HDFS

     ```bash
     docker cp ./q2_dataset.txt namenode:/tmp/q2_dataset.txt
     docker exec -it namenode hdfs dfs -mkdir -p /inputB
     docker exec -it namenode hdfs dfs -put /tmp/q2_dataset.txt /inputB/
     ```

3. **JAR Files**

   * The JAR files (`Q1Analysis.jar` and `Q2Analysis.jar`) must be copied into the `resourcemanager` container:

     ```bash
     docker cp ./Q1Analysis.jar resourcemanager:/tmp/Q1Analysis.jar
     docker cp ./Q2Analysis.jar resourcemanager:/tmp/Q2Analysis.jar
     ```

4. **HDFS Output Directories**

   * If an output directory already exists, Hadoop will throw a `FileAlreadyExistsException`. Remove existing output directories before rerunning a job:

     ```bash
     docker exec -it namenode hdfs dfs -rm -r /q1_output_A
     docker exec -it namenode hdfs dfs -rm -r /q2_output_A
     ```

5. **Host Machine**

   * The commands assume the host machine is running Docker with proper access to the Hadoop containers.
   * Output files will be copied from HDFS to the host using `/tmp/` in the container as an intermediate step.

---

## **Step 1: Copy JAR Files to the Resourcemanager**

```bash
docker cp ./Q1Analysis.jar resourcemanager:/tmp/Q1Analysis.jar
docker cp ./Q2Analysis.jar resourcemanager:/tmp/Q2Analysis.jar
```

---

## **Step 2: Run Q1 Analysis**

```bash
docker exec -it resourcemanager hadoop jar /tmp/Q1Analysis.jar /inputA /q1_output_A A
docker exec -it resourcemanager hadoop jar /tmp/Q1Analysis.jar /inputA /q1_output_B B
docker exec -it resourcemanager hadoop jar /tmp/Q1Analysis.jar /inputA /q1_output_C C
```

---

## **Step 3: Run Q2 Analysis**

```bash
docker exec -it resourcemanager hadoop jar /tmp/Q2Analysis.jar /inputB /q2_output_A A
docker exec -it resourcemanager hadoop jar /tmp/Q2Analysis.jar /q2_output_A /q2_output_B B
```

---

## **Step 4: Retrieve Output Files to Host**

```bash
docker exec -it namenode hdfs dfs -get /q1_output_A/part-r-00000 /tmp/part-r-00000
docker cp namenode:/tmp/part-r-00000 ./q1_output_A

docker exec -it namenode hdfs dfs -get /q1_output_B/part-r-00000 /tmp/part-r-00000
docker cp namenode:/tmp/part-r-00000 ./q1_output_B

docker exec -it namenode hdfs dfs -get /q1_output_C/part-r-00000 /tmp/part-r-00000
docker cp namenode:/tmp/part-r-00000 ./q1_output_C

docker exec -it namenode hdfs dfs -get /q2_output_A/part-r-00000 /tmp/part-r-00000
docker cp namenode:/tmp/part-r-00000 ./q2_output_A

docker exec -it namenode hdfs dfs -get /q2_output_B/part-r-00000 /tmp/part-r-00000
docker cp namenode:/tmp/part-r-00000 ./q2_output_B
```

---

