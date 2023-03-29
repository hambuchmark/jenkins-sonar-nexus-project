pipeline {
    agent any
    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_LOGIN = credentials('sqp_7d45d7967b1d1b55626612232bbfc6fd587d67b6')
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/hambuchmark/teszt.git'
            }
        }
        stage('Build') {
            steps {
                sh 'gradle clean build'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('My SonarQube Server') {
                    sh 'gradle sonarqube -Dsonar.projectKey=teszt -Dsonar.projectName="teszt"'
                }
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hambuchmark/teszt.git'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Building..."'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "/bin/sonar-scanner -Dsonar.projectKey=teszt"
                }
            }
        }
    }
}


pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.9.0/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git branch: 'main', url: 'https://github.com/tech-with-moss/sample-maven-project.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonar') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn sonar:sonar"
    }
        }
        }
       
    }
}


pipeline {
  agent any
  
  environment {
    SONAR_URL = "http://localhost:9000"
    NEXUS_REPO_URL = "http://localhost:8081/repository/maven-releases/"
  }
  
  stages {
    stage('Clone repository') {
      steps {
        git branch: 'main', url: 'https://github.com/hambuchmark/java-teszt.git'
      }
    }
    
    stage('Static Code Analysis') {
      steps {
        withSonarQubeEnv('sonar-scanner') {
          sh 'mvn sonar:sonar -Dsonar.host.url=$SONAR_URL'
        }
      }
    }
    
    stage('Build and Deploy') {
      steps {
        sh 'mvn clean install deploy -DaltDeploymentRepository=nexus::default::${NEXUS_REPO_URL}'
      }
    }
  }
}


pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/rchidana/calcwebapp.git'    
		            echo "Code Checked-out Successfully!!";
            }
        }
        
        stage('Package') {
            steps {
                sh 'mvn package'    
		            echo "Maven Package Goal Executed Successfully!";
            }
        }
        
        stage('Jacoco Reports') {
            steps {
                  jacoco()
                  echo "Publishing Jacoco Code Coverage Reports";
            }
        }

	stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn package sonar:sonar'
                }
            }
        }
        
    }
    post {
        
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
    
    }
}


nexusArtifactUploader(
        nexusVersion: 'nexus3',
        protocol: 'http',
        nexusUrl: "${NEXUS_URL}",
        groupId: 'com.example',
        version: version,
        repository: "${NEXUS_REPOSITORY}",
        credentialsId: "${NEXUS_LOGIN}",
        artifacts: [
            [artifactId: 'my-app',
             classifier: '',
             file: '/target/my-app-1.0-SNAPSHOT.jar',
             type: 'jar']
        ]
     )
