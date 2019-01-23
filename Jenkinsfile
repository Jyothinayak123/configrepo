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
// stage ('deloy to tomcat'){

   // sh ' cp **/*.war /opt/tomcat/latest/webapps/'

//}
  stage('docker')
    {
        sh 'docker --version'
        sh 'docker run -dit --name tomc -p 8084:8080 tomcat  '
        sh 'docker cp /home/minduseradmin/tomcat-users.xml tomc:/usr/local/tomcat/conf/tomcat-users.xml '
        sh 'docker cp /home/minduseradmin/context.xml tomc:/usr/local/tomcat/webapps/manager/META-INF/context.xml '
        sh 'docker restart tomc'
        sh 'docker cp **/*.war tomc:/usr/local/tomcat/webapps/'

        
    
}
 stage('Mail')
 {
  emailext body: '', subject: '', to: 'jyothinayak.123@gmail.com'
 }
 
}
