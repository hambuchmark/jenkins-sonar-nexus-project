pipeline {
    agent any
    environment {
        SONAR_URL = "http://172.18.0.6:9000"
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "172.18.0.4:8081"
        NEXUS_CREDENTIAL_ID = "nexus"
     }
    stages {
        
        stage('Clone Github repository') {
            steps {
                git branch: 'main', url: 'https://github.com/hambuchmark/java-teszt.git'
             }
        }
    
    
        stage('Build with Maven'){
            steps{
                sh 'mvn clean jacoco:prepare-agent package jacoco:report -DargLine="@{argLine} -Dxxx=yyy"'
            }
        }
    
        stage('Jacoco Coverage Reports') {
            steps {
                jacoco( 
                    execPattern: 'target/*.exec',
                    classPattern: 'target/classes',
                    sourcePattern: 'src/main/java',
                    exclusionPattern: 'src/test*'
                )
                echo "Publishing Jacoco Code Coverage Reports";
            }
        }
         
        stage('Sonarqube analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.host.url=$SONAR_URL'
                }
            }
        }
    
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    NEXUS_REPOSITORY = pom.version.endsWith("SNAPSHOT") ? "java-app-snapshot" : "java-app-release";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    
    }
}