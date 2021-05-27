node {
   // properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    def mavenhome = tool name: "maven3.8.1"
    stage('checkout code')
    {
        git branch: 'development', credentialsId: '86038a28-671c-4d35-ba0c-6d8f4498e784', url: 'https://github.com/alekhyareddyk/maven-web-application.git'
    }
    stage('build')
    {
        sh "${mavenhome}/bin/mvn clean package"
    }
    stage('execute sonarqube report')
    {
        sh "${mavenhome}/bin/mvn sonar:sonar"
    }
    stage('upload artifacts into nexus')
    {
        sh "${mavenhome}/bin/mvn deploy"
    }
    stage('deploy app into tomcat servevr')
    {
        sshagent(['3c45726a-6b45-4084-9657-ba1cf48050ef']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.225.4:/opt/apache-tomcat-9.0.45/webapps/"
}
    }
    stage('send email notification')
    {
    emailext body: '''build over

regards,
alekhyareddy''', subject: 'build over', to: 'alekhya.kondreddy@gmail.com'    
    }
}
