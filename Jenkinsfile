pipeline{
    agent any
    tools{
        maven'mymaven'
        jdk'myjava'
    }
    stages{
        stage("Compile"){
            steps{
              script{
                 echo "Compiling the code"
                 sh 'mvn compile'
              }
            }
        }
        stage("test"){
             steps{
                script{
                  echo "Testing the code"
                  sh 'mvn test'
                  }
             }
             post{
                 always{
                     junit 'target/surefire-reports/*.xml'
                 }
             }
        }
        stage("Package"){
             steps{
                script{
                  echo "Packaging the code"
                  sh 'mvn package'
              }
             }
        }
        stage("Build the docker image"){
            steps{
                script{
                    echo "Building the dockerimage"
                    sh 'systemctl start docker'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PSWD', usernameVariable: 'USER')]) {
            sh 'sudo docker build -t prakashpragallapati/prakash:$BUILD_NUMBER .'
            sh 'sudo docker login -u $USER -p $PSWD'
            sh 'sudo docker push prakashpragallapati/prakash:$BUILD_NUMBER'
}
                }
            }
        }
        stage("Deploy the docker container"){
            steps{
                sript{
                    echo "Deploying the app"
                    sh 'sudo docker run -itd -P prakashpragallapati/prakash:$BUILD_NUMBER'   
                  }
            }
    }
}
}


