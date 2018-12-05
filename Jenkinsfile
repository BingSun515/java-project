properties([pipelineTriggers([githubPush()])])

node('linux') {
        stage('Unit Test') {
		git 'https://github.com/bingsun515/java-project.git'
		sh 'ant -f test.xml -v'
		junit 'reports/result.xml'
        }
        stage('Build') {
            sh 'ant -f build.xml -v'
        }
        stage('Deploy') {
            sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://seis665-bing'
        }
		 
	stage('Report') {
	   withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '7ddf09e6-424b-4dbb-aba8-0f74f9ca1df6', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
         sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'}
       }
}
