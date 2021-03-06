TrajOpt-Docker

This contains a workspace setup dockerfile I will use for my research on trajectory optimization with factor graphs

### Build Docker
0. git clone this project to a location on the machine
```shell
~/Documents/TrajOpt-Docker
```

1. Add a github token to your system so that you can access biorobotics lab private repos. When you got the github token following [Github guide](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line).

   Put it as a environment variable or put it into ~/.bashrc. Setup GITHUB_TOKEN is very important, otherwise you will see a lot of failures when the Dockerfile tries to pull various git repos.
```shell
export GITHUB_TOKEN=XXXXXXXXXXXXXXXXXXXXXXXXXX
```


2. Build the docker image 
```shell
sudo docker build --build-arg GITHUB_TOKEN=${GITHUB_TOKEN} -t="work_image" .
```
here "work_image" is the name of the docker image you are going to build. Wait for docker image to build. It will take very long time. You can study [docker tutorials](https://docker-curriculum.com/) to understand more about how to check and manage docker images. There are many good online resources.

3. Start a docker container. Notice this docker container will mount a folder on the host machine to a location in the docker. So that development can happen on the host machine. Other argument are used to enable GUI (so we can run matplotlib inside the docker and see the result on the host)
```shell
sudo docker run -it \
                --network host \
                -e DISPLAY=$DISPLAY \
                -u docker \
                -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
                --name traj_opt_docker \
                -v ~/Documents/TrajOpt-Docker/ros_pkgs:/root/catkin_ws/src/link_ros_pkgs \
                work_image
```
Then a important command should be executed on the host computer to enable GUI from the docker 
```shell
xhost + 
```
This command will output "access control disabled, clients can connect from any host", so your docker container can now launch GUI. For example, the ct_ilqr program will start a matplotlib GUI to display trajectories.

4. After the initial container construction. Use following commands to access the container 
```shell
sudo docker start traj_opt_docker
sudo docker stop traj_opt_docker
sudo docker exec -it traj_opt_docker bash
```

### Test Code

This docker has primarily control_toolbox and gtsam installed. I will use it to study how to solve contained optimal control using factor graphs

in ros_pkgs

    1. test_ct_gtam. Test installation of control_toolbox and gtsam
        


##        