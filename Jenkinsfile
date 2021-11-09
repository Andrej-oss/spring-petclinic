pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                // Get some code from a GitHub repository
                git branch:'main', url:'https://github.com/Andrej-oss/spring-petclinic.git'

                // Run Maven on a Unix agent.
               // sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Build'){
            steps{

             sh './mvnw clean package'
            }
            post{
                always{
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
                changed{
                    emailext subject: "Job '${JOB_NAME}' ('${BUILD_NUMBER}') in '${currentBuild.result}'",
                        body: "Please go to the '${BUILD_URL}' and verify the build",
                        to: 'test@jenkins',
                        attachLog: true,
                        compressLog: true,
                        recipientProviders: [upstreamDevelopers(), requestor()]
                }
            }

        }

    }
}
