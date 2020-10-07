pipeline{
	agent { label 'master'}
  stages{
    stage('SCM'){
	    steps{
	    git branch: 'main',
	        url: 'https://github.com/javahometech/multibranch'
	    }
    }
  }
}
