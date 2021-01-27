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
        sh 'docker run gesellix/trufflehog --json https://github.com/Natashanuar/demo.git > trufflehog'
        sh 'cat trufflehog'
       }
    }
    
 /* stage ('Software Composition Analysis') {
      steps {
        sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/Natashanuar/demo/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
         sh 'rm -r dependency-check* || true' 
         sh 'wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.0.3/dependency-check-6.0.3-release.zip'
         sh 'unzip dependency-check-6.0.3-release.zip'
         sh './dependency-check/bin/dependency-check.sh --scan ./* --enableRetired -f "ALL" '
       stage ('Dependency Check') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'dependencycheck'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
  //  }*/
    
    /* stage ('OWASP Dependency-Check Vulnerabilities') {  
    steps {  
     withMaven(maven : 'mvn-3.6.3') {  
      sh 'mvn dependency-check:check'  
     }  
   
     dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'  
    }  
   }  */
    
    
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
       }*/
    
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@35.239.79.249:/home/natasha_1998/prod/apache-tomcat-9.0.41/webapps/webapp.war'
              }      
           }       
    }
    
     /*stage ('DAST') {
      steps {
        sshagent(['zap']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@35.193.155.239 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://35.225.146.167:8080/shoppingcartapp-web-V2/" || true'
        }
      }
    }*/
  }
}
