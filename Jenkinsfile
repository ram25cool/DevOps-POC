node
{
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    server = Artifactory.server "artifactory"
    // Create an Artifactory Maven instance.
     rtMaven = Artifactory.newMavenBuild()
     buildInfo
    
 rtMaven.tool = "Maven"

    stage('Clone sources') {
        git url: 'https://github.com/Ikram123-dev/Devops-POC.git'
    }
 
        stages {
stage('Sonarqube') {
    environment {
        scannerHome = tool 'SonarQubeScanner'
    }
    steps {
        withSonarQubeEnv('sonar') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
        }
        
      
   

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install'
    }

 stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
        rtMaven.tool = "Maven"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    }
    //stage('Publish build info') {
     //   server.publishBuildInfo buildInfo
 //   }
    }
