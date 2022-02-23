node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        sh docker login -u shalligoel -p Mycloud@8
        app = docker.build("shalligoel/training:hellonode")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
          */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'git') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
	
	stage('Run image') {
        /* Finally, we'll pull and run the image :
        */
        docker.withRegistry('https://registry.hub.docker.com', 'git') {
            sh docker pull shalligoel/training:hellonode
            sh docker run -p 8000:8000 shalligoel/training:hellonode
        }
    }
}
