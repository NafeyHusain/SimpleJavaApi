@Library('my-shared-library') _

pipeline{
    agent any

    stages{
        stage('Git Checkout'){
            steps{
                script{
                    gitCheckout(
                                    branch: "main",
                                    url: "https://github.com/NafeyHusain/SimpleJavaApi.git"
                    )
                }
            }
        }
        stage('Unit Test maven'){
            steps{
                script{
                    mvnTest()
                }
            }
        }
        stage('Integration Test maven'){
            steps{
                script{
                    mvnIntegrationTest()
                }
            }
        }
    }

}