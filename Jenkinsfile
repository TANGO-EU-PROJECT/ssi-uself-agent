pipeline {
    agent {
        node {
            label 'Agent01'
        }
    }
    stages{
        stage("Deployment"){
       	    steps {
               withKubeConfig([credentialsId: 'Metro-K8s-config-file' , serverUrl: 'https://213.249.10.150:6443', namespace:'retail']) {
                 sh 'kubectl delete -f deployment-uself-agent.yaml && kubectl apply -f deployment-uself-agent.yaml'
                 sh 'kubectl apply -f service-uself-agent.yaml'
                 sh 'kubectl apply -f uself-agent-ingress.yaml'
                 sh 'kubectl apply -f redis-uself-agent.yaml'  
                 sh 'kubectl get pods -n retail' 
                 sh 'kubectl describe pods -n retail -l app=redis-uself-agent' 
                 sh 'kubectl describe ingress -l component=ssi-wallet'
                 sh 'kubectl describe pod -l component=ssi-wallet'
                 sh 'kubectl describe service -l component=ssi-wallet'
               }
 
            }
        }
    
        stage("Show uself-agent pod logs"){
       	        steps {
                   withKubeConfig([credentialsId: 'Metro-K8s-config-file' , serverUrl: 'https://213.249.10.150:6443', namespace:'retail']) {
                     sh 'kubectl logs -l app=uself-agent --all-containers --ignore-errors --tail 1000'
                   }
                }
    }
        stage("Show redis-uself-agent pod logs"){
       	        steps {
                   withKubeConfig([credentialsId: 'Metro-K8s-config-file' , serverUrl: 'https://213.249.10.150:6443', namespace:'retail']) {
                     sh 'kubectl logs -l app=redis-uself-agent --all-containers --ignore-errors --tail 1000'
                   }
                }
    }
        stage("Show uself-agent pod logs before deployment "){
       	        steps {
                   withKubeConfig([credentialsId: 'Metro-K8s-config-file' , serverUrl: 'https://213.249.10.150:6443', namespace:'retail']) {
                     sh 'kubectl get pods -n retail'
                     sh 'timeout 120s kubectl logs -n retail -f -l app=uself-agent --all-containers || true'
                     sh 'kubectl exec -n retail $(kubectl get pods -n retail -l app=uself-agent -o jsonpath='{.items[0].metadata.name}') -- ls -la /app/data'  
                   }
                }
    }        

}
}
