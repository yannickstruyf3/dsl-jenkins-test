pipeline {
  agent none
  stages {
    stage('Create bucket') {
            agent {
            kubernetes {
            defaultContainer 'dsl'
            yaml """
        kind: Pod
        spec:
            containers:
              - name: dsl
                image: ntnx/calm-dsl
                imagePullPolicy: Always
                command:
                 - /bin/sh
                 - -c
                 - |
                   while true; do sleep 1000; done
                tty: true
                volumeMounts:
                - name: workspace-volume
                  mountPath: /home/jenkins/agent
              - name: jnlp
                securityContext: 
                    runAsUser: 1000
                volumeMounts:
                - name: workspace-volume
                  mountPath: /home/jenkins/agent
            volumes:
                - emptyDir: {}
                  name: workspace-volume
        """
            }
        }
      steps {
        withCredentials(
            [
                usernamePassword(credentialsId: 'prism_central',usernameVariable: 'PC_USERNAME', passwordVariable: 'PC_PASSWORD'),
            ]) {
            sh '''
            ls .
        echo "Hello_World"
        whoami
        calm
            '''
        }
      }
    }
  }
}
