## Getting tab completion in powershell

tab completion so you can use tabs in the powershell command line

```{powershell}
Install-Module posh-docker
Import-Module posh-docker
```

posh docker will not by imported by default, ensure to add to the `$PROFILE`, if it exists. the following code from the docker site explains how to make it persistent across your powershell session

```{powershell}
if (-Not (Test-Path $PROFILE)){
    New-Item $PROFILE -Type File -Force
}
Add-Content $PROFILE "!nImport-Module posh-docker"
```


## Using docker toolbox

check out info on docker machine here https://docs.docker.com/machine/concepts/


https://docs.docker.com/toolbox/toolbox_install_windows/

- For machines that don't have native docker support (win7 etc)
- Click the Docker quickstart shortcut on the desktop
- `docker-machine ls` will list running docker images
- uses http://192.168.99.100  
