pipeline {
  agent {
    kubernetes {
      yaml """
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        some-label: some-label-value
    spec:
      containers:
      - name: jenkins-slave
        image: bluewhale666/hertayii:1.1
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
    stage('BUILD') {
      steps {
        sh 'docker-compose up -d'
      }      
    }
    stage('TESTING') {
      steps {
        sh  'docker exec -it backend ./manage.py migrate'
        // sh  label "migrating" script "docker exec -it backend ./manage.py loaddata data.json"
        // sh  label "migrating" script "docker exec -it backend ./manage.py test"
      }
    }
  }
}

