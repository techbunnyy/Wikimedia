# Wikimedia
Simple way to install Wikimedia with MariaDB integration on Kubernetes Cluser Via HELM

This helm package contains following resources:
    
    a. mediawiki deployment with 2 repicas and rolling update to maxsurge:1 and maxunavailable 0.
    
    b. mariadb deployment without any rolling update as updating DB in the same helm chart as application is not a good practice.
    
    c. A service which will expose both of these containers to nodeport.
    
    d. Persistent Volume of 5GiB on HostPath itself for both MediaWiki and MariaDB. 
    
    e. Additionally , I'm providing hpa.yaml as saperate file for horizontal Pod Autoscaling. Use if needed and package the Helm chart.
    
    f. Mediawiki is being installed from scratch on centos 8 image with all the dependencies.

  Note: You can change all these configuration as per your requirement and package the helm chart afterwards. Also , you can use secrets as well to store the passwords. 
  
  Assumptions :
  
    -- Kubernetes Cluster is Present with atleast 1 worker node or MiniKube.
    
    -- Directories for PV has been created as /mnt/mediawiki-data and /mnt/mariadb-data
    
    -- Docker login process is completed if you are creating your own docker Image.
    
    -- HELM has been installed and configured.
    
    -- And obviously , a person should know basics of Linux and Kubernetes :D
    
1.Start with Creation of Docker files - Refer the files from this repo for MediaWiki and MariaDB.

If you are using the images from my repository move directly to step 4 . 

2.Build a Docker Image using these docker files . See below example.

/dockerfiles/mediawiki/Dockerfile : For installing MediaWiki on CentOS 8 Image with all the dependencies and configurations.

/dockerfiles/mariadb/Dockerfile.mariadb : A base image of MariaDB itself with the additional configurations.

Optional :

/dockerfiles/mariadb/Dockerfile : For installing MariaDB on CentOS 8 Image with Environment and DataBase Configurations.

$ docker build -t YOUR_REPO_NAME/IMAGE_NAME:TAG -f Dockerfile .

<img width="1437" alt="image" src="https://github.com/techbunnyy/Wikimedia/assets/77633708/513d3c86-4964-4889-9892-35d27079a82f">

3.Push the Image to your Docker Hub Repository

$ docker push YOUR_REPO_NAME/IMAGE_NAME:TAG

<img width="1437" alt="image" src="https://github.com/techbunnyy/Wikimedia/assets/77633708/553c5fd2-f11c-4259-89c0-c2c670907d84">

4. if you are not making any updates or changes . Copy the helm chart TAR file provided in this Repository. and install on your kubernetes cluster as below.

    $ helm install TAG_NAME mediawiki-chart-0.1.0.tgz

   <img width="1437" alt="image" src="https://github.com/techbunnyy/Wikimedia/assets/77633708/95b259ea-235e-4ec1-9362-1a629a30157e">
