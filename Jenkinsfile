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

                if ! git branch | grep "${branch}" 
                then
                    git fetch origin ${branch}
                    git checkout ${branch}
                elif [[ git branch --show-current != ${branch} ]]
                then
                    git checkout ${branch}                    
                fi

                git pull origin ${branch}

                if ! node -v > /dev/null
                then
                    nvm install 20.16.0
                fi
                npm install
                npm run build-prod
                exit 0
                '''
            }
        }

        stage("Deploy"){
            steps{
                sh '''
                cd ${dirName}
                tar -zcvf build.tar.gz ./build
                ssh -t beta@helloWorld.beta 'cd builds && rm -rf build.tar.gz'
                scp -r build.tar.gz beta@helloWorld.beta:~/builds
                ssh -t beta@helloWorld.beta 'cd builds && ./start_server.sh'
                exit 0
                '''
            }
        }
    }
}
