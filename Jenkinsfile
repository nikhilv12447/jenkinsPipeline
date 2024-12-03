pipeline{
    agent any
    parameters {
        string defaultValue: 'main', name: 'branch', trim: true
    }
    stages{
        stage("Build"){
            steps{
                sh '''
                repo="git@github.com:nikhilv12447/HelloWorldFrontend.git"
                if ! ls | grep "HelloWorldFrontend" > /dev/null
                then
                    git clone ${repo} 
                fi
                cd HelloWorldFrontend
                if ! git remote | grep "origin" > /dev/null
                then
                    git remote add origin ${repo}
                fi
                git fetch origin ${branch}
                git checkout ${branch}
                git pull origin ${branch}
                echo #{PATH}
                . ~/.nvm/nvm.sh
                if ! node -v > /dev/null
                then
                    nvm install 20.16.0
                fi
                npm install
                npm run build-server
                '''
            }
        }

        stage("Deploy"){
            steps{
                echo "Deploying"
            }
        }
    }
}