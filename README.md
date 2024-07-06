# docker installtion in ubuntu
# Step 1 — Installing Docker
# First, update your existing list of packages:
sudo apt update
# Next, install a few prerequisite packages which let apt use packages over HTTPS:
sudo apt install apt-transport-https ca-certificates curl software-properties-common
# Then add the GPG key for the official Docker repository to your system:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# Add the Docker repository to APT sources:
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
# This will also update our package database with the Docker packages from the newly added repo.
# Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
apt-cache policy docker-ce
#  install Docker:
sudo apt install docker-ce
# Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:
sudo systemctl status docker

# Step 2 — Executing the Docker Command Without Sudo (Optional)

# If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:
sudo usermod -aG docker ${USER}
# To apply the new group membership, log out of the server and back in, or type the following:
su - ${USER}
# You will be prompted to enter your user’s password to continue.
# Confirm that your user is now added to the docker group by typing:
groups
# If you need to add a user to the docker group that you’re not logged in as, declare that username explicitly using:
sudo usermod -aG docker username

# Step 3 — Using the Docker Command
# The rest of this article assumes you are running the docker command as a user in the docker group. If you choose not to, please prepend the commands with sudo.

docker docker-subcommand --help
# To view system-wide information about Docker, use:
docker info

# Step 4 — Working with Docker Images
# To check whether you can access and download images from Docker Hub, type:
docker run hello-world
# You can search for images available on Docker Hub by using the docker command with the search subcommand. For example, to search for the Ubuntu image, type:
docker search ubuntu
# Execute the following command to download the official ubuntu image to your computer:
docker pull ubuntu
# To see the images that have been downloaded to your computer, type:
docker images
# Step 5 — Running a Docker Container
# As an example, let’s run a container using the latest image of Ubuntu. The combination of the -i and -t switches gives you interactive shell access into the container:
docker run -it ubuntu

# Step 6 — Managing Docker Containers
# After using Docker for a while, you’ll have many active (running) and inactive containers on your computer. To view the active ones, use:
docker ps
# To view all containers — active and inactive, run docker ps with the -a switch:
docker ps -a

# To view the latest container you created, pass it the -l switch:
docker ps -l
# To start a stopped container, use docker start, followed by the container ID or the container’s name. Let’s start the Ubuntu-based container with the ID of 1c08a7a0d0e4:
docker start 1c08a7a0d0e4

# To stop a running container, use docker stop, followed by the container ID or name. This time, we’ll use the name that Docker assigned the container, which is quizzical_mcnulty:
docker stop quizzical_mcnulty

# Once you’ve decided you no longer need a container anymore, remove it with the docker rm command, again using either the container ID or the name. Use the docker ps -a command to find the container ID or name for the container associated with the hello-world image and remove it.
docker rm youthful_curie

# Step 7 — Committing Changes in a Container to a Docker Image
# Then commit the changes to a new Docker image instance using the following command.

docker commit -m "What you did to the image" -a "Author Name" container_id repository/new_image_name
docker commit -m "added Node.js" -a "sammy" d9b100f2f636 sammy/ubuntu-nodejs

# Listing the Docker images again will show the new image, as well as the old one that it was derived from:
docker images

# Step 8 — Pushing Docker Images to a Docker Repository
# To push your image, first log into Docker Hub.
docker login -u docker-registry-username
docker tag sammy/ubuntu-nodejs docker-registry-username/ubuntu-nodejs
# Then you may push your own image using:
docker push docker-registry-username/docker-image-name
 

 # To push the ubuntu-nodejs image to the sammy repository, the command would be:
docker push sammy/ubuntu-nodejs


# how to create the container
docker create id
# then start the container
docker start id
# go to the inside the container
docker exec -it id /bin/bash
# to list the all files
ls -ltr
# to create the new file
touch filename.txt
# to add the content
echo "adding the text" >> filename.txt
# to see the data 
cat filename.txt


# top 10 useful docker commands
1. docker --version
2. docker login -> to login docker hub
3. docker logout -> to logout
4. docker search nginx -> to search specific image from the global docker hub
5. docker pull nginx -> to pull the image
6. docker inspect image nginx -> to inspect the particular acitivity ( ex: image, network, and etc)

# container operations
# url -> https://www.youtube.com/watch?v=h6fQnzf3fXY&list=PLFoX_td1iTj-ATh48yU4qudcsLPi2kzv0&index=6
7. docker images / docker image ls -> to see the docker images
8. docker create nginx -> to create the container
9. docker ps -> to see the only running containers
10. docker ps -a -> to see the all containers including stopped
11.  docker start containerid / container name -> to start the docker container
12. docker stop containerid / container name -> to stop the running container
13. docker start --name nginx-mahesh01 nginx -> to create the docker with custom name (--name is prefix, then custom name, then image name)
14. docker run --name ngnix_mahesh02 nginx -> to create the container, and will start and run (instead of using create, ps, and start commands)
15. docker run -d --name nginx_mahesh nginx -> this is detached mode, means with login to the container, we can do the operations, because if u want to terminate this container some time it won't work the escp, ctrl+c, ctrl+d, ect.
# run command basically combination of create and start commands.
# -d means detached mode, without login
16. docker kill containerid / container name -> sometimes if the container is not responding, this kill command forcefully terminate that container.(forcefully shutdown)

# to login into the container

