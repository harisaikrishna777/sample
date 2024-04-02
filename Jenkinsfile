def registry = 'https://jfrogtest777.jfrog.io/'
pipeline {
    agent {
         node {
            label 'maven'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
}
    stages {
        stage("build"){
            steps {
                sh 'mvn clean deploy'
            }
        }
              stage("Jar Publish") {
             steps {
                script {
                        echo '<--------------- Jar Publish Started --------------->'
                         def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"arti-cred"
                         def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                         def uploadSpec = """{
                              "files": [
                                {
                                  "pattern": "jarstaging/(*)",
                                  "target": "mavenrepo-libs-release/{1}",
                                  "flat": "false",
                                  "props" : "${properties}",
                                  "exclusions": [ "*.sha1", "*.md5"]
                                }
                             ]
                         }"""
                         def buildInfo = server.upload(uploadSpec)
                         buildInfo.env.collect()
                         server.publishBuildInfo(buildInfo)
                         echo '<--------------- Jar Publish Ended --------------->'  
                
                }
            }   
        }




        
    /* #stage('SonarQube analysis') {
    #environment {
      #scannerHome = tool 'sonar-scanner'
    #}
    #steps {
    #withSonarQubeEnv('sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
      #sh "${scannerHome}/bin/sonar-scanner"
    #}
    #}
  #} */
} 
}
 
