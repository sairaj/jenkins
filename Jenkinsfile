pipeline {
    agent { 
        label 'ANSIBLE'
    }
    
    environment {                                       // Declaring at pipeline will allow all the stages to access this variable
        ENV_URL  = "pipeline.global.com"
        SSH_CRED = credentials('SSH_CRED') 
    }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
        disableConcurrentBuilds()
        disableResume()
        timeout(time: 1, unit: 'MINUTES')
    }

     triggers { 
        pollSCM('H */4 * * 1-5')
    }

    parameters {
        string(name: 'PERSON', defaultValue: 'Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }

    tools {
        maven 'mvn-381' 
    }

    stages {
          stage('Parallel-Stage')   {
            parallel {   
                stage('One') {
                    when { 
                        environment name: 'CHOICE', value: 'One'  
                    }
                    steps {
                        echo "I am Stage One Step"
                        echo "ENV_URL is ${ENV_URL}"
                        sh "mvn --version"
                        sh "hostname"
                        sh "sleep 300"
                    }
                }

                stage('Two') {
                    environment {                                       // Declaring at state will allow only that stage to access that variable
                        ENV_URL = "stage2.global.com"
                    }
                    steps {
                        echo "I am Stage Two Step"
                        echo "ENV_URL is ${ENV_URL}"
                        sh "sleep 300"
                    }
                }

                stage('Three') {
                    steps {
                        sh '''
                            echo Hello World
                            echo Hai World
                            echo I am using Pipeline Syntax Generator
                            sleep 300
                            env
                        '''
                    }
                }
            }
        }
    }
}