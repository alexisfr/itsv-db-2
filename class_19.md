# Dockerized installation

1.  Download Image.  
Pull image from oficial repository:  
 `docker pull mongo:latest`  
**OR**, preferably inside the school download image from [this link](http://networking.itsv.edu.ar/descargas/mongo/) and :  
 `docker load -i mongodb.docker.img`   
    
2. Run command to load container.  
 ` docker run -v path/to/files:/data/db -p 27017:27017 mongo`   

# Ubuntu installation

Please, see [this link] (https://docs.mongodb.com/tutorials/install-mongodb-on-ubuntu/).