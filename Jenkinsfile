def imageName = 'richinex/movies-loader'
def registry = "${env.ACCOUNT_ID}.dkr.ecr.${env.REGION}.amazonaws.com"
def region = env.REGION // Updating the region here

node('workers'){
    stage('Checkout'){
        checkout scm
    }

    stage('Unit Tests'){
        def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
        imageTest.inside{
            sh "python test_main.py"
        }
    }

    stage('Build'){
        docker.build(imageName)
    }

    stage('Push'){
    sh "aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${registry}"

    if (env.BRANCH_NAME == 'develop') {
        sh "docker tag ${imageName} ${registry}/${imageName}:develop"
        sh "docker push ${registry}/${imageName}:develop"
    }
}

}
