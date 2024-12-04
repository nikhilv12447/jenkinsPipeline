pipeline{
    agent any
    environment {
        dirName = "HelloWorldFrontend"
    }
    parameters {
        string defaultValue: 'main', name: 'branch', trim: true
    }
    stages{
        stage("Build"){
            steps{
                sh '''
                export PATH="$PATH:/home/jenkins/.nvm/versions/node/v20.16.0/bin"
                rm -rf ${dirName}
                repo="git@github.com:nikhilv12447/${dirName}.git"
                if ! ls | grep "${dirName}" > /dev/null
                then
                    git clone ${repo} 
                fi
                cd ${dirName}
                if ! git remote | grep "origin" > /dev/null
                then
                    git remote add origin ${repo}
                fi
                git fetch origin ${branch}
                git checkout ${branch}
                git pull origin ${branch}
                echo #{PATH}
                if ! node -v > /dev/null
                then
                    nvm install 20.16.0
                fi
                npm install
                npm run build-server
                exit 0
                '''
            }
        }

        stage("Deploy"){
            steps{
                sh '''
                pwd
                tar -zcvf build.tar.gz ./${dirName}/build
                scp -r build.tar.gz beta@helloWorld.beta:~/builds
                ssh -t beta@helloWorld.beta 'cd builds && ./start_server.sh'
                exit 0
                '''
            }
        }
    }
}
