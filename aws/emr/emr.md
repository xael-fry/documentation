# documentation


configurer le metastore rds : https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hive-metastore-external.html (edited) 

In emr_configuration.json

```
 {
    "Classification": "hive-site",
    "Properties": {
      "javax.jdo.option.ConnectionURL": "jdbc:mysql://streaming-hive-metastore.czcrw6ft9di6.eu-west-1.rds.amazonaws.com:3306/hive_metastore?
createDatabaseIfNotExist=true",
      "javax.jdo.option.ConnectionDriverName": "org.mariadb.jdbc.Driver",
      "javax.jdo.option.ConnectionUserName": "admin",
      "javax.jdo.option.ConnectionPassword": "A9H3DQo7gWRZqCiAvEbd"
    }
  }
```

1.	Where is the jar ?

The jar can be found at the location-  /usr/lib/hive/auxlib/aws-glue-datacatalog-hive2-client.jar

Hive looks in /etc/hive/conf/hive-env.sh for the location of “AWSGlueDataCatalogHiveClientFactory”

export HIVE_AUX_JARS_PATH=${HIVE_AUX_JARS_PATH}${HIVE_AUX_JARS_PATH:+:}/usr/lib/hive-hcatalog/share/hcatalog

I replicated the scenario at my end, where I launched an EMR cluster 5.28.0 with the below configuration.
[
  {
    "Classification": "hive-site",
    "Properties": {
      "hive.metastore.client.factory.class": "com.amazonaws.glue.catalog.metastore.AWSGlueDataCatalogHiveClientFactory",
      "hive.metastore.schema.verification": "false"
    }
  }
]


I have unlinked the jar (aws-glue-datacatalog-hive2-client.jar) at location /usr/lib/hive/auxlib . To unlink, " sudo unlink aws-glue-datacatalog-hive2-client.jar " . When I did so, I could get the exactly same error that you have received , while launching the hive shell.

Further I would request you to check the soft link to jar in the location “/usr/lib/hive/auxlib” and add the soft link if it is missing.  If the issue persists, can you please provide the below details to investigate further:

1. EMR cluster id
2. Hive-site.xml
3. Output of the command ls -lrth at location ‘/usr/lib/hive/auxlib’
4. hive-env.sh

References:
[1] https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hive-metastore-glue.html 
[2] https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-metastore-external-hive.html 


Path of hive-site


/usr/lib/hive/conf/hive-site.xml





HADOOP_USER_NAME=hadoop

HADOOP_USER_NAME=hdfs

hdfs dfs -put /usr/share/aws/hmclient/lib/aws-glue-datacatalog-hive2-client.jar /user/oozie/share/lib/lib_20200320064851/hive/
hdfs dfs -put /usr/share/aws/hmclient/lib/aws-glue-datacatalog-hive2-client.jar /user/oozie/share/lib/lib_20200320064851/hive2/


oozie admin -shareliblist hive | grep glue
oozie admin -shareliblist hive2 | grep glue

sudo stop oozie

cp /usr/share/aws/hmclient/lib/aws-glue-datacatalog-hive2-client.jar /usr/lib/oozie/lib/
oozie admin -sharelibupdate 


these are the steps:


hdfs df -ls /user/oozie/share/lib/hive


oozie admin -shareliblist hive | grep glue ---> nothing here 


hdf dfs -put /usr/share/aws/hmclient/lib/aws-glue-datacatalog-hive2-client.jar /user/oozie/share/lib/hive/


hdfs dfs -chown oozie:oozie /user/oozie/share/lib/hive/aws-glue-datacatalog-hive2-client.jar 


oozie admin -sharelibupdate


oozie admin -shareliblist hive | grep glue ---> found jar
run the job , it worked 
I think this was not required 
cp /usr/share/aws/hmclient/lib/aws-glue-datacatalog-hive2-client.jar /usr/lib/oozie/lib/ 
i am thinking we missed oozie admin -sharelibupdate first time
copying to hive2 is also not required 
only hive can work
hive2 is for beeline
here you are using hive
if you want to use spark then you have to copy it to spark folder
if you want to use pig then in pig folder and hcatalog folder
in your case only copying to hive worked
i will send summary then...and resolve this case..
































