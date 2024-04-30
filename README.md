# Create new instance t2 large ubuntu mechine .....>enable network settings 
java port 8080 and jenkis port 9000 connect ec2 instance 
---------------------------------------------------------------------------
installed apllications 
---------------------------------------------------------------------------
# ansible 
# java 
# maven
# Docker 
# git 
# jenkins
# sonarqube
----------------------------------------------------------------------------

doc jenkins install  setup
-------------------------
https://www.jenkins.io/doc/book/installing/linux/

 getent group | wc -l

install java 17
----------------
sudo apt update -y
sudo apt install fontconfig openjdk-17-jre -y
java -version

install jenkins ubuntu
-----------------------

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y

start service
----------------

sudo systemctl enable jenkins


sudo systemctl start jenkins


sudo systemctl status jenkins

-----------------------------------------


install docker
----------------
sudo apt install docker.io git maven -y
docker --version
docker ps

# after installed all applications copy public ID and paste new web page and java port 8080 after jenkins key open copy that key and paste into the server like "sudo cat paste key here and enter after display the password copy that password and paste it into the jenkis page after jenkins engine are started. 
to give jenkins access of docker
---------------------------------
# command to show groups present
    sudo cat /etc/passwd
    or
    getent passwd
# command to show groups present
    sudo cat /etc/group
    or
    getent group

#  if the user "jenkins" is a member of the groups "jenkins" and "docker",

groups jenkins
     the output would be:
     -----------------------
         jenkins : jenkins docker


 # docker: This is the name of the group to which the user jenkins will be added. In Unix-like systems, groups are used to control access to resources and files. Adding the jenkins user to the docker group typically allows the jenkins user to interact with Docker,

sudo usermod -aG docker jenkins

    -a: It stands for "append." When used with usermod, it appends the user to the specified group without removing the user from other groups.
    -G: It specifies the groups to which the user should be added. In this case, docker is the group name.

#  if the user "jenkins" is a member of the groups "jenkins" and "docker",

  groups jenkins

     the output would be:
     -----------------------
         jenkins : jenkins docker


chmod 777 /var/run/docker.sock

STEP 5 :-Create a dockerhub account and Add Dockerhub Credentials on Jenkins
-----------------------------------------------------------------------
- Setup Docker Hub Secret Text in Jenkins

You can set the docker credentials by going into -

Goto -> Jenkins -> Manage Jenkins -> Credentials ->
Stored scoped to jenkins -> global -> Add Credentials [
GIVE YOUR DOCKER HUB CREDENTIALS ]


STEP6 –STEP6 – ADD MAVEN IN GLOBAL TOOL CONFIGURATION
------------------------------------------------------------

 Jenkins -> Manage Jenkins -> Tools and [search for Maven installations]

1. Name
       maven3

          Install automatically?
           -- Install from Apache
               Version
                    3.9.6
            Add Installer

STEP 7 – ADD JENKINS SHARED LIBRARY
-------------------------------------
Go to Manage Jenkins -> Configure System -> Global
Pipeline Libraries ->
Give Library name –jenkins-shared-library-sing
Default Version - main
Project Repository -
https://github.com/vickydevo/jenkins-shared-library-sing.git


STEP 8 - Build, deploy and test CI/CD pipeline
-------  Create new Pipeline: Go to Jenkins Dashboard or
         Jenkins home page click on New Item and add the repo
         details
    https://github.com/vickydevo/spring-clud-k8s-sing.git


========================================================================

stage ("docker build"){
            steps{
                sh "docker build -t diclarativejob:latest ."

            }
            }
        stage ("docker hub login"){
            steps{
                sh "export DOCKER_USERNAME=mydoker987"
                sh "export DOCKER_PASSWORD=Thanks@2024"
                sh "echo $DOCKER_PASSWORD | sudo docker login -u $DOCKER_USERNAME --password-stdin"
                }
            }
        stage ("DOCKER_TAG"){
            steps{
                sh "docker tag diclarativejob:latest mydoker987/diclarativejob:latest"
            }
            }
        stage ("docker push"){
            steps{
                sh "sudo docker push mydoker987/diclarativejob:latest "
            }
            }

    }
}



sudo systemctl start docker
sudo systemctl status docker
cd /var/run
udo chmod 777 docker.sock
docker pull sonarqube
sudo docker images
sudo docker run -d --name sonarcontainer -p9000:9000 3cfdf60e6c70

open sonarqube interface
change administration to my account
navigate to security
create new token
sqa_87f1db852343ef6dfe196eab712bf90c8d596dab

goto administration
navigate to configuration select webhooks create




pipeline{

       agent any
            tools{
                maven "maven3"
                     }

        stages{

             stage("SCM CHECKOUT"){
                 steps{
                      git url: "https://github.com/chandu-987/springboot-hello.git",branch: "main"
                           }
              }

            stage("parallel_mvn_build"){
                 parallel{
                         stage("validate:maven"){
                              steps{
                                   sh "mvn validate"
                                       }
             }
                         stage("compile:maven"){
                              steps{
                                   sh "mvn compile"
                                       }
             }
                         stage("unit test :maven"){
                              steps{
                                   sh "mvn test"
                                       }
             }
                        stage("package:maven"){
                              steps{
                                   sh "mvn package"
                                       }
             }
                         stage("integration testing:maven"){
                              steps{
                                   sh "mvn verify"
                                       }
             }


         }

     }
                          stage("maven build"){
                              steps{
                                   sh "mvn clean package"
                                       }
             }


    }


}



secret code: sqa_4794b73c5bed52d7a6f3e14501541f04f8cbcff2


