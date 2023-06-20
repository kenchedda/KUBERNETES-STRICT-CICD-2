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
      image: kenappiah/build-agent:1.0
      command:
      - cat
      tty: true
  """
  ) {
      node (label) {

          stage ('Checkout SCM'){
            git url: 'https://github.com/kenchedda/KUBERNETES-STRICT-CICD-2.git', branch: 'qa'
          }

          stage ('Helm Chart') {
            container('deploy') {
              dir('charts') {
                withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                      sh '/usr/local/bin/helm repo add dptweb-helm-local  https://edwikifacts.jfrog.io/artifactory/edweb-helm-local --username $username --password $password'
                      sh "/usr/local/bin/helm repo update"
                      sh "/usr/local/bin/helm install dptweb-qa --namespace qa -f values.yaml ."
                      sh "/usr/local/bin/helm list -a --namespace qa"
                }
              }
          }
          }
      }
  }