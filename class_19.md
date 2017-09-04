# Dockerized installation

1.  Download Image.  
Pull image from oficial repository:  
 `docker pull mongo:latest`  
**OR**, preferably inside the school download image from [this link](http://networking.itsv.edu.ar/descargas/mongo/mongodb.docker.img) and :  
 `docker load -i mongodb.docker.img`   
    
2. Run command to load container.  
 ` docker run --name mongo-test -v path/to/files:/data/db -p 27017:27017 -d mongo`  

2. Start container (on reboot or stop container).  
 ` docker start mongo-test `   

# Ubuntu installation

Please, see [this link] (https://docs.mongodb.com/tutorials/install-mongodb-on-ubuntu/).  

# Install tools

On ubuntu 16.04:

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6  

echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list  

sudo apt-get update  

sudo apt-get install mongodb-org-shell  mongodb-org-tools 
```
 


# Install dataset  
1. Download ide from (this link)[http://networking.itsv.edu.ar/descargas/mongo/mongobooster-4.0.1-x86_64.AppImage]  
   Change linux permissions `chmod +x mongobooster-4.0.1-x86_64.AppImage`  
1. Download dataset from (this link)[http://networking.itsv.edu.ar/descargas/mongo/primer-dataset.json]  
1. Download import script from (this link)[http://networking.itsv.edu.ar/descargas/mongo/import_script.js]
1. Run IDE (Doble click)  
1. Create new conection (host: localhost:27017)
1. Connect to database
1. Click on local-> create collection -> name collection as restaurant
1. Select restaurant collection
1.  Open script **'import_script.js'**
1. Change file_js url, and name of connection.
1. Run Script
1. :umbrella_on_ground: 

### OR....  
If you install mongo shell, can try  this method:  

`mongoimport --db test --collection restaurants --drop --file ~/downloads/primer-dataset.json`  
