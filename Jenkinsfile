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
                 sh 'kubectl apply -f service-uself-agent.yaml'
                 sh 'kubectl apply -f uself-agent-ingress.yaml'
                 sh 'kubectl apply -f redis-uself-agent.yaml'  
                 sh 'kubectl get pods -n ips-testing1' 
                 sh 'kubectl describe pods -n ips-testing1 -l app=redis-uself-agent' 
                 sh 'kubectl describe ingress -l component=ssi-wallet'
                 sh 'kubectl describe pod -l component=ssi-wallet'
                 sh 'kubectl describe service -l component=ssi-wallet'
               }
 
            }
        }
    
        stage("Show uself-agent pod logs"){
       	        steps {
                   withKubeConfig([credentialsId: 'K8s-config-file' , serverUrl: 'https://167.235.66.115:6443', namespace:'ips-testing1']) {
                     sh 'kubectl logs -l app=uself-agent --all-containers --ignore-errors --tail 1000'
                   }
                }
    }
        stage("Show uself-agent pod logs"){
       	        steps {
                   withKubeConfig([credentialsId: 'K8s-config-file' , serverUrl: 'https://167.235.66.115:6443', namespace:'ips-testing1']) {
                     sh 'kubectl logs -l app=redis-uself-agent --all-containers --ignore-errors --tail 1000'
                   }
                }
    }

}
}
