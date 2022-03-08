pipeline {
  agent {
    kubernetes {
      yaml: """
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        some-label: some-label-value
    spec:
      containers:
      - name: jenkins-slave
        image: mshaibek/jenkins-slave-312
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
        sh  label: "building" script: "docker-compose up -d"
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

