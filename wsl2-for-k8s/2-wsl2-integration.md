# WSL2 integration

## Check docker and kubectl version in WSL2
```
# Try to see if the docker cli and daemon are installed
docker version
# Same for kubectl
kubectl version
```
Should return and error.

## Enable WSL2 integration in Docker Desktop
Go to Docker Desktop Settings/Resources/WSL_Integration and enable integration with your distro.

## Check docker and kubectl version in WSL2 again
```
# Try to see if the docker cli and daemon are installed
docker version
# Same for kubectl
kubectl version
```
It should find both.