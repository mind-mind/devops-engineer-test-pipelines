pipeline {
    agent any

    stages {
        stage('Clone Repo A') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/mind-mind/grpc.git'
            }
        }

        stage('Generate Doxygen HTML and warnings log') {
            steps {
                sh '''
                    doxygen -g Doxyfile
                    sed -i 's|^INPUT .*|INPUT = src|' Doxyfile
                    sed -i 's|^RECURSIVE .*|RECURSIVE = YES|' Doxyfile
                    sed -i 's|^GENERATE_HTML .*|GENERATE_HTML = YES|' Doxyfile
                    sed -i 's|^GENERATE_LATEX .*|GENERATE_LATEX = NO|' Doxyfile
                    sed -i 's|^GENERATE_XML .*|GENERATE_XML = NO|' Doxyfile
                    sed -i 's|^GENERATE_MAN .*|GENERATE_MAN = NO|' Doxyfile
                    sed -i 's|^GENERATE_RTF .*|GENERATE_RTF = NO|' Doxyfile
                    sed -i 's|^WARN_LOGFILE .*|WARN_LOGFILE = doxygen_warnings.log|' Doxyfile

                    : > doxygen_warnings.log
                    doxygen Doxyfile
                    tar -czf doc.tar.gz html
                '''
            }
        }

        stage('Clone Repo C and parse warnings') {
            steps {
                dir('doxygen-warning-parser') {
                    git branch: 'main',
                        url: 'https://github.com/mind-mind/doxygen-warning-parser.git'
                    sh 'python3 parser.py ../doxygen_warnings.log ../doxygen_report.csv'
                }
            }
        }

        stage('Archive Documentation and warning report') {
            steps {
                archiveArtifacts artifacts: 'doc.tar.gz,doxygen_report.csv', fingerprint: true
            }
        }
    }
}
