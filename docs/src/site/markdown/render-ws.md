# Render Web Services
The Render Web Services module provides [level 2 REST APIs] to persisted collections of tile and 
transform specifications (see [data model]).  The [Render Web Services API documentation] identifies 
the specific supported APIs.     

## Deployment Architecture
The Render Web Services module is packaged as a Java web application archive ([war file]) deployed 
on a [Jetty] web server/container.  It connects to a [MongoDB] database where tile and transform 
specifications are persisted (see [data model]).

The web server and database components can be run on the same machine for small scale processing or 
evaluation purposes.  For large scale processing needs, each component should be deployed on separate 
machines that are tuned for their specific purpose.

## Basic Installation
These installation instructions cover setup of an evaluation instance with a collocated web server and 
database running on the most recent stable release of Ubuntu (14.04 precise).  

### 1. Install Git and Maven
```bash
sudo apt-get install -y git maven
```
### 2. Clone the Repository
```bash
git clone https://github.com/saalfeldlab/render.git
```
### 3. Install JDK and Jetty
```bash
# assumes cloned render repository is in ./render
cd ./render
./render-ws/src/main/scripts/install.sh
```
### 4. Build the Render Modules
```bash
# assumes current directory is still the cloned render repository root (./render)
export JAVA_HOME=`readlink -m ./deploy/jdk*`
mvn package
```
### 5. Deploy Web Service
```bash
# assumes current directory is still the cloned render repository root (./render)
cp render-ws/target/render-ws-*.war deploy/jetty_base/webapps/render-ws.war
```
### 6. Install MongoDB
> These instructions were taken from <https://docs.mongodb.org/v3.0/tutorial/install-mongodb-on-ubuntu/>

```bash
# import MongoDB public GPG key
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10

# create a list file for MongoDB
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list

# reload local package database
sudo apt-get update

# install MongoDB packages (this should also start the mongod process)
sudo apt-get install -y mongodb-org=3.0.9 mongodb-org-server=3.0.9 mongodb-org-shell=3.0.9 mongodb-org-mongos=3.0.9 mongodb-org-tools=3.0.9
```

### 7. Start MongoDB
> The install process should start mongod, but you can be sure by running this.

```bash
sudo service mongod start
```

### 8. Start Jetty
```bash
# assumes current directory is still the cloned render repository root (./render)
deploy/jetty_base/jetty_wrapper.sh start
```

## Using the Installation
The [Render Web Services Example] workflow steps demonstrate how this installation can be used for a 
small example project.


  [data model]: <data-model.md>
  [Jetty]: <https://eclipse.org/jetty/>
  [level 2 REST APIs]: <http://martinfowler.com/articles/richardsonMaturityModel.html>
  [MongoDB]: <https://www.mongodb.org/>
  [Render Web Services API documentation]: <render-ws-api/render-ws-api.md>
  [Render Web Services Example]: <render-ws-example.md>
  [Swagger UI]: <http://swagger.io/swagger-ui/>
  [war file]: <https://docs.oracle.com/javaee/7/tutorial/packaging003.htm>