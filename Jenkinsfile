node 
{
 checkout scm;
  
 
   def url = readProperties  file: 'propfolder/properties.txt'
   stage('checkout') 
   { 
   
   echo "${url}"
   def Var1= url.APP_GIT_URL
   echo "Var1=${Var1}"
   git "${Var1}"
  
   }
   stage('Build')
   {
   def Var2= url.APP_BUILD
   echo "${Var2}"
     sh "${Var2}"
   }
   stage('SonarQube analysis') 
    { 
    def Var3= url.APP_SONAR1
    def Var4=url.APP_SONAR2
   Sonarurl=Var3+Var4
    echo "${Sonarurl}"
     sh "${Sonarurl}"
      }
 
stage ('deploying artifact'){
def server =Artifactory.server 'artifact'
def uploadSpec="""{
"files":[
{
 "pattern":"**/*.war",
 "target":"repo/"
}
]
}"""
server.upload(uploadSpec)
}
 stage ('deloy to tomcat'){
 sshagent(['tomcat-dev']) {
    sh 'cp **/*.war minduseradmin@10.0.0.4:/opt/tomcat/webapps'
}
 }

}
