pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // SCM პოლინგი ყოველ 5 წუთში
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'stage', url: 'https://github.com/lmag1/jenktask.git'
            }
        }

        stage('User Input') {
            steps {
                // მომხმარებლისგან input-ის მოთხოვნა
                script {
                    def userInput = input(
                        id: 'userInput', 
                        message: 'გსურთ გაგრძელება?', 
                        parameters: [
                            choice(name: 'choice', choices: ['yes', 'no'], description: 'აირჩიეთ yes ან no')
                        ]
                    )
                    if (userInput != 'yes') {
                        error("მომხმარებელმა არჩია 'no'. პაიპლაინი შეჩერებულია.")
                    }
                }
            }
        }

        stage('Parallel Execution') {
            steps {
                parallel(
                    "Curl Google": {
                        script {
                            try {
                                sh 'curl -I https://google.ge'
                            } catch (Exception e) {
                                echo "this url does not work"
                            }
                        }
                    },
                    "Curl Error URL": {
                        script {
                            try {
                                sh 'curl -I https://error.ueczo.com'
                            } catch (Exception e) {
                                echo "this url does not work"
                            }
                        }
                    }
                )
            }
        }
    }

    post {
        always {
            echo "პაიპლაინი წარმატებით დასრულდა."
        }
    }
}

