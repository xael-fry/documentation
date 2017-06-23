# Docker 
 
## Windows

Docker use a default VM that contains the containers. By default the VM shared the folder \\?\c:\Users as c/Users, so you may need to add a folder if you want put the code in another folder Example E:/devel

```batch   
docker-machine stop default
VBoxManage sharedfolder add default --name "e/devel" --hostpath "E:\devel" 
VBoxManage showvminfo default | grep -i share -A4
docker-machine start default
```





