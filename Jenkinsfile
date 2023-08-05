// def imageName = 'richinex/movies-loader'
// def registry = "${env.ACCOUNT_ID}.dkr.ecr.${env.REGION}.amazonaws.com"
// def region = env.REGION // Updating the region here

// node('workers'){
//     stage('Checkout'){
//         checkout scm
//     }

//     stage('Unit Tests'){
//         def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
//         imageTest.inside{
//             sh "python test_main.py"
//         }
//     }

//     stage('Build'){
//         docker.build(imageName)
//     }

//     stage('Push'){
//     sh "aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${registry}"

//     if (env.BRANCH_NAME == 'develop') {
//         sh "docker tag ${imageName} ${registry}/${imageName}:develop"
//         sh "docker push ${registry}/${imageName}:develop"
//     }
// }

// }


def imageName = 'movies-loader' // removed username
def dockerHubUsername = env.DOCKERHUB_USERNAME
def dockerHubPassword = env.DOCKERHUB_PASSWORD
def ecrRegistry = "${env.ACCOUNT_ID}.dkr.ecr.${env.REGION}.amazonaws.com"
def region = env.REGION

node('workers'){
    stage('Checkout'){
        checkout scm
    }

    stage('Unit Tests'){
        def imageTest= docker.build("${dockerHubUsername}/${imageName}-test", "-f Dockerfile.test .")
        imageTest.inside{
            sh "python test_main.py"
        }
    }

    stage('Build'){
        docker.build("${dockerHubUsername}/${imageName}")
    }

    stage('Push to ECR'){
        sh "aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${ecrRegistry}"
        if (env.BRANCH_NAME == 'develop') {
            sh "docker tag ${dockerHubUsername}/${imageName} ${ecrRegistry}/${dockerHubUsername}/${imageName}:develop"
            sh "docker push ${ecrRegistry}/${dockerHubUsername}/${imageName}:develop"
        }
    }

    stage('Push to DockerHub'){
        sh "docker login -u ${dockerHubUsername} -p ${dockerHubPassword}"
        if (env.BRANCH_NAME == 'develop') {
            sh "docker tag ${dockerHubUsername}/${imageName} ${dockerHubUsername}/${imageName}:develop"
            sh "docker push ${dockerHubUsername}/${imageName}:develop"
        }
    }
}
