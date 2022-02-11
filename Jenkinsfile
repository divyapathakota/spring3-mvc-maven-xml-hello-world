pipeline{
    agent any
    stages{
        stage("scm"){
            steps{
                git credentialsId: 'github_credentials', url: 'https://github.com/divyapathakota/spring3-mvc-maven-xml-hello-world.git'
            }
        }
        stage("maven"){
            steps{
            sh 'mvn package'
                        }
        }
         stage("Artifact"){
            steps{
           archiveArtifacts artifacts: 'target/*.war'
                        }
        }
        stage("deploy"){
            steps{
                withCredentials([usernameColonPassword(credentialsId: 'remote_tomcatcredential', variable: 'tomcred')]) {
            sh "curl -v -u $tomcred admin:admin -T /var/lib/jenkins/workspace/git-maven/target/spring3-mvc-maven-xml-hello-world-1.0-SNAPSHOT.war 'http://ec2-3-109-157-222.ap-south-1.compute.amazonaws.com:8080/manager/text/deploy?path=/tomcat-pipeline'"
        }
        }
    }
}
        post {
        failure {
            script {
                currentBuild.result = 'FAILURE'
        }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "divyapathakota@gmail.com",
                sendToIndividuals: true])
        }
        }
}
