pipeline {
    agent {
        node {
            label 'Agent01'
        }
    }
    stages{
        stage("Deployment"){
       	    steps {
               withKubeConfig([credentialsId: 'RIAS-K8s-config-file' , serverUrl: 'https://api.riastone.eu:6443', namespace:'smart-manufacturing']) {
                 sh 'kubectl delete -f deployment-uself-agent.yaml && kubectl apply -f deployment-uself-agent.yaml'
                 sh 'kubectl apply -f service-uself-agent.yaml'
                 sh 'kubectl apply -f uself-agent-ingress.yaml'
                 sh 'kubectl delete -f redis-uself-agent.yaml && kubectl apply -f redis-uself-agent.yaml'  
                 sh 'kubectl get pods -n smart-manufacturing' 
                 sh 'kubectl describe pods -n smart-manufacturing -l app=redis-uself-agent' 
                 sh 'kubectl describe ingress -l component=ssi-wallet'
                 sh 'kubectl describe pod -l component=ssi-wallet'
                 sh 'kubectl describe service -l component=ssi-wallet'
               }
 
            }
        }
    
        stage("Show uself-agent pod logs"){
       	        steps {
                   withKubeConfig([credentialsId: 'RIAS-K8s-config-file' , serverUrl: 'https://api.riastone.eu:6443', namespace:'smart-manufacturing']) {
                     sh 'kubectl logs -l app=uself-agent --all-containers --ignore-errors --tail 1000'
                   }
                }
    }
        stage("Show redis-uself-agent pod logs"){
       	        steps {
                   withKubeConfig([credentialsId: 'RIAS-K8s-config-file' , serverUrl: 'https://api.riastone.eu:6443', namespace:'smart-manufacturing']) {
                     sh 'kubectl logs -l app=redis-uself-agent --all-containers --ignore-errors --tail 1000'
                   }
                }
    }

}
}
