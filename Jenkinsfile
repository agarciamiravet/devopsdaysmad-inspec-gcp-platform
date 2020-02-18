pipeline {
         agent any
         stages {
                 stage('Inspec GCP Platform Tests') {
                    steps {

                                withCredentials([file(credentialsId: 'gcp_credentials', variable: 'gcp_credentials')]) {
                                dir("${env.WORKSPACE}/src/inspec/devopsdaysmad-gcp-platform"){

                                      catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                                       sh '''
                                          export  GOOGLE_APPLICATION_CREDENTIALS=$gcp_credentials
                                          inspec exec . --attrs attributes.yml -t gcp:// --reporter cli junit:inspec_results.xml json:output.json
                                       '''
                                    }
                            }
                            }                         
                            }
                 }
                 
                    stage('Upload Tests to Grafana') {
                        steps {
                             dir("${env.WORKSPACE}/src/inspec/devopsdaysmad-gcp-platform"){                                   
                                   sh '''
                                        ls
                                        curl -F 'file=@output.json' -F 'platform=gcp-platform' http://localhost:5001/api/InspecResults/Upload
                                   '''                                   
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
