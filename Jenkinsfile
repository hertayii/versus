pipeline {
  agent {
    kubernetes {
      defaultContainer 'jenkins-slave'
      yamlFile 'jenkins.yaml'
    }
  }
  stages {
    stage ('Manage: Environment') {
      steps {
        script {
          if (env.GIT_BRANCH == 'prod') {
            stage ('Stage: Production') {
                env.STAGE = 'prod'
                sh 'echo ${STAGE}'
            }
          } else {
            stage ('Stage: Development') {
                env.STAGE = 'dev'
                sh 'echo ${STAGE}'
            }
          }            
        }
      }
    }
    stage('Image build: WEB') {
      steps {
        dir('frontend') {
          sh 'make build'
        }
      }
    }
    stage('Image build: API') {
      steps {
        dir('backend') {
          sh 'make build'
        }
      }
    }
    stage('Push to Repo') {
            parallel {
                stage('WEB-repo') {
                    steps {
                      dir('frontend') {
                        sh 'make push'
                      }
                    }
                }
                stage('API-repo') {
                    steps {
                        dir('backend') {
                          sh 'make push'
                        }
                    }
                }
            }
        }
    stage('Deploy to the EKS cluster') {
            parallel {
                stage('WEB') {
                    steps {
                      dir('frontend') {
                        sh 'make deploy'
                      }
                    }
                }
                stage('API') {
                    steps {
                        dir('backend') {
                          sh 'make deploy'
                        }
                    }
                }
            }
        }
  }
}
