pipeline {
agent any
 
 stages {
        stage('Push to arfifactory') { 
 
         steps {
          script {
           def jarName = "project-common-1.jar"
           echo "JAR NAME  ${jarName}"
          
           
          def gitbranch = "$GIT_BRANCH"
          echo "${gitbranch}"
    
           bat 'mvn clean install'
           
           rtUpload (
              serverId: "MyArtifactory",
              spec:
                  """{
                    "files": [
                      {
                        "pattern": "target/*project-common-1.jar",
                        "target": "phoenix-local/"
                      }
                   ]
                  }"""
              )
           
          }
         }
        }
 }
}
