# Hadoop_MapReduce_Analysis

```
README - Hadoop MapReduce Assignment
-------------------------------------

Author: <FirstName> <LastName>
NetID: <YourNetID>
Course: <CourseName>
Assignment: Hadoop MapReduce Analysis

-------------------------------------
FILES INCLUDED
-------------------------------------
1. Q1Analysis.java        (Java source file for Question 1)
2. Q1Analysis.jar         (Compiled JAR file for Question 1)
3. Q2Analysis.java        (Java source file for Question 2)
4. Q2Analysis.jar         (Compiled JAR file for Question 2)
5. q1_output_A            (Output directory for Q1A: Word Count)
6. q1_output_B            (Output directory for Q1B: Target Words)
7. q1_output_C            (Output directory for Q1C: Unique Words by Pattern)
8. q2_output_A            (Output directory for Q2A: Inverted Index)
9. q2_output_B            (Output directory for Q2B: Maximum Occurrence Words)
10. README.txt            (This file)

-------------------------------------
COMPILATION INSTRUCTIONS
-------------------------------------
1. Copy the Java files into your Hadoop container or local machine with Hadoop configured.
2. Compile Q1Analysis.java:
   hadoop com.sun.tools.javac.Main Q1Analysis.java
   jar cf Q1Analysis.jar Q1Analysis*.class

3. Compile Q2Analysis.java:
   hadoop com.sun.tools.javac.Main Q2Analysis.java
   jar cf Q2Analysis.jar Q2Analysis*.class

4. Move the JAR files into the /tmp directory inside the container for execution.

-------------------------------------
EXECUTION INSTRUCTIONS
-------------------------------------
Input data:
- Place `q1_dataset.txt` into HDFS under /inputA
- Place `q2_dataset.txt` into HDFS under /inputB

Example (inside Docker):
   hdfs dfs -mkdir /inputA
   hdfs dfs -put q1_dataset.txt /inputA
   hdfs dfs -mkdir /inputB
   hdfs dfs -put q2_dataset.txt /inputB

-------------------------------------
QUESTION 1 (Q1Analysis.jar)
-------------------------------------

A. Word Count
docker exec -it resourcemanager hadoop jar /tmp/Q1Analysis.jar /inputA /q1_output_A A

B. Target Words (whale, sea, ship)
docker exec -it resourcemanager hadoop jar /tmp/Q1Analysis.jar /inputA /q1_output_B B

C. Unique Words by (length, first character)
docker exec -it resourcemanager hadoop jar /tmp/Q1Analysis.jar /inputA /q1_output_C C

-------------------------------------
QUESTION 2 (Q2Analysis.jar)
-------------------------------------

A. Inverted Index
docker exec -it resourcemanager hadoop jar /tmp/Q2Analysis.jar /inputB /q2_output_A A

B. Maximum Occurrence Words
(uses output of Q2A as input)
docker exec -it resourcemanager hadoop jar /tmp/Q2Analysis.jar /q2_output_A /q2_output_B B

-------------------------------------
OUTPUT FORMAT
-------------------------------------

Q1A - Word Count:
word    count
a       4697
aback   2

Q1B - Target Words:
sea     1234
ship    123
whale   34

Q1C - Unique Words by Pattern:
10,a    104
10,b    91

Q2A - Inverted Index:
abbeville   1769, 2759, 6617, 16360
abbey       33949, 48196

Q2B - Max Occurrence Words:
abbeville   6

-------------------------------------
NOTES
-------------------------------------
- The third argument (A/B/C) specifies which subproblem to run.
- All input is automatically converted to lowercase to normalize words.
- In Q2A, only alphabetic words are considered (numbers/dates ignored).
- In Q2A, the first token of each line is treated as the line number.
- Q2B uses the output file from Q2A as its input.

End of README
```
