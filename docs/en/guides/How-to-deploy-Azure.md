# How to Deploy skywalking in Azure VM

## Without Docker
1. Create an Azure Virtual machine with appropriate Operating System. eg: Ubuntu, Debian etc.
2. Install git, JDK11+ and Maven.
3. Clone the skywalking Project
    ```bash
    git clone https://github.com/apache/skywalking.git
    cd skywalking/
    git submodule init
    git submodule update
    ```
4. Run `./mvnw clean package -Dmaven.test.skip`
5. All packages are in `/dist` (.tar.gz for Linux and .zip for Windows).
6. Extract the tar file using  
     `tar -xvzf apache-skywalking-apm-bin.tar.gz`  
7. Start the server using `sh statup.sh` in /bin.
8. You can access the skywalking UI at `http://Azure VM IP:8080`


## Using Docker
We can use Makefile located at the root folder to build the docker image with the current codebase.

`make docker`  

We can start up backend cluster by docker-compose

```
cd skywalking/docker   
docker-compose up
```
You can access the skywalking UI at `http://Azure VM IP:8080`



