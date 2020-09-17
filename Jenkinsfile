pipeline {
    agent any
    
    stages{
        stage('Git Clone or Pull'){
            
            steps {
                git 'https://github.com/asquarezone/spring-petclinic.git'
            }
        }
        
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
                
            }
        }
        
        stage('Publish'){
            steps{
                archiveArtifacts 'target/springboot-petclinic-1.4.1.jar'
                junit 'target/surefire-reports/*.xml'
                
               // sh 'mvn sonar:sonar -Dsonar.projectKey=Spring-petclinic -Dsonar.host.url=http://20.57.123.154:9000 -Dsonar.login=9dcc7accbe6d3e26f47a5d2c3656978e2ff669e7'
                sh '''
                    mvn sonar:sonar \
                       -Dsonar.projectKey=spring-petclinic \
                       -Dsonar.host.url=http://spring-petclinic.eastus2.cloudapp.azure.com:9000 \
                       -Dsonar.login=0ee7c459de80370a0f011bd1e28ba5c9f6bc3857 '''
            }
        }

    }
	post {
        success {
            mail to: 'tadkauday11@gmail.com',
            subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] ",
            body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' ":</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""

        }
   
        failure {
            mail to: 'tadkauday11@gmail.com',
            subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Failed!",
            body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed!":</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""
        }
        always {
            mail to: 'tadkauday11@gmail.com',
            subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
            body: "${env.BUILD_URL} has result ${currentBuild.result}"
        }
    }
}
