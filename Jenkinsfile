pipeline {
    agent any  // 选择执行环境，'any' 表示可以在任何可用节点上执行

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'master', description: 'Branch to build')
        string(name: 'NODE_VERSION', defaultValue: '22.12.0', description: 'Nodejs version')
    }
  
    stages {
        stage('Checkout') {
            steps {
                // 检出代码
                git branch: "${params.BRANCH_NAME}", url: 'git@github.com:leezone241201/simulator-front.git'
            }
        }

        stage('Build') {
            steps {
                sh "docker run --rm -v /home/ubuntu/docker-data/jenkins/data/workspace/simulator-front:/simulator -v /home/ubuntu/apps/front/simulator:/simulator/dist -e NPM_CONFIG_REGISTRY=https://mirrors.cloud.tencent.com/npm/ node:${params.NODE_VERSION} sh -c \"cd /simulator && npm install -g npm@10.9.2 && npm install && npm run build\""
            }
        }

    }

    post {
        success {
            // 构建成功后的步骤
            echo 'Build was successful!'
        }

        failure {
            // 构建失败后的步骤
            echo 'Build failed!'
        }

        always {
            // 始终执行，清理悬挂的镜像
            echo 'Cleaning up dangling images...'
            sh 'docker image prune -f'
        }
    }
}
