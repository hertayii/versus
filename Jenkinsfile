pipeline {
  agent {
    kubernetes {
      yaml """
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        some-label: some-label-value
      namespace: test
    spec:
      containers:
      - name: jenkins-slave
        image: bluewhale666/jenkins-slave:1.0
        command:
        - cat
        tty: true
        env:
        - name: DOCKER_HOST
          value: 'tcp://localhost:2375'
      - name: dind-daemon
        image: 'docker:18-dind'
        command:
        - dockerd-entrypoint.sh
        tty: true
        securityContext:
          privileged: true
    """
    }
  } 
  stages {
    stage('BUILDING FRONTEND') {
      steps {
        sh "./frontend.sh"
      }      
    }    
  }
}
