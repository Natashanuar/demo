pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check-Git-Secrets'){
        steps{
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/Natashanuar/shoppingcartapp-web-V2.git > trufflehog'
        sh 'cat trufflehog'
       }
    }
    
    stage ('Software Composition Analysis') {
      steps {
         sh 'rm -r dependency-check* || true' 
         sh 'wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.0.3/dependency-check-6.0.3-release.zip'
         sh 'unzip dependency-check-6.0.3-release.zip'
         sh './dependency-check/bin/dependency-check.sh --scan ./* --enableRetired -f "ALL" '
               }
    }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
   /*  stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
    
    stage ('Build Execute Jar') {
      steps {
        sh 'mvn clean package'
         }
       }
    
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@35.225.146.167:/home/natashaanuar98/apache-tomcat-7.0.107/webapps/webapp.war'
              }      
           }       
    }
    
     stage ('DAST') {
      steps {
        sshagent(['zap']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@35.193.155.239 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://35.225.146.167:8080/shoppingcartapp-web-V2/" || true'
        }
      }
    }*/
  }
}
