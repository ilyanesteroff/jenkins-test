pipeline {
    agent {
        label "macOS"
    }

    stages {
        stage('install-pip-deps') { 
            steps { 
                script { 
                    install() 
                } 
            } 
        }
        stage('deploy-to-dev') { 
            steps { 
                script { 
                    deploy("dev", 7001) 
                } 
            } 
        }
        stage('tests-on-dev') { 
            steps { 
                script { 
                    test("dev") 
                } 
            } 
        }
        stage('deploy-to-staging') { 
            steps { 
                script { 
                    deploy("staging", 7002) 
                } 
            } 
        }
        stage('tests-on-staging') { 
            steps { 
                script { 
                    test("staging") 
                } 
            } 
        }
        stage('deploy-to-preprod') { 
            steps { 
                script { 
                    deploy("preprod", 7003) 
                } 
            } 
        }
        stage('tests-on-preprod') { 
            steps { 
                script { 
                    test("preprod") 
                } 
            }
        }
        stage('deploy-to-prod') { 
            steps { 
                script { 
                    deploy("prod", 7004) 
                } 
            } 
        }
        stage('tests-on-prod') { 
            steps {
                script { 
                    test("prod") 
                } 
            } 
        }
    }
}

def install() {
    echo "Installing deps"
    checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings.git']]])
    sh "pip install -r requirements.txt"
}

def deploy(String env, int port) {
    echo "Deploying to ${env}"
    checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings.git']]])
    sh "pm2 delete greetings-app-${env} || true"
    sh "pm2 start app.py --name greetings-app-${env} -- --port ${port}"
}

def test(String env) {
    echo "Testing in ${env} environment"
    checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework.git']]])
    sh "npm install"
    sh "npm run greetings greetings_${env}"
}
