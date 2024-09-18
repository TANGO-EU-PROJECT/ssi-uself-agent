pipeline {
    agent {
        node {
            label 'Agent01'
        }
    }
    stages{
        stage("Deployment"){
       	    steps {
               withKubeConfig([credentialsId: 'K8s-config-file' , serverUrl: 'https://167.235.66.115:6443', namespace:'tango-development']) {
                 sh 'kubectl apply -f deployment-uself-agent.yaml'
                 sh 'kubectl apply -f uself-agent-ingress.yaml'
                 sh 'kubectl get pods'
                 sh 'kubectl get pods -n ips-testing1' 
                 sh 'kubectl describe deployment uself-agent -n ips-testing1'  
                 sh 'sleep 120' 
                 sh 'kubectl describe deployment uself-agent -n ips-testing1'  
                 sh 'kubectl logs -l app=uself-agent -n ips-testing1'
               }
 
            }
        }
    }
}
