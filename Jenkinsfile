node
{
    def mavenHome = tool name: "maven3.8.5"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    stage('CheckoutCode'){
        git credentialsId: 'c7d99d5c-1809-493f-98b6-6388276dd870', url: 'https://github.com/msuram17/maven-web-application.git'
    }
    
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('ExecuteSonarqubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('uploadartifactsintonexus'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('DeploytoTomcat'){
        sshagent(['e41cb2b5-62b9-4c00-89dc-3d14dae9600e']) {
         sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.2.119.3:/opt/tomcat/webapps/"
        }
    }

}
