import java.text.SimpleDateFormat

pipeline {
agent any
 
 stages {
        stage('Push to arfifactory') { 
 
         steps {
          script {
          def release_repo = 'phoenix-libs-release'
          def snapshot_repo ='phoenix-snapshot-release'
           
          def dirName ='/com/example/project-common'
           
          def gitbranch = "$GIT_BRANCH"
          echo "${gitbranch}"
          def buildNum = "$BUILD_NUMBER"
           echo "build Number ${buildNum}"
          
           
           bat 'mvn clean install'
           def env =''
           
           if(gitbranch == 'origin/master') {
            env = 'staging'
            repo= "${release_repo}"
            
           } else {
            env = 'development'
            repo ="${snapshot_repo}"           
           }
           
           def image =  readMavenPom().getArtifactId()
           def version = readMavenPom().getVersion()
           
           echo "image ${image}"
           echo "version ${version}"
           
           def dt = new Date()
           def dateformat =  new SimpleDateFormat("yyyyMMdd.HHmmss")
           echo "date format ${dateformat.format(dt)}"
           
           // change the jar and pom names
           def jarName = "${image}"+"-"+"${version}"+"-"+"${dateformat.format(dt)}"+"-"+"${buildNum}"
            echo "JAR NAME  ${jarName}"
        
          // sh 'mv target/*SystemEventsService*.jar target/"${jarName}" '
           
           rtUpload (
              serverId: "MyArtifactory",
              spec:
                  """{
                    "files": [
                      {
                        "pattern": "target/*project-common*.jar",
                        "target": "phoenix-local/${repo}/${dirName}/${version}/"
                      }
                   ]
                  }""",
               buildNumber: "${buildNum}"
              )
           
           // this will fail as the pom structure does not match the directory structure being created in local artifactory
         rtUpload (
                       serverId: "MyArtifactory",
                       spec:
                               """{
               "files": [
                 {
                   "pattern": "pom.xml",
                   "target": "phoenix-local/${repo}/${dirName}/${version}/${jarName}.pom"
                 }
              ]
             }"""
               )
           
          }
         }
        }
 }
}
