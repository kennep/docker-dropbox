def image = 'dropbox'
def serviceName = 'dropbox-kenneth'
def registry = 'https://registry.wangpedersen.com'
def registryLogin = 'registry-login'

node {
  checkout scm

  docker.withRegistry(registry, registryLogin) {
    stage 'Build'
    def builtImage = docker.build(image, '--pull .')

    if (env.BRANCH_NAME == 'master') {
        stage 'Push'
        builtImage.push()
    }
  }

  stage 'Deploy'
  if (env.BRANCH_NAME == 'master') {
    build job: '/restart-service', parameters: [string(name: 'service', value: serviceName)]
  }
}
