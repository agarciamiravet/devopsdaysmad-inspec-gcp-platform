pipeline {
         agent any
         stages {
                 stage('Inspec GCP Platform Tests') {
                    steps {

                                withCredentials([file(credentialsId: 'gcp_credentials', variable: 'gcp_credentials')]) {
                                dir("${env.WORKSPACE}/src/inspec/devopsdaysmad-gcp-platform"){
                                    sh '''
                                            export  GOOGLE_APPLICATION_CREDENTIALS=$gcp_credentials
                                            inspec exec . -t gcp:// --reporter cli junit:inspec_results.xml
                                        '''
                            }
                            }                         
                            }
                 }             
                }
         post {
        always {
            junit '**/src/inspec/devopsdaysmad-gcp-platform/inspec_results.xml'
        }
               }
}