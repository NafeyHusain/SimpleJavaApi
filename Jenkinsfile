@Library('my-shared-library') _

pipeline{
    agent any
    environment {
            AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
            AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        }

    parameters{
            choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
            string(name: 'aws_account_id', description: " AWS Account ID", defaultValue: '766934352283')
            string(name: 'Region', description: "Region of ECR", defaultValue: 'ap-south-1')
            string(name: 'ECR_REPO_NAME', description: "name of the ECR", defaultValue: 'nafeyahthenx566')
    }


    stages{
        stage('Git Checkout'){
            when { expression {  params.action == 'create' } }
            steps{
                script{
                    gitCheckout(
                                    branch: "main",
                                    url: "https://github.com/NafeyHusain/SimpleJavaApi.git"
                    )
                }
            }
        }
//         stage('Unit Test maven'){
//         when { expression {  params.action == 'create' } }
//             steps{
//                 script{
//                     mvnTest()
//                 }
//             }
//         }
//         stage('Integration Test maven'){
//         when { expression {  params.action == 'create' } }
//             steps{
//                 script{
//                     mvnIntegrationTest()
//                 }
//             }
//         }
//         stage('Static code analysis: Sonarqube'){
//         when { expression {  params.action == 'create' } }
//              steps{
//                  script{
//
//                            def SonarQubecredentialsId = 'sonar-api'
//                            staticCodeAnalysis(SonarQubecredentialsId)
//                        }
//              }
//         }
//         stage('Quality Gate Status Check : Sonarqube'){
//                  when { expression {  params.action == 'create' } }
//                     steps{
//                        script{
//
//                            def SonarQubecredentialsId = 'sonar-api'
//                            qualityGateStatus(SonarQubecredentialsId)
//                        }
//                     }
//                 }
        stage('Maven Build : maven'){
                         when { expression {  params.action == 'create' } }
                            steps{
                               script{

                                   mvnBuild()
                               }
                            }
                        }
        stage('Docker Image Build : ECR'){
                    when { expression {  params.action == 'create' } }
                        steps{
                            script{
                                 dockerBuild("${params.aws_account_id}","${params.Region}","${params.ECR_REPO_NAME}")
                            }
                        }
                    }
         stage('Docker Image Scan: trivy '){
                  when { expression {  params.action == 'create' } }
                     steps{
                       script{

                            dockerImageScan("${params.aws_account_id}","${params.Region}","${params.ECR_REPO_NAME}")
                      }
                    }
                }
         stage('Docker Image Push : ECR '){
                  when { expression {  params.action == 'create' } }
                    steps{
                        script{

                            dockerImagePush("${params.aws_account_id}","${params.Region}","${params.ECR_REPO_NAME}")
                       }
                     }
              }
         stage('Docker Image Cleanup : ECR '){
                when { expression {  params.action == 'create' } }
                    steps{
                      script{

                          dockerImageCleanup("${params.aws_account_id}","${params.Region}","${params.ECR_REPO_NAME}")
                       }
                   }
              }


    }

}