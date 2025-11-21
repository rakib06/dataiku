# (base) dataiku@ubuntu1804:~/dss_data/bin$ 


# stop DSS if running (adjust path to your DATADIR)
~/dss_data/bin/dss stop || true

# (optional) backup existing spark dir
sudo mv /opt/spark /opt/spark.backup.$(date +%Y%m%d-%H%M) || true

# download Spark 3.4.1 prebuilt for Hadoop3 (example)
wget https://archive.apache.org/dist/spark/spark-3.4.1/spark-3.4.1-bin-hadoop3.tgz -O /tmp/spark-3.4.1.tgz

# extract to /opt
sudo tar -xzf /tmp/spark-3.4.1.tgz -C /opt
# create a stable symlink that DSS can point to
sudo ln -sfn /opt/spark-3.4.1-bin-hadoop3 /opt/spark

# make sure spark-submit works and shows 3.4.1
/opt/spark/bin/spark-submit --version

# run the Dataiku spark integration installer again
cd ~/dss_data/bin
./dssadmin install-spark-integration -sparkHome /opt/spark/

# start DSS
~/dss_data/bin/dss start
