pipeline {
    agent any
     tools { 
        maven 'Maven' 
      
    }
    stages { 
     
 stage('checkout') { 
     steps {

            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'bd5d995c-0609-489e-be0d-09a600ea7b92', url: 'https://github.com/haresh-gurram/game-of-life.git']]])

      
     }
   }
   stage('Build') {
       steps {
       // Run the maven build

      //if (isUnix()) {
         sh label: '', script: 'mvn clean package'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
   
   stage('Sonarqube') {
    environment {
        def scannerHome = tool 'sonarqube';
    }
    steps {
      withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
    }
}

   stage('Artifact uploader') {
       steps{
nexusPublisher nexusInstanceId: '12345', nexusRepositoryId: '1234', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/gol-pipeline-job/gameoflife-web/target/gameoflife.war']], mavenCoordinate: [artifactId: 'gameoflife', groupId: 'com.wakaleo.gameoflife', packaging: 'war', version: '1.0']]]
           
       }
}




}
}
