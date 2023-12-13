# kubernetes


01) #Create a new pod called admin-pod with image busybox. Allow it to be able to set system_time. Container should sleep for 3200 seconds.


  15  alias g=kubectl
   17  g run admin-pod --image=busybox --command sleep 3200 --dry-run=client -o yaml | tee admin-pod.yaml
   18  HISTORY


 securityContext :
      add : ["NET_ADMIN", "SYS_TIME"]









4  kubectl run tomcat-pod1 --image=vishymails/tomcatimage:1.0 --labels=app=tomcat-app --dry-run=client 
    5  kubectl run tomcat-pod1 --image=vishymails/tomcatimage:1.0 --labels=app=tomcat-app --dry-run=client -o yaml
    6  kubectl run tomcat-pod1 --image=vishymails/tomcatimage:1.0 --labels=app=tomcat-app
    7  kubectl get po 
    8  kubectl run tomcat-pod2 --image=vishymails/tomcatimage:1.0 --labels=app=tomcat-app
    9  kubectl run tomcat-pod3 --image=vishymails/tomcatimage:1.0 --labels=app=tomcat-app
   10  kubectl get po 
   11  cat > example3.yml
   12  kubectl create -f example3.yml
   13  kubectl get po 
   14  kubectl describe rc tomcat-rc 
   15  kubectl describe rc tomcat-rc -o wide
   16  kubectl describe rc tomcat-rc 
   17  kubectl delete po tomcat-pod1
   18  kubectl get po 
   19  kubectl describe rc tomcat-rc 
   
   
   
   
    21  kubectl delete rc tomcat-rc 
   22  kubectl get po 
   23  kubectl run tomcat-pod1 --image=vishymails/tomcatimage:1.0 --labels=app=tomcat-app
   24  kubectl run tomcat-pod2 --image=centos  --labels=app=tomcat-app
   25  kubectl create -f example3.yml
   26  kubectl delete rc tomcat-rc 
   27  kubectl run tomcat-pod1 --image=vishymails/tomcatimage:1.0 --labels=app=tomcat-app
   28  kubectl run tomcat-pod2 --image=centos  --labels=app=tomcat-app
   29  kubectl create -f example3.yml
   30  kubectl describe rc tomcat-rc 
   31  kubectl get rc 
   32  kubectl get po
   33  kubectl delete pod tomcat-pod2
   34  kubectl get po
   


6) deploy a web-load-5461 pod using nginx:1.17 with the label set to tier=web





terminal 1 


12  kubectl get deploy -o wide 
   13  kubectl set image deploy tomcat-deploy tomcat-containers=nginx:1.9.1 --record 
   14  kubectl rollout status deployment/tomcat-deploy
   15  kubectl get deploy -o wide 
   16  kubectl get rs 
   17  kubectl scale deployment tomcat-deploy --replicas=5
   18  kubectl get deploy -o wide 
   19  kubectl scale deployment tomcat-deploy --replicas=3
   20  kubectl get deploy -o wide 
   21  kubectl set image deploy tomcat-deploy tomcat-containers=tomcat11
   22  kubectl rollout status deployment/tomcat-deploy





terminal 2 

  8  kubectl get deploy
    9  kubectl get rs 
   10  kubectl describe deploy tomcat-deploy
   11  kubectl rollout history deployment/tomcat-deploy
   12  kubectl get deploy -o wide 
   13  kubectl rollout undo  deployment/tomcat-deploy
   14  kubectl get deploy -o wide 


Create a new deployment web-003, scale this deployment to 3 replicas, make sure desired number of pods are always running.



Create a new deployment called web-proj-268 with image nginx:1.16 and one replica. Next, upgrade the deployment to version 1.17 using rolling update. 
Make sure that the version upgrade is recorded in the resource annotation.




34  g get deploy
   35  g delete deploy web-003
   36  g create deployment web-003 --image=nginx --replicas=3 -o yaml | tee deploy1.yaml
   37  g describe deployment web-003
   38  g get pods -A
   39  g logs kube-controller-man -node6 -n kube-system



https://github.com/mmumshad/kubernetes-the-hard-way




apiVersion : apps/v1
kind : Deployment
metadata :
  name : tomcat-deploy
  labels : 
    app : tomcat-app

spec :
  replicas : 4
  selector : 
    matchLabels : 
      app : tomcat-app
  template :
    metadata :
      labels :
        app : tomcat-app
    spec :
      containers :
        - name : tomcat-containers
          image : vishymails/tomcatimage:1.0
          imagePullPolicy : IfNotPresent
          ports :
            - containerPort : 8080






apiVersion : apps/v1
kind : Deployment
metadata :
  name : tomcat-deploy
  labels : 
    app : tomcat-app

spec :
  replicas : 4
  selector : 
    matchLabels : 
      app : tomcat-app
  template :
    metadata :
      labels :
        app : tomcat-app
    spec :
      containers :
        - name : tomcat-containers
          image : vishymails/tomcatimage:1.0
          imagePullPolicy : Always
          ports :
            - containerPort : 8080




apiVersion : apps/v1
kind : Deployment
metadata :
  name : tomcat-deploy
  labels : 
    app : tomcat-app

spec :
  replicas : 4
  selector : 
    matchLabels : 
      app : tomcat-app
  strategy:
    type : Recreate
    
  template :
    metadata :
      labels :
        app : tomcat-app
    spec :
      containers :
        - name : tomcat-containers
          image : vishymails/tomcatimage:1.0
          imagePullPolicy : Always
          ports :
            - containerPort : 8080



  4  ls /etc/kubernetes/manifests
    5  kubectl run static-nginx --image=nginx --dry-run=client -o yaml > static-pod.yaml
    6  ls
    7  cat static-pod.yaml 
    8  kubectl get pods 
    9  kubectl get pods -o wide 
   10  kubectl delete pod static-nginx-node01
   11  kubectl get pods -o wide 




ssh node01


 5  sudo grep static /var/lib/kubelet/config.yaml
    6  ls /etc/kubernetes/manifests
    7  ls
    8  pwd
    9  cd /etc/kubernetes/manifests
   10  pwd
   11  ls
   12  cat > static-pod.yaml





12  cat > example9.1.yaml
   13  cat > example9.2.yaml
   14  kubectl create -f example9.1.yaml 
   15  kubectl get po -o wide 
   16  kubectl get svc
   17  kubectl create -f example9.2.yaml 
   18  kubectl get svc
   19  kubectl describe my-service
   20  kubectl describe svc my-service
   21  curl http://10.5.1.2:8080
   22  curl http://10.5.2.2:8080
   23  curl http://10.5.2.3:8080
   24  kubectl get nodes 
   25  kubectl get nodes -o wide 
   26  curl http://192.168.0.17:31000
   27  curl http://192.168.0.18:31000
   28  history 
   29  cat > example9.3.yaml
   30  kubectl delete svc my-service
   31  kubectl create -f example9.3.yaml
   32  kubectl describe svc my-service
   33  curl http://10.104.83.200:8080




Expose "audit-web-app" pod to by creating a service "audit-web-app-service" on port 30002 on nodes of given cluster.




  4  cat > example11.yaml
    5  kubectl create -f example11.yaml
    6  kubectl get po 
    7  kubectl exec -it tomcat-pod -- /bin/sh
    8  clear
    9  kubectl exec -it tomcat-pod -- /bin/sh
   10  kubectl delete pod tomcat-pod
   11  kubectl create -f example11.yaml
   12  kubectl get po 
   13  kubectl exec -it tomcat-pod -- /bin/sh
   14  kubectl delete pod tomcat-pod	





 1  apt-get update
    2  halt
    3  cat > example12.yaml
    4  kubectl create -f example12.yaml 
    5  kubectl get po 
    6  kubectl exec -it tomcat-hostpath -- /bin/sh
    7  kubectl delete pod tomcat-hostpath
    8  kubectl create -f example12.yaml 
    9  ls
   10  kubectl exec -it tomcat-hostpath -- /bin/sh







apiVersion : v1
kind : Pod
metadata :
  name : multicontainer-pod2

spec :
  containers :
    - name : producer 
      image : ubuntu
      command : ["/bin/bash"]
      args : ["-c", "while true; do echo $(hostname) $(date) >> /var/log/index.html; sleep 10; done"]
      volumeMounts:
        - name:  webcontent
          mountPath: /var/log

    - name : consumer
      image : nginx
      ports :
        - containerPort : 80
      volumeMounts:
        - name:  webcontent
          mountPath: /usr/share/nginx/html


  volumes :
    - name : webcontent 
      emptyDir : {}

      






5  kubectl create -f example15.yaml
    6  kubectl get po
    7  kubectl exec -it multicontainer-pod2 -- /bin/sh
    8  kubectl exec -it --container=producer multicontainer-pod2 -- /bin/sh 







Create a pod called pod-multi with 2 containers as it is descripted below:
   Container 1 : name:container1, image: nginx 
   Container 2 : name:container2, image: busybox, command: sleep 4800
   


Create a persistent volume with given specifications:
  Volume Name - pv-rnd
  storage - 100Mi
  Access modes - ReadWriteMany
  host path - /pv/host-data-rnd




Craete a PersistentVolume, PersistentVolumeClaim and Pod with below specifications

  PV - name : mypvl , Size: 100Mi, AccessModes: ReadWritemany, Hostpath: /pv/log, Reclaim Policy: Retain
  PVC - name:  pv-claim-l, Storage request: 50Mi, Access Modes: ReadWritemany 
  Pod - name : my-nginx-pod, image Name: nginx, Volume: PersistentVolumeClaim: pv-claim-l, volume mount : /log
  



Create a replicaset (name : web-replica, image=nginx, replicas=3), there is already a pod running in our cluster.  
 Please make sure that total count of pods running in the cluster is not more than 3.



Create a new deployment called nginx-deployment with an image nginx:1.16 and 5 replicas. There are 2 worker nodes in our cluster. 
 Please make sure no pod will get deployed on node7.
 


apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      nodeSelector:
        kubernetes.io/hostname:        # Label the nodes appropriately, e.g., node7 for node 7
          notin: [node7]          # Exclude node7 from deployment
      containers:
      - name: nginx
        image: nginx:1.16

  


We have worker 3 nodes in our cluster, create a DaemonSet (name prod-pod, image=nginx) on each node 
