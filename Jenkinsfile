def imageName = 'richinex/movies-loader'
def registry = "${env.ACCOUNT_ID}.dkr.ecr.${env.REGION}.amazonaws.com"
def region = 'REGION'


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
        sh "aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${registry}/${imageName}"

        if (env.BRANCH_NAME == 'develop') {
            docker.image(imageName).push('develop')
        }

    }

}