# documentation


On the following:

grep -rnw '/path/to/somewhere/' -e 'pattern'

    -r or -R is recursive,
    -n is line number, and
    -w stands for match the whole word.
    -l (lower-case L) can be added to just give the file name of matching files.

Along with these, --exclude, --include, --exclude-dir flags could be used for efficient searching:

    This will only search through those files which have .c or .h extensions:

    grep --include=\*.{c,h} -rnw '/path/to/somewhere/' -e "pattern"

    This will exclude searching all the files ending with .o extension:

    grep --exclude=*.o -rnw '/path/to/somewhere/' -e "pattern"

    For directories it's possible to exclude a particular directory(ies) through --exclude-dir parameter. For example, this will exclude the dirs dir1/, dir2/ and all of them matching *.dst/:

    grep --exclude-dir={dir1,dir2,*.dst} -rnw '/path/to/somewhere/' -e "pattern"

This works very well for me, to achieve almost the same purpose like yours.

For more options check man grep.


I’m looking for some way to clean up the contents of /var/lib/docker/overlay (or /var/lib/docker/overlay2 with overlay2 - I run both, but on different nodes, both seem to have this issue).
```
docker rm -vf $(docker ps -aq)
docker rmi -f $(docker images -aq)
docker volume prune -f
```

docker system prune -a -f


docker info | egrep '^(Server Version|Storage)'

docker volume prune -f
Total reclaimed space: 0 B
[root@*** ~]# docker system prune -a -f
Total reclaimed space: 0 B

here’s a lot of dirs left behind in /var/lib/docker/overlay2:

[root@*** ~]# ls -la /var/lib/docker/overlay2/ | wc -l


Just to keep the record, I have done some clean up using:
```
docker volume rm $(docker volume ls -qf dangling=true)
```
This will not delete any container or any volume in use!


    Modifier le fichier  /home/jenkins/.ssh/known_hosts en supprimant l entrée

ssh-keygen -R scm.ul.mediametrie.fr


ssh-keygen -R [hostname]
ssh-keygen -R [ip_address]
ssh-keygen -R [hostname],[ip_address]
ssh-keyscan -H [hostname],[ip_address] >> ~/.ssh/known_hosts
ssh-keyscan -H [ip_address] >> ~/.ssh/known_hosts
ssh-keyscan -H [hostname] >> ~/.ssh/known_hosts



MM]# nc -zvw 1 dvbdms92.mediametrie.fr 3306
Connection to dvbdms92.mediametrie.fr 3306 port [tcp/mysql] succeeded!
[root@gosu appliMM]#
