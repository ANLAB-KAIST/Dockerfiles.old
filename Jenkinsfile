pipeline {
    agent none
    stages {
        stage ("Test Dockerfile") {
            parallel {
                stage ("texlive-full") {
                    agent {
                        dockerfile {
                            dir "texlive-full"
                        }
                    }
                    steps {
                        sh "latexmk --help"
                    }
                }
            }
        }
    }
}