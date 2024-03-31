# Learn Jenkins App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

### How to run Docker Container in Jenkins 

This project is node.js. So we need node runtime environment to run javascript. However, Jenkins does not provide default node environment, Therefore we will be using docker container<br>
<br>
dashboard - > Jenkins setting -> plugin -> docker pipeline <br>

Then , we are able to use docker container in pippeline 

```
pipeline{
    agent any

    stages{
        stage('w/ docker'){
            agent{
                docker{
                    image 'node:18-alpine'
                }
            }
        }
    }

}
```
docker will pull the image if it has not been installed.

### Workspace synchronization
when using docker, I used agent stage twice and it made my workspace created seperately <br>
To avoid that, we can use reuseNode true.

```
pipeline {
    agent any

    stages {
        stage('w/o docker') {
            steps {
                sh '''
                    echo "without docker"
                    ls -la
                    touch container-no.txt
                    '''
            }
        }
        
         stage('w/ docker') {
             agent{
                 docker{
                     image 'node:18-alpine'
                     reuseNode true
                 }
             }
            steps {
                sh '''
                    echo "with docker"
                    ls -la   
                    touch container-yes.txt
                '''
            }
        }
        
    }
}

```

![image](https://github.com/vdespa/learn-jenkins-app/assets/126591306/2ac06aac-c6cf-439a-829f-7915676e2729)

### Using a git Repository in jenkins 
create Jenkinsfile in root directory on workspace <br>

![image](https://github.com/vdespa/learn-jenkins-app/assets/126591306/6c80a1fd-d789-46cf-abef-109b3d8545f2)

Then, Write the pipeline that you want to buid in Jenkinsfile<br>

After that, go to Jenkins , 
In the configure -> pipeline, simply switch definition pipeline to "pipeline script from scm(Source Code Management)" <br>
Then write as follows 

![image](https://github.com/vdespa/learn-jenkins-app/assets/126591306/95b7b969-cdea-4fb7-88b0-c7729edc20fd) <br>
![image](https://github.com/vdespa/learn-jenkins-app/assets/126591306/95b16bc5-78c4-4c1d-a260-ddefeeb36d32)

After completing everything, build Jenkins and it's complete.

If we have changes in Jenkinsfile, push to git again and build again. 
