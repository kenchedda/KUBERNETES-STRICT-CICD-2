  def label = "deploy"
  podTemplate(label: label, yaml: """
  apiVersion: v1
  kind: Pod
  metadata:
    labels:
      name: deploy
  spec:
    serviceAccount: jenkins-agent-sa
    containers:
    - name: deploy
      image: dpthub/jenkins-build-agent:1.0
      command:
      - cat
      tty: true
  """
  ) {
      node (label) {

          stage ('Checkout SCM'){
            git url: 'https://github.com/kenchedda/KUBERNETES-STRICT-CICD-2.git', branch: 'dev', credentialsId: 'github'
          }

          stage ('Helm Chart') {
            container('deploy') {
              dir('charts') {
                withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                      sh '/usr/local/bin/helm version'
                      sh '/usr/local/bin/helm repo add default-helm https://kenappiah.jfrog.io/artifactory/default-helm username jnrcheddabob@gmail.com --password Widaad@77'
                      sh "/usr/local/bin/helm repo update"
                      sh "/usr/local/bin/helm install web-dev --namespace dev -f values.yaml ."
                      sh "/usr/local/bin/helm list -a --namespace dev"
                }
              }
          }
          }
      }
  }