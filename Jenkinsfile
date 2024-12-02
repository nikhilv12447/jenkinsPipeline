pipeline{
    agent any
    parameters {
        string defaultValue: 'main', name: 'branch', trim: true
    }
    stages{
        stage("Build"){
            steps{
                sh '''
                ssh-keygen -y
                #repo="git@github.com:nikhilv12447/HelloWorldFrontend.git"
                #if ! ls | grep "HelloWorldFrontend" > /dev/null
                #then
                #    git clone ${repo} 
                #fi
                #git checkout ${branch}
                #git pull origin ${branch}
                #npm install
                #npm run build-server
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