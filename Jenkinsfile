node{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
def mavenhome = tool name: 'maven3.8.4'

echo "Job name is:  ${env.JOB_NAME}"
echo "Node name is:  ${env.NODE_NAME}"
echo "Branch name is:  ${env.BRANCH_NAME}"

// Get the code from GitHub repo
stage ('CheckoutCode'){
git branch: 'development', credentialsId: '0aff45b7-bbf3-4069-8325-d0c609c453f5', url: 'https://github.com/vali04091988/maven-web-application.git'
}

//Do the buil using maven build tool 

stage ('Build'){
sh "${mavenhome}/bin/mvn clean package"
}

//Execute the sonarqube report

stage ('Execute sonarqube report'){
sh "${mavenhome}/bin/mvn sonar:sonar"
}


//Upload artifacts into artifactory Repo

stage ('Upload artifacts into artifactory repo'){
sh "${mavenhome}/bin/mvn deploy"
}

//Deploy application into Tomcat server

stage ('Deploy application'){
sshagent(['4302801e-2140-49a4-8c1a-805b55f7f9a0']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@172.31.11.186:/opt/apache-tomcat-9.0.64/webapps/"
}
}

}
