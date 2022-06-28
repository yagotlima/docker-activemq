node {
    def repository = 'yagotlima/docker-activemq'
    def builds = [
        [version: '5.15.9', baseImage: 'openjdk:8-jre', hash: '35cae4258e38e47f9f81e785f547afc457fc331d2177bfc2391277ce24123be1196f10c670b61e30b43b7ab0db0628f3ff33f08660f235b7796d59ba922d444f'],
    ]
    stage('checkout') {
        checkout scm
    }
    stage('build') {
        if (env.BRANCH_NAME == 'main') {
            dir('debian') {
                builds.each { build ->
                    sh "docker build --build-arg BASE_IMAGE=${build.baseImage} --build-arg ACTIVEMQ_VERSION=${build.version} --build-arg ACTIVEMQ_VERSION=${build.version} --build-arg SHA512_VAL=${build.hash} -t ${repository}:${build.version} ."
                }
            }
            dir('alpine') {
                builds.each { build ->
                    sh "docker build --build-arg BASE_IMAGE=${build.baseImage}-alpine --build-arg ACTIVEMQ_VERSION=${build.version} --build-arg ACTIVEMQ_VERSION=${build.version} --build-arg SHA512_VAL=${build.hash} -t ${repository}:${build.version}-alpine ."
                }
            }
            sh "docker tag ${repository}:${builds[0].version} latest"
            sh "docker tag ${repository}:${builds[0].version}-alpine latest-alpine"
        }
    }
    stage('publish') {
        if (env.BRANCH_NAME == 'main') {
            withDockerRegistry([credentialsId: 'dockerhub-yagotlima-activemq']) {
                sh "docker image push --all-tags $repository"
            }
        }
    }
}