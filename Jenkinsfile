@Library('Shared') _
def mernMicroservicesHello = securityConfig("securelooper/mern-microservices-hello:${BUILD_NUMBER}",'')
def mernMicroservicesProfile = securityConfig("securelooper/mern-microservices-profile:${BUILD_NUMBER}",'')

pipeline {
    agent any

    parameters {
        choice(
            name: 'SERVICE_TO_BUILD',
            choices: ['ALL', 'frontend', 'backend-hello', 'backend-profile'],
            description: 'Select which part of the application to build and deploy.'
        )
    }

    stages {
        stage("Security Scans") {
            steps {
                script {
                    securityScan()
                }
            }
        }
    
        stage('Build Docker Images') {
            parallel {
                stage('Build Frontend') {
                    when {
                        expression {
                            params.SERVICE_TO_BUILD == 'ALL' || params.SERVICE_TO_BUILD == 'frontend'
                        }
                    }
                    steps {
                        echo "Building Frontend because '${params.SERVICE_TO_BUILD}' was selected."
                        dir('frontend') {
                            script {
                                echo "Building Docker image for frontend"
                                // def imageName = "securelooper/frontend:${BUILD_NUMBER}"
                                // docker.build(imageName, '.')
                            }
                        }
                    }
                }
                stage('Build Backend Hello') {
                    when {
                        expression {
                            params.SERVICE_TO_BUILD == 'ALL' || params.SERVICE_TO_BUILD == 'backend-hello'
                        }
                    }
                    steps {
                        echo "Building Backend Hello because '${params.SERVICE_TO_BUILD}' was selected."
                        dir('backend/helloService') {
                            script {
                                docker.build("${mernMicroservicesHello.DOCKER_IMAGE}")
                            }
                        }
                    }
                }
                stage('Build Backend Profile') {
                    when {
                        expression {
                            params.SERVICE_TO_BUILD == 'ALL' || params.SERVICE_TO_BUILD == 'backend-profile'
                        }
                    }
                    steps {
                        echo "Building Backend Profile because '${params.SERVICE_TO_BUILD}' was selected."
                        dir('backend/profileService') {
                            script {
                                docker.build("${mernMicroservicesProfile.DOCKER_IMAGE}")
                            }
                        }
                    }
                }
            }
        }
    }
}