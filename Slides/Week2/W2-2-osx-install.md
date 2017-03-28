# MongoDB installation on OS X
  * Go to the MongoDB website’s download section and [download](https://www.mongodb.com/download-center#community) the community version of MongoDB.
  
    After downloading Mongo move the gzipped tar file (the file with the extension .tgz that you downloaded) to the folder where you want Mongo installed. In this case, we’ll say that we want Mongo to live in our home folder, and so the commands might look something like this:
   ```
   cd Downloads
   mv mongodb-osx-[version].tgz ~/
  ```
  * Extract MongoDB from the the downloaded archive, and change the name of the directory to something more palatable:
  
  ```
  cd ~/
  tar -zxvf mongodb-osx-[version].tgz
  mv mongodb-osx-[version] mongodb
  ```
  * Create the directory where Mongo will store data, create the “db” directory. You can create the directory in the default location by running
  
  ```
  mkdir -p /data/db
  ```
  
  * Make sure that the /data/db directory has the right permissions by running
    
  ```
  sudo chown -R `id -un` /data/db
   # Enter your password
   ```
   
  * after installing set PATH variable:
    
  ```
  vim ~/.bashrc
  ```
    
  * add following line:
    
  ```
  export PATH=${HOME}/mongodb/bin:$PATH
  ```
  
  * run `source ~.bashrc` or restart terminal to update settings
  * run the Mongo daemon, in one terminal window run `mongod`. This will start the Mongo server.
  * run the Mongo shell, with the Mongo daemon running in one terminal, type `mongo` in another terminal window. This will run the Mongo shell which is an application to access data in MongoDB.
  * to exit the Mongo shell run `quit()`
  * yo stop the Mongo daemon hit `ctrl-c`