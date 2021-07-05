node {
    
    stage 'Checkout'
    git "https://github.com/spring-projects/spring-petclinic.git"

    stage 'Build application war file'
    // Build petclinic in a Maven3+JDK8 Docker container
    docker.image('maven:3-jdk-8').inside('-v /.m2:/root/.m2') {
        sh 'mvn -B package -DskipTests'
    }
    
    stage 'Build application Docker image'
    def appImg = docker.build("local/petclinic")
}
