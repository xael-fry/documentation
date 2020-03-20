# Jenkins

## Printing a list of credentials and their IDs


Printing a list of all the credentials in the system and their IDs (cf[doc](https://wiki.jenkins.io/display/JENKINS/Printing+a+list+of+credentials+and+their+IDs))

```
def creds = com.cloudbees.plugins.credentials.CredentialsProvider.lookupCredentials(
    com.cloudbees.plugins.credentials.common.StandardUsernameCredentials.class,
    Jenkins.instance,
    null,
    null
);
for (c in creds) {
     println(c.id + ": " + c.description)
}

```

http://pkg.jenkins-ci.org/redhat-stable/

https://s3.console.aws.amazon.com/s3/buckets/ul-migration/jenkins/?region=eu-west-1&tab=overview

## remove CR from file

```
sudo tr -d '\r' /etc/resolv.conf > resolv.conf

sudo sed 's/^M$//' /etc/resolv.conf > resolv.conf 
```

sudo dig +trace www.amazon.com




