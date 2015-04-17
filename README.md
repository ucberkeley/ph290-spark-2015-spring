ph290-spark-2015-spring
===================

Spark materials for the workshop for PH290, Spring 2015, on MapReduce and distributed computing using Spark and Elastic MapReduce, April, 2015.

The files spark.Rmd and spark.md are used to create spark.html, which is the main document for the workshop. You can easily copy code chunks from either spark.Rmd or spark.md.

To get started, follow the instructions below. These instructions are also in the startSparkVirtualCluster.txt file and an abbreviated version of them appear in spark.html as well.

These instructions are intended for use on a Mac or Linux machine. On Windows, you may have to tweak things and will need access to the command line. 

0) As noted in (7) below, it's important that you delete your cluster when you're not actively using it. Don't leave it running overnight or you'll use up your AWS credits.

1) To download Spark, go to https://spark.apache.org/downloads.html, choose 
* 1.3.0
* source code
* direct download 

Click on link in #4 entry. Download and untar/zip the file.
```bash
tar -xvzf spark-1.3.0.tar.gz
```

2) Create UNIX environment variables containing your AWS credentials.
```bash
export AWS_ACCESS_KEY_ID=FOO
export AWS_SECRET_ACCESS_KEY=BAR
```

3) Put your private SSH key file on your local machine, ideally in ~/.ssh. Change permissions on the file:
```bash
chmod 400 ~/.ssh/your_ssh_key.pem
```

4) Navigate to spark-1.3.0/ec2 and start a spark cluster, specifying the number of worker nodes you want in your Spark cluster:
```bash
export CLUSTER_SIZE=12
cd spark-1.3.0/ec2
./spark-ec2 -k your_ssh_key_name -i ~/.ssh/your_ssh_key.pem --region=us-west-2  -s ${CLUSTER_SIZE} -v 1.3.0 launch sparkvm-<your_name>
```
Occasionally you'll get some error messages and things won't start up correctly.  Just kill the cluster (see step 7 below) and try again. 

Also note that I've had other glitches in starting up a Spark cluster. You should not get messages about files not found nor should the startup interrupt you to ask you to answer any questions. If either of these happen, something may have gone wrong. Feel free to email consult@stat.berkeley.edu  if you run into problems.

5) Once it has finished the setup (it will take 5-15 minutes), you can login as follows:
```bash
./spark-ec2 -k <your_ssh_key_name -i ~/.ssh/your_ssh_key.pem --region=us-west-2 login sparkvm-<your_name>
```

6) If your wireless connection on your local machine is interrupted, your connection to the Spark cluster can be affected and you might have to start over again. To protect against this, as soon as you login to the Spark cluster, do
```bash
screen -x -RR
```

This will put you in a shell session that you'll be able to go back to even if you lose your connection. If you lose the connection then when you connect back to the master node, type "screen -x -RR" again and you should be back where you were.

6) Now you can follow the template provided in the Spark workshop notes in spark.html to use Spark. The spark.Rmd is a Markdown (and therefore plain text file) used to generate spark.html and from which it may be easier to copy/paste syntax for the bash and Python code.

7) IMPORTANT: delete your cluster as soon as you are done with it. 

```bash
./spark-ec2 --region=us-west-2 --delete-groups destroy sparkvm-<your_name>
```
