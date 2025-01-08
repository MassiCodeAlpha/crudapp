


pipeline {
    agent any
    

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_TOKEN = credentials('SONAR-TOKEN')
        SONAR_ORGANIZATION = 'massicodealpha'
        SONAR_PROJECT_KEY = 'MassiCodeAlpha_crudapp'
    }

    stages {
        
        stage('Code-Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
  -Dsonar.organization=jenkins-project-123 \
  -Dsonar.projectKey=jenkins-project-123_ci-jenkins \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonarcloud.io '''
                }
            }
        }
       
        
      
       stage('Docker Build And Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-cred') {
                        def buildNumber = env.BUILD_NUMBER ?: '1'
                        def image = docker.build("massinissa96/crudapp:latest")
                        image.push()
                    }
                }
            }
        }
    
       
        stage('Deploy To EC2') {
            steps {
                script {
                        sh 'docker rm -f $(docker ps -q) || true'
                        sh 'docker run -d -p 3000:3000 massinissa96/crudapp:latest'
                        
                    
                }
            }
        }
        
}
}
