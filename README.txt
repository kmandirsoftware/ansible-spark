ansible all -m ping -u ubuntu  --key-file=/home/ubuntu/.ssh/sparkenv.pem

ansible all -a "df -h" -u ubuntu --key-file=/home/ubuntu/.ssh/sparkenv.pem


################# To configure and start Spark Cluster
## After ansible still need to add sparkmaster ip and sparkslave ip to /etc/hosts on both machines manually
### Must first create EC2 instances with terraform
 ansible-playbook  downloadspark-playbook.yml --key-file=/home/ubuntu/.ssh/sparkenv.pem
 ansible-playbook  masterRun-playbook.yml --key-file=/home/ubuntu/.ssh/sparkenv.pem
 ansible-playbook  jobrun-playbook.yml --key-file=/home/ubuntu/.ssh/sparkenv.pem

#### examle Spark Appkcation 
spark-submit --class org.apache.spark.examples.SparkPi --master spark://sparkmaster:7077 --driver-memory 512m --executor-memory 512m --executor-cores 1 $SPARK_HOME/examples/jars/spark-examples*.jar 10