17. docker exec -it containerid/container name /bin/bash -> (shell name-> /bin/bash or /sh for few of the containers will not work so use this commands based on the requrement), (-it means iteractive terminale)
18. hostname -> to check the hostname
# after login limited commands only will work
19. ls -> list 
20. ifconfig -> to check the ip address
21. exit -> to exit from the container
# remove the container, first stop and remove the containers
22. docker rm nginx_mahesh03 -> to remove the container
23. docker rm nginx_mahesh03 --force -> not good practice
24. docker rmi nginx / tagname of the image-> to remove the image (if don't have any reference, it will delete otherwise throw an error and will not remove the image from the list, based on the one image we can create multiple containers, so before removing it remove the all containers and do the remove image)


# Docker Networking
collections of devices established
# types
1. Bridge
2. Host
3. None

# to see the networks in docker
-> docker network ls
# while creating the container by default it will connect to the Bridge network, if you want to give another network wantedly, you can give

# create the container and accessing the network from out side
# bridge
-> docker run -d --name httpd01 -p 8080:80 httpd 

# host
-> docker run -d --name httpd02 --net host httpd

# none
-> docker run -d --name httpd02 --net none httpd

# to delete the all containre at a time 
-> docker ps rm containerid1 containerid2 containerid3

# another method of creating the container
docker container run -d --name nginx01 -p 80:80 nginx

# plain images
-> ubuntu, alpine, centos has the plain images, they create the docker file from scratch.


# Docker file creation
# From
-> defines the base image used to start the build process.
# ADD
-> copies the files from a source on the host into the container's own filesystem at the set destination.
# Copy
-> copies the files from a source on the host into the container's own filesystem at the set destination.

# difference b/w ADD and COPY
-> If you ADD, in the source place, you can give the file of the url, ex: google drive file
-> Unlike, if you use the COPY, that url will not work, within the folder/system only we can access the source file.
-> ADD another advantage is we can take the zip file as also in the source place, what it will do na it extract and copy into the container.

# CMD:
-> can be used for executing a specific command within the container

# ENTRYPOINT:
sets a default application to be used every time a container is created with the image.

# ENV:
-> sets environment variables.

# EXPOSE:
-> associates a specific port to enable networking between the container and to the outside

# MAINTAINER:
-> defines a full name and email address of the image creator.

# RUN:
-> is the central executing directive for Dockerfiles.

# USER:
-> sets the UID (or username) which is to run the container.

# VOLUME: 
-> is used to enable access from the container to a directory on the host machine.

# WORKDIR:
-> sets the path where the command, defined with CMD, is to be executed.

# LABEL:
-> allow you to add a label to your docker image.

# create the docker file through terminal

-> check the images and containres -> docker images && docker ps -a
-> create the folder ->  mkdir folder name
->  add the code -> then ctrl + X -> will ask are u sure want to save y/n -> type Y and hit the enter
-> ^ means ctrl button.
-> to delete the file -> rm Dockerfiley
-> then build the file, which you have created. -> docker build -t maheshyadav7702/ubuntuapache2 .



# docker with terra form -> need to see
https://www.youtube.com/watch?v=z5XW17BRh7k&pp=ygUbaG93IHRvIHJ1biBuZ2lueCBpbiB1YnVudHUg


# what is YAML
1. YAML (YAML Ain't Markup Language) is a human-readable data serialization format.
2. It is commonly used for configuration files and data exchange b/w systems.
3. YAML is designed to be easy to read and write, making it popular among developers and system adminstrators.
4. YAML used indentation to indicate the nesting of elements.
5. Spaces are recomenede over tabs, the number of spaces for identation should be consistent throught out the file.
6. Conventionally, two spaces are used, for indentation.
# scalars:
7. Scalars represents simple values, like strings, numbers, booleans, and null.
8. Scalaras don't have any indentation and can be expressed directly.
# Lists:
9. Lists are represented by using a hypen (-) followed by a space.
10. Lists can contain any combination of scalars, other lists, or mapings (key- value pairs).
ex: 
list_key:
  - item1
  - item2
  - sublist:
      - sub_item1
      - sub_item2
# Mappings:
11. Mapping represents key-value pairs and use a colon (:) to separate the key from the value.
12. Mapping can be nested within each other.
# Multiline Scalars:
13. If a scalar value spans multiple lines, you can use the | character to indicate a literal block scalar or > character to indicate a folder scalar.
ex:
multiline_key: |
  This is a
  multiline 
  scalar value

# tool for checking the yaml format
-> yaml init

# to create the new user in ubuntu

login as root user
create a user useradd user1
create a password for a user passwd user1
if you are using amazon ec2 linux add your user to wheel group by usermod -aG wheel user1
then, try to login as your user su user1
use any root command now sudo less /etc/shadow
it Prompt for a password now enter it..


# you must not run with sudo

RUNNER_ALLOW_RUNASROOT="1" ./config.sh --url https://github.com/basobaasnepal/BasobaasWeb --token DFGFSDF234sf3fg45hd


# nest  docker file

FROM node:alpine3.18
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD [ "npm","run", "start" ]

# mongoose with next js
https://www.youtube.com/watch?v=bMHs2pHkbsM&pp=ygU5bmdpbnggNDAzIGZvcmJpZGRlbiBlcnJvciBmb3IgZG9ja2VyaXplZCBjb250YWluZXIgbmV4dGpz

# next auth
https://www.youtube.com/watch?v=Cm6-3pVCPEI&pp=ygU5bmdpbnggNDAzIGZvcmJpZGRlbiBlcnJvciBmb3IgZG9ja2VyaXplZCBjb250YWluZXIgbmV4dGpz


# url use full commands

https://stackoverflow.com/questions/53293210/403-forbidden-nginx


# to change the directory in the ubuntu

docker exec -it mahesh-container sh