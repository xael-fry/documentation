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
