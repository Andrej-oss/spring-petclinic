pipeline {
    agent any {
        triggers {
            pollSCM('* * * * *')
        }
        stage ('CheckOut') {
            step{
                git url: 'https://github.com/Andrej-oss/spring-petclinic.git',
                branch: 'main'
            }
        }
        stage('Build') {
            step{
                sh './mvnw clean package'
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
                regression {
                     emailext attachLog: true,
                             body: 'Please go to the \'${BUILD_URL}\' and verify the build',
                             compressLog: true,
                             recipientProviders: [upstreamDevelopers(), requestor(), developers()],
                             subject: 'Job \'${JOB_NAME}\' (\'${BUILD_NUMBER}\') is waiting for input'
                }
                fixed {
                     emailext attachLog: true,
                            body: 'Please go to the \'${BUILD_URL}\' and verify the build',
                            compressLog: true,
                            recipientProviders: [upstreamDevelopers(), requestor(), developers()],
                            subject: 'Job \'${JOB_NAME}\' (\'${BUILD_NUMBER}\') is waiting for input'

                }
                changed {
                     emailext attachLog: true,
                             body: 'Please go to the \'${BUILD_URL}\' and verify the build',
                             compressLog: true,
                             recipientProviders: [upstreamDevelopers(), requestor(), developers()],
                             subject: 'Job \'${JOB_NAME}\' (\'${BUILD_NUMBER}\') is waiting for input'
                                }
            }
        }
    }
}
