node {
    stage("Determine Steps") {
        git 'https://github.com/maddinenisri/springboot-vertx.git'
    }
    stage("Build") {
        withEnv([
            "JAVA_HOME=${tool 'jdk1.8'}",
            "PATH+MAVEN=${tool 'maven-3.5.0'}/bin:${env.JAVA_HOME}/bin"
        ]) {
            sh "mvn clean package"
        }
    }
    stage("Promote Build") {

    }
    stage("Deploy to Dev") {
        withCredentials([usernamePassword(credentialsId: 'e383ee23-4270-40c5-ac9d-a4620e18e2cf', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh '''
            export AWS_ACCESS_KEY_ID=$USERNAME
            export AWS_SECRET_ACCESS_KEY=$PASSWORD
            export ANSIBLE_HOSTS=inventory/ec2.py
            export EC2_INI_PATH=inventory/ec2.ini
            export ANSIBLE_HOST_KEY_CHECKING=False
            chmod +x inventory/ec2.py
            inventory/ec2.py --list
            ansible-playbook -i inventory/ec2.py deploy_bounce.yml --limit tag_Environment_dev
            '''
        }
    }
}
