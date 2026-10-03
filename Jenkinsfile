pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"
    }

    stages {
        stage('Clone Repo A') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/mind-mind/grpc.git'
            }
        }

        stage('Generate Doxygen HTML') {
            steps {
                sh '''
                    doxygen -g Doxyfile
                    sed -i '' 's|^INPUT .*|INPUT = src|' Doxyfile
                    sed -i '' 's|^RECURSIVE .*|RECURSIVE = YES|' Doxyfile
                    sed -i '' 's|^GENERATE_HTML .*|GENERATE_HTML = YES|' Doxyfile
                    sed -i '' 's|^GENERATE_LATEX .*|GENERATE_LATEX = NO|' Doxyfile
                    sed -i '' 's|^GENERATE_XML .*|GENERATE_XML = NO|' Doxyfile
                    sed -i '' 's|^GENERATE_MAN .*|GENERATE_MAN = NO|' Doxyfile
                    sed -i '' 's|^GENERATE_RTF .*|GENERATE_RTF = NO|' Doxyfile

                    doxygen Doxyfile
                    tar -czf doc.tar.gz html
                '''
            }
        }

        stage('Archive Documentation') {
            steps {
                archiveArtifacts artifacts: 'doc.tar.gz', fingerprint: true
            }
        }

    }
}