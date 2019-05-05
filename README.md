* Kubectl Kubernetes CheatSheet                                   :Cloud:
:PROPERTIES:
:type:     kubernetes
:END:

** Common Commands
| Name                                 | Command                                                                          |
|--------------------------------------+----------------------------------------------------------------------------------|
| Run curl test temporarily            | =kubectl run --rm mytest --image=yauritux/busybox-curl -it=                      |
| Run wget test temporarily            | =kubectl run --rm mytest --image=busybox -it=                                    |
| Run nginx deployment with 2 replicas | =kubectl run my-nginx --image=nginx --replicas=2 --port=80=                      |
| Set namespace preference             | =kubectl config set-context $(kubectl config current-context) --namespace=<ns1>= |
| List pods with nodes info            | =kubectl get pod -o wide=                                                        |
| List everything                      | =kubectl get all --all-namespaces=                                               |
| Get all services                     | =kubectl get service --all-namespaces=                                           |
| Show nodes with labels               | =kubectl get nodes --show-labels=                                                |
| Validate yaml file with dry run      | =kubectl create --dry-run --validate -f pod-dummy.yaml=                          |
| Start a temporary pod for testing    | =kubectl run --rm -i -t --image=alpine test-$RANDOM -- sh=                       |
| kubectl run shell command            | =kubectl exec -it mytest -- ls -l /etc/hosts=                                    |
| Get system conf via configmap        | =kubectl -n kube-system get cm kubeadm-config -o yaml=                           |
| Get deployment yaml                  | =kubectl -n denny-websites get deployment mysql -o yaml=                         |
| Explain resource                     | =kubectl explain pods=, =kubectl explain svc=                                    |
| Watch pods                           | =kubectl get pods  -n wordpress --watch=                                         |
| Query healthcheck endpoint           | =curl -L http://127.0.0.1:10250/healthz=                                         |
| Open a bash terminal in a pod        | =kubectl exec -it storage sh=                                                    |
| Check pod environment variables      | =kubectl exec redis-master-ft9ex env=                                            |
| Enable kubectl shell autocompletion  | =echo "source <(kubectl completion bash)" >>~/.bashrc=, and reload               |
| Use minikube dockerd in your laptop  | =eval $(minikube docker-env)=, No need to push docker hub any more               |
| Kubectl apply a folder of yaml files | =kubectl apply -R -f .=                                                          |
| Get services sorted by name          | =kubectl get services --sort-by=.metadata.name                                    |
| Get pods sorted by restart count     | =kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'           |
| List all container images            | =kubectl get pods --all-namespaces -o jsonpath="{..image}" | tr -s '[[:space:]]' '\n'| sort | uniq -d |

** Check Performance
| Name                                         | Command                                              |
|----------------------------------------------+------------------------------------------------------|
| Get node resource usage                      | =kubectl top node=                                   |
| Get pod resource usage                       | =kubectl top pod=                                    |
| Get resource usage for a given pod           | =kubectl top <podname> --containers=                 |
| List resource utilization for all containers | =kubectl top pod --all-namespaces --containers=true= |
** Resources Deletion
| Name                                    | Command                                                  |
|-----------------------------------------+----------------------------------------------------------|
| Delete pod                              | =kubectl delete pod/<pod-name> -n <my-namespace>=        |
| Delete pod by force                     | =kubectl delete pod/<pod-name> --grace-period=0 --force= |
| Delete pods by labels                   | =kubectl delete pod -l env=test=                         |
| Delete deployments by labels            | =kubectl delete deployment -l app=wordpress=             |
| Delete all resources filtered by labels | =kubectl delete pods,services -l name=myLabel=           |
| Delete resources under a namespace      | =kubectl -n my-ns delete po,svc --all=                   |
| Delete persist volumes by labels        | =kubectl delete pvc -l app=wordpress=                    |
| Delete statefulset only (not pods)      | =kubectl delete sts/<stateful_set_name> --cascade=false= |

** Log & Conf Files
| Name                      | Comment                                                                            |
|---------------------------+------------------------------------------------------------------------------------|
| Config folder             | =/etc/kubernetes/=                                                                 |
| Certificate files         | =/etc/kubernetes/pki/=                                                             |
| Credentials to API server | =/etc/kubernetes/kubelet.conf=                                                     |
| Superuser credentials     | =/etc/kubernetes/admin.conf=                                                       |
| kubectl config file       | =~/.kube/config=                                                                   |
| Kubernets working dir     | =/var/lib/kubelet/=                                                                |
| Docker working dir        | =/var/lib/docker/=, =/var/log/containers/=                                         |
| Etcd working dir          | =/var/lib/etcd/=                                                                   |
| Network cni               | =/etc/cni/net.d/=                                                                  |
| Log files                 | =/var/log/pods/=                                                                   |
| log in master node        | =/var/log/kube-apiserver.log=, =kube-scheduler.log=, =kube-controller-manager.log= |
| log in worker node        | =/var/log/kubelet.log=, =kubelet-proxy.log=                                        |
| Env                       | =/etc/systemd/system/kubelet.service.d/10-kubeadm.conf=                            |
| Env                       | export KUBECONFIG=/etc/kubernetes/admin.conf                                       |

** Pod
| Name                             | Command                                                                                                                       |
|----------------------------------+-------------------------------------------------------------------------------------------------------------------------------|
| List all pods                    | =kubectl get pods=                                                                                                            |
| List pods for all namespace      | =kubectl get pods -all-namespaces=                                                                                            |
| List all critical pods           | =kubectl get -n kube-system pods -a=                                                                                          |
| List pods with more info         | =kubectl get pod -o wide=, =kubectl get pod/<pod-name> -o yaml=                                                               |
| Get pod info                     | =kubectl describe pod/srv-mysql-server=                                                                                       |
| List all pods with labels        | =kubectl get pods --show-labels=                                                                                              |
| List running pods                | kubectl get pods --field-selector=status.phase=Running                                                                        |
| Get Pod initContainer status     | =kubectl get pod --template '{{.status.initContainerStatuses}}' <pod-name>=                                                   |
| kubectl run command              | kubectl exec -it -n "$ns" "$podname" -- sh -c "echo $msg >>/dev/err.log"                                                      |
| Watch pods                       | =kubectl get pods  -n wordpress --watch=                                                                                      |
| Get pod by selector              | podname=$(kubectl get pods -n $namespace --selector="app=syslog" -o jsonpath='{.items[*].metadata.name}')                     |
| List pods and containers         | kubectl get pods -o='custom-columns=PODS:.metadata.name,CONTAINERS:.spec.containers[*].name'                                  |
| List pods, containers and images | kubectl get pods -o='custom-columns=PODS:.metadata.name,CONTAINERS:.spec.containers[*].name,Images:.spec.containers[*].image' |
| Kubernetes Yaml Examples         | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]]                                                                                               |

** Label & Annontation
| Name                             | Command                                                           |
|----------------------------------+-------------------------------------------------------------------|
| Filter pods by label             | =kubectl get pods -l owner=denny=                                 |
| Manually add label to a pod      | =kubectl label pods dummy-input owner=denny=                      |
| Remove label                     | =kubectl label pods dummy-input owner-=                           |
| Manually add annonation to a pod | =kubectl annotate pods dummy-input my-url=https://dennyzhang.com= |

** Deployment & Scale
| Name                         | Command                                                                  |
|------------------------------+--------------------------------------------------------------------------|
| Scale out                    | =kubectl scale --replicas=3 deployment/nginx-app=                        |
| online rolling upgrade       | =kubectl rollout app-v1 app-v2 --image=img:v2=                           |
| Roll backup                  | =kubectl rollout app-v1 app-v2 --rollback=                               |
| List rollout                 | =kubectl get rs=                                                         |
| Check update status          | =kubectl rollout status deployment/nginx-app=                            |
| Check update history         | =kubectl rollout history deployment/nginx-app=                           |
| Pause/Resume                 | =kubectl rollout pause deployment/nginx-deployment=, =resume=            |
| Rollback to previous version | =kubectl rollout undo deployment/nginx-deployment=                       |
| Kubernetes Yaml Examples     | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]], [[https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#pausing-and-resuming-a-deployment][Link: Pausing and Resuming a Deployment]] |

** Quota & Limits & Resource
| Name                          | Command                                                                          |
|-------------------------------+----------------------------------------------------------------------------------|
| Customize resource definition | =kubectl set resources deployment nginx -c=nginx --limits=cpu=200m,memory=512Mi= |
| List Resource Quota           | =kubectl get resourcequota=                                                      |
| List Limit Range              | =kubectl get limitrange=                                                         |
| Customize resource definition | =kubectl set resources deployment nginx -c=nginx --limits=cpu=200m,memory=512Mi= |
| Kubernetes Yaml Examples      | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]]                                                  |

** Service
| Name                            | Command                                                                           |
|---------------------------------+-----------------------------------------------------------------------------------|
| List all services               | =kubectl get services=                                                            |
| List service endpoints          | =kubectl get endpoints=                                                           |
| Get service detail              | =kubectl get service nginx-service -o yaml=                                       |
| Get service cluster ip          | kubectl get service nginx-service -o go-template='{{.spec.clusterIP}}'            |
| Get service cluster port        | kubectl get service nginx-service -o go-template='{{(index .spec.ports 0).port}}' |
| Expose deployment as lb service | =kubectl expose deployment/my-app --type=LoadBalancer --name=my-service=          |
| Expose service as lb service    | =kubectl expose service/wordpress-1-svc --type=LoadBalancer --name=wordpress-lb=  |
| Kubernetes Yaml Examples        | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]]                                                   |

** Secrets
| Name                        | Command                                                                 |
|-----------------------------+-------------------------------------------------------------------------|
| List secrets                | =kubectl get secrets --all-namespaces=                                  |
| Create secret from cfg file | =kubectl create secret generic db-user-pass --from-file=./username.txt= |
| Generate secret             | =echo -n 'mypasswd'=, then redirect to =base64 -decode=                 |
| Kubernetes Yaml Examples    | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]]                                         |

** StatefulSet
| Name                               | Command                                                  |
|------------------------------------+----------------------------------------------------------|
| List statefulset                   | =kubectl get sts=                                        |
| Delete statefulset only (not pods) | =kubectl delete sts/<stateful_set_name> --cascade=false= |
| Scale statefulset                  | =kubectl scale sts/<stateful_set_name> --replicas=5=     |
| Kubernetes Yaml Examples           | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]]                          |

** Volumes & Volume Claims
| Name                      | Command                                                      |
|---------------------------+--------------------------------------------------------------|
| List storage class        | =kubectl get storageclass=                                   |
| Check the mounted volumes | =kubectl exec storage ls /data=                              |
| Check persist volume      | =kubectl describe pv/pv0001=                                 |
| Copy local file to pod    | =kubectl cp /tmp/my <some-namespace>/<some-pod>:/tmp/server= |
| Copy pod file to local    | =kubectl cp <some-namespace>/<some-pod>:/tmp/server /tmp/my= |
| Kubernetes Yaml Examples  | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]]                              |

** Events & Metrics
| Name                            | Command                                                    |
|---------------------------------+------------------------------------------------------------|
| View all events                 | =kubectl get events --all-namespaces=                      |
| List Events sorted by timestamp | kubectl get events --sort-by=.metadata.creationTimestamp   |

** Node Maintenance
| Name                                      | Command                       |
|-------------------------------------------+-------------------------------|
| Mark node as unschedulable                | =kubectl cordon $NDOE_NAME=   |
| Mark node as schedulable                  | =kubectl uncordon $NDOE_NAME= |
| Drain node in preparation for maintenance | =kubectl drain $NODE_NAME=    |

** Namespace & Security
| Name                          | Command                                                                         |
|-------------------------------+---------------------------------------------------------------------------------|
| List authenticated contexts   | =kubectl config get-contexts=, =~/.kube/config=                                 |
| Load context from config file | =kubectl get cs --kubeconfig kube_config.yml=                                   |
| Switch context                | =kubectl config use-context <cluster-name>=                                     |
| Delete the specified context  | =kubectl config delete-context <cluster-name>=                                  |
| List all namespaces defined   | =kubectl get namespaces=                                                        |
| Set namespace preference      | =kubectl config set-context $(kubectl config current-context) --namespace=<ns1>= |
| List certificates             | =kubectl get csr=                                                               |
| Kubernetes Yaml Examples      | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]]                                                 |

** Network
| Name                              | Command                                                  |
|-----------------------------------+----------------------------------------------------------|
| Temporarily add a port-forwarding | =kubectl port-forward redis-izl09 6379=                  |
| Add port-forwaring for deployment | =kubectl port-forward deployment/redis-master 6379:6379= |
| Add port-forwaring for replicaset | =kubectl port-forward rs/redis-master 6379:6379=         |
| Add port-forwaring for service    | =kubectl port-forward svc/redis-master 6379:6379=        |
| Get network policy                | =kubectl get NetworkPolicy=                              |

** Patch
| Name                          | Summary                                                                                  |
|-------------------------------+------------------------------------------------------------------------------------------|
| Patch service to loadbalancer | =kubectl patch svc "$APP_INSTANCE_NAME-grafana" -p '{"spec": {"type": "LoadBalancer"}}'= |

** Extenstions
| Name                         | Summary                    |
|------------------------------+----------------------------|
| List api group               | =kubectl api-versions=     |
| List all CRD                 | =kubectl get crd=          |
| List storageclass            | =kubectl get storageclass= |
| List all supported resources | =kubectl api-resources=    |



https://kubernetes.io/docs/reference/kubectl/cheatsheet/

https://codefresh.io/kubernetes-guides/kubernetes-cheat-sheet/

#+BEGIN_HTML

#+STARTUP: overview customtime noalign logdone showall
#+DESCRIPTION:
#+KEYWORDS:
#+LATEX_HEADER: \usepackage[margin=0.6in]{geometry}
#+LaTeX_CLASS_OPTIONS: [8pt]
#+LATEX_HEADER: \usepackage[english]{babel}
#+LATEX_HEADER: \usepackage{lastpage}
#+LATEX_HEADER: \usepackage{fancyhdr}
#+LATEX_HEADER: \pagestyle{fancy}
#+LATEX_HEADER: \fancyhf{}
#+LATEX_HEADER: \rhead{Updated: \today}
#+LATEX_HEADER: \rfoot{\thepage\ of \pageref{LastPage}}
#+LATEX_HEADER: \lfoot{\href{https://github.com/dennyzhang/cheatsheet-kubernetes-A4}{GitHub: https://github.com/dennyzhang/cheatsheet-kubernetes-A4}}
#+LATEX_HEADER: \lhead{\href{https://cheatsheet.dennyzhang.com/cheatsheet-slack-A4}{Blog URL: https://cheatsheet.dennyzhang.com/cheatsheet-kubernetes-A4}}

  CLOSED: [2018-11-17 Sat 12:23]
- Tail pod log by label
#+BEGIN_SRC sh
namespace="mynamespace"
mylabel="app=mylabel"
kubectl get pod -l "$mylabel" -n "$namespace" | tail -n1 \
    | awk -F' ' '{print $1}' | xargs -I{} \
      kubectl logs -n "$namespace" -f {}
#+END_SRC

- Get node hardware resource utilization
#+BEGIN_SRC sh
kubectl get nodes --no-headers \
     | awk '{print $1}' | xargs -I {} \
     sh -c 'echo {}; kubectl describe node {} | grep Allocated -A 5'

kubectl get nodes --no-headers | awk '{print $1}' | xargs -I {} \
    sh -c 'echo {}; kubectl describe node {} | grep Allocated -A 5 \
     | grep -ve Event -ve Allocated -ve percent -ve -- ; echo'
#+END_SRC

- Apply the configuration in manifest.yaml and delete all the other configmaps that are not in the file.

#+BEGIN_EXAMPLE
kaubectl apply --prune -f manifest.yaml --all --prune-whitelist=core/v1/ConfigMap
#+END_EXAMPLE
* [#A] Kubernets                                         :IMPORTANT:
https://github.com/dennyzhang/cheatsheet-kubernetes-A4

k8s provides declarative primitives for the "desired state"
- Self-healing
- Horizontal scaling
- Automatic binpacking
- Service discovery and load balancing
** Names of certificates files
https://github.com/kubernetes/kubeadm/blob/master/docs/design/design_v1.9.md
Names of certificates files:
ca.crt, ca.key (CA certificate)
apiserver.crt, apiserver.key (API server certificate)
apiserver-kubelet-client.crt, apiserver-kubelet-client.key (client certificate for the apiservers to connect to the kubelets securely)
sa.pub, sa.key (a private key for signing ServiceAccount )
front-proxy-ca.crt, front-proxy-ca.key (CA for the front proxy)
front-proxy-client.crt, front-proxy-client.key (client cert for the front proxy client)
** TODO update k8s cheatsheet github: https://github.com/alex1x/kubernetes-cheatsheet
** TODO Setting up MySQL Replication Clusters in Kubernetes: https://blog.kublr.com/setting-up-mysql-replication-clusters-in-kubernetes-ab7cbac113a5
** TODO MySQL on Docker: Running Galera Cluster on Kubernetes
https://severalnines.com/blog/mysql-docker-running-galera-cluster-kubernetes
** TODO Try Functions as a Service - a serverless framework for Docker & Kubernetes http://docs.get-faas.com/
https://blog.alexellis.io/first-faas-python-function/
** TODO [#A] k8s clustering elasticsearch
https://blog.alexellis.io/kubernetes-kubeadm-video/
** TODO k8s scale with redis
** TODO k8s scale with mysqld
** TODO [#A] k8s: https://5pi.de/2016/11/20/15-producation-grade-kubernetes-cluster/
** TODO Try kops with k8s
** TODO k8s free course: https://classroom.udacity.com/courses/ud615
** TODO feedbackup for k8s study project
Aaron Mulholland [1:18 AM]
So it looks pretty good. Got some good concepts in early on. Couple of suggestions for further work;

Potentially the following scenarios;
    * Setting up ingresses and TLS
              * Fully configure something like Nginx Ingress Controller or Traefik.
              * Create TLS Secrets within Kubernetes, and use them in your ingress controller.
    * Managing RBAC  (Don't know enough about this one, but sounds like a good concept to include)
              * Creating new roles, etc

I'll have a think and if anymore come to me, I'll let you know.


Denny Zhang (Github . Blogger)
[1:19 AM]
:thumbsup:

Will update per your suggestions tomorrow, Aaron
** TODO k8s add DNS chanllenges
Gui [4:01 PM]
Getting familiar with the concepts like pod, service, RC, deployment, etc.


[4:02]
Try volume


[4:02]
DNS.


Denny Zhang (Github . Blogger)
[4:02 PM]
I'm trying to cover the volume via mysql scenarios


Gui [4:02 PM]
And other addons
1 reply Today at 4:03 PM View thread


Denny Zhang (Github . Blogger)
[4:02 PM]
For DNS, not sure whether I get your point


Gui [4:03 PM]
I haven't tried a lot myself.
1 reply Today at 4:03 PM View thread


[4:03]
Like every pod and service has an DNS name to talk to each other.


Denny Zhang (Github . Blogger) [4:04 PM]
Yes, that makes sense


[4:04]
For addons, do you have any recommended scenario?
** TODO k8s add challenge of addon
https://www.cncf.io

https://kubernetes.io/docs/concepts/cluster-administration/addons/
** TODO k8s networking models
** TODO k8s example: https://github.com/kubernetes/examples
** TODO Blog: Wordpress powered by k8s, docker swarm


** TODO [#A] absord: https://github.com/kubecamp/kubernetes_in_one_day
** TODO [#A] absord: https://github.com/kubecamp/kubernetes_in_2_days
** DONE kubectl config view
   CLOSED: [2017-12-31 Sun 10:40]
** DONE [#A] kubernetes persistent volume claim pending
  CLOSED: [2017-12-31 Sun 11:32]
https://github.com/openshift/origin/issues/7170

kubectl get pvc
kubectl get pv

#+BEGIN_EXAMPLE
ubuntu@k8s1:~$ kubectl describe pvc
Name:          ironic-gerbil-jenkins
Namespace:     default
StorageClass:
Status:        Pending
Volume:
Labels:        app=ironic-gerbil-jenkins
               chart=jenkins-0.10.2
               heritage=Tiller
               release=ironic-gerbil
Annotations:   <none>
Capacity:
Access Modes:
Events:
  Type    Reason         Age                 From                         Message
  ----    ------         ----                ----                         -------
  Normal  FailedBinding  37s (x261 over 2h)  persistentvolume-controller  no persistent volumes available for this claim and no storage class is set


Name:          my-mysql-mysql
Namespace:     default
StorageClass:
Status:        Pending
Volume:
Labels:        app=my-mysql-mysql
               chart=mysql-0.3.2
               heritage=Tiller
               release=my-mysql
Annotations:   <none>
Capacity:
Access Modes:
Events:
  Type    Reason         Age              From                         Message
  ----    ------         ----             ----                         -------
  Normal  FailedBinding  7s (x5 over 1m)  persistentvolume-controller  no persistent volumes available for this claim and no storage class is set
#+END_EXAMPLE
** DONE kubernetes start a container for testing: kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il
   CLOSED: [2017-12-31 Sun 11:26]
** DONE [#A] ReplicaSet is the next-generation Replication Controller.
  CLOSED: [2017-12-04 Mon 11:26]
The only difference between a ReplicaSet and a Replication Controller right now is the selector support.

https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

https://github.com/arun-gupta/oreilly-kubernetes-book/blob/master/ch01/wildfly-replicaset.yml
Next generation Replication Controller

Set-based selector requirement
- Expression: key, operator, value
- Operators: In, NotIn, Exists, DoesNotExist

▪Generally created with Deployment
▪Enables Horizontal Pod Autoscaling
** DONE k8s yaml API version: https://kubernetes.io/docs/reference/federation/extensions/v1beta1/definitions/
   CLOSED: [2017-12-03 Sun 12:50]
** DONE k8s cronjob
  CLOSED: [2018-01-03 Wed 12:26]
https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

kubectl create -f ./cronjob.yaml
kubectl get cronjob hello
kubectl get jobs --watch
kubectl delete cronjob hello

#+BEGIN_EXAMPLE
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            args:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
#+END_EXAMPLE
** DONE [#B] check k8s status: kubectl get cs
   CLOSED: [2018-01-03 Wed 11:57]
** BYPASS crictl not found in system path: warning
   CLOSED: [2018-01-03 Wed 12:36]
** DONE kubernetes default service type: ClusterIP
   CLOSED: [2018-01-02 Tue 11:07]
** DONE kubectl get nodes: Unable to connect to the server: x509: certificate signed by unknown authority: incorrect /etc/kubernetes/admin.conf
  CLOSED: [2018-01-04 Thu 00:09]


root@k8s1:~# kubectl get nodes
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")
root@k8s1:~# echo $KUBECONFIG

root@k8s1:~# export KUBECONFIG=/etc/kubernetes/admin.conf
root@k8s1:~# kubectl get nodes
NAME      STATUS     ROLES     AGE       VERSION
k8s1      Ready      master    29m       v1.9.0
k8s2      NotReady   <none>    17m       v1.9.0
** DONE [#A] kubernetes-the-hard-way: https://github.com/kelseyhightower/kubernetes-the-hard-way
   CLOSED: [2017-12-04 Mon 15:49]
*** CANCELED k8s hardway: etcdctl: Error:  context deadline exceeded
  CLOSED: [2017-12-04 Mon 17:54]
https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/e8d728d0162ebcdf951464caa8be3a5b156eb463/docs/07-bootstrapping-etcd.md
#+BEGIN_EXAMPLE
mac@controller-0:~$ ETCDCTL_API=3 etcdctl member list
Error:  context deadline exceeded
#+END_EXAMPLE

#+BEGIN_EXAMPLE
mac@controller-0:~$ kubectl get componentstatuses
NAME                 STATUS      MESSAGE                                                                                          ERROR
etcd-2               Unhealthy   Get https://10.240.0.12:2379/health: dial tcp 10.240.0.12:2379: getsockopt: connection refused
controller-manager   Healthy     ok
etcd-1               Unhealthy   Get https://10.240.0.11:2379/health: dial tcp 10.240.0.11:2379: getsockopt: connection refused
scheduler            Healthy     ok
etcd-0               Unhealthy   Get https://10.240.0.10:2379/health: net/http: TLS handshake timeout
#+END_EXAMPLE
** DONE k8s livenessProbe(when to restart a Container), readinessProbe(when is ready to accept requests)
  CLOSED: [2018-01-08 Mon 07:41]
https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/
http://kubernetesbyexample.com/healthz/
https://kubernetes-v1-4.github.io/docs/user-guide/liveness/
https://github.com/arun-gupta/kubernetes-java-sample/blob/master/wildfly-pod-hc-http.yaml
http://kubernetesbyexample.com/healthz/

Probes have a number of fields that you can use to more precisely control the behavior of liveness and readiness checks:

initialDelaySeconds: Number of seconds after the container has started before liveness or readiness probes are initiated.
periodSeconds: How often (in seconds) to perform the probe. Default to 10 seconds. Minimum value is 1.
timeoutSeconds: Number of seconds after which the probe times out. Defaults to 1 second. Minimum value is 1.
successThreshold: Minimum consecutive successes for the probe to be considered successful after having failed. Defaults to 1. Must be 1 for liveness. Minimum value is 1.
failureThreshold: When a Pod starts and the probe fails, Kubernetes will try failureThreshold times before giving up. Giving up in case of liveness probe means restarting the Pod. In case of readiness probe the Pod will be marked Unready. Defaults to 3. Minimum value is 1.

#+BEGIN_EXAMPLE
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - echo ok > /tmp/health; sleep 10; rm -rf /tmp/health; sleep 600
    image: gcr.io/google_containers/busybox
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/health
      initialDelaySeconds: 15
      timeoutSeconds: 1
    name: liveness
#+END_EXAMPLE
** DONE list all critical pods
  CLOSED: [2018-01-04 Thu 10:10]
kubectl --namespace kube-system get pods

for pod in $(kubectl --namespace kube-system get pods -o jsonpath="{.items[*].metadata.name}"); do
    node_info=$(kubectl --namespace kube-system describe pod $pod | grep "Node:")
    echo "Pod: $pod, $node_info"
done
** DONE k8s cheatsheet: kube-shell https://github.com/cloudnativelabs/kube-shell
   CLOSED: [2017-12-31 Sun 10:47]
** DONE k8s configmap
  CLOSED: [2018-01-08 Mon 10:32]
https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/
| Name                                                | Summary |
|-----------------------------------------------------+---------|
| kubectl get configmaps my-wordpress-mariadb -o yaml |         |
** DONE [#A] k8s initContainers debug: kubectl logs <pod-name> -c <init-container-2>
  CLOSED: [2018-01-05 Fri 16:29]
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-init-containers/
** DONE Use GCE to setup k8s cluster deployment
  CLOSED: [2018-01-07 Sun 07:26]
https://github.com/kelseyhightower/kubernetes-the-hard-way

https://cloud.google.com/
source /Users/mac/Downloads/google-cloud-sdk/completion.bash.inc
source /Users/mac/Downloads/google-cloud-sdk/path.bash.inc
*** doc: gcloud setup
#+BEGIN_EXAMPLE
   [28] us-central1-f
   [29] us-central1-c
   [30] us-central1-b
   [31] us-east1-d
   [32] us-east1-c
   [33] us-east1-b
   [34] us-east4-c
   [35] us-east4-a
   [36] us-east4-b
   [37] us-west1-a
   [38] us-west1-c
   [39] us-west1-b
   [40] Do not set default zone
  Please enter numeric choice or text value (must exactly match list
  item):  36

  Your project default Compute Engine zone has been set to [us-east4-b].
  You can change it by running [gcloud config set compute/zone NAME].

  Your project default Compute Engine region has been set to [us-east4].
  You can change it by running [gcloud config set compute/region NAME].

  Created a default .boto configuration file at [/Users/mac/.boto]. See this file and
  [https://cloud.google.com/storage/docs/gsutil/commands/config] for more
  information about configuring Google Cloud Storage.
  Your Google Cloud SDK is configured and ready to use!

  * Commands that require authentication will use denny.zhang001@gmail.com by default
  * Commands will reference project `denny-k8s-test1` by default
  * Compute Engine commands will use region `us-east4` by default
  * Compute Engine commands will use zone `us-east4-b` by default

  Run `gcloud help config` to learn how to change individual settings

  This gcloud configuration is called [default]. You can create additional configurations if you work with multiple accounts and/or projects.
  Run `gcloud topic configurations` to learn more.

  Some things to try next:

  * Run `gcloud --help` to see the Cloud Platform services you can interact with. And run `gcloud help COMMAND` to get help on any gcloud command.
  * Run `gcloud topic -h` to learn about advanced features of the SDK like arg files and output formatting
#+END_EXAMPLE
*** TODO [#A] can't find gcloud                                   :IMPORTANT:
source /Users/mac/Downloads/google-cloud-sdk/completion.bash.inc
source /Users/mac/Downloads/google-cloud-sdk/path.bash.inc
** DONE kubectl get pod
   CLOSED: [2018-04-28 Sat 09:28]
 /etc/kubernetes/admin.conf /etc/kubernetes/kubelet.conf /etc/kubernetes/bootstrap-kubelet.conf /etc/kubernetes/controller-manager.conf /etc/kubernetes/scheduler.conf]

#+BEGIN_EXAMPLE
 Your Kubernetes master has initialized successfully!

 To start using your cluster, you need to run the following as a regular user:

   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config

 You should now deploy a pod network to the cluster.
 Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
   https://kubernetes.io/docs/concepts/cluster-administration/addons/
#+END_EXAMPLE
** DONE pod CrashLoopBackOff: starting, then crashing, then starting again and crashing again.

   CLOSED: [2018-01-05 Fri 15:47]
 https://www.krenger.ch/blog/crashloopbackoff-and-how-to-fix-it/

 https://kubernetes.io/docs/tasks/debug-application-cluster/debug-init-containers/

| Status                     | Meaning                                                     |
|----------------------------+-------------------------------------------------------------|
| Init:N/M                   | The Pod has M Init Containers, and N have completed so far. |
| Init:Error                 | An Init Container has failed to execute.                    |
| Init:CrashLoopBackOff      | An Init Container has failed repeatedly.                    |
| Pending                    | The Pod has not yet begun executing Init Containers.        |
| PodInitializing or Running | The Pod has already finished executing Init Containers.     |
** DONE k8s ImagePullBackOff: describe pod $pod_name; No space
   CLOSED: [2018-06-25 Mon 14:28]
** DONE default pods for single node installation
   CLOSED: [2018-04-28 Sat 08:49]
#+BEGIN_EXAMPLE
 root@mdm-k8s-node2:~# docker ps
 CONTAINER ID        IMAGE                                                                                                              COMMAND                  CREATED             STATUS              PORTS               NAMES
 75d08dd2b171        k8s.gcr.io/kube-proxy-amd64@sha256:c7036a8796fd20c16cb3b1cef803a8e980598bff499084c29f3c759bdb429cd2                "/usr/local/bin/ku..."   16 hours ago        Up 16 hours                             k8s_kube-proxy_kube-proxy-jmcs9_kube-system_02a0eac8-4a75-11e8-afce-7aa5a78d07bd_0
 0a769558ec4f        k8s.gcr.io/pause-amd64:3.1                                                                                         "/pause"                 16 hours ago        Up 16 hours                             k8s_POD_kube-proxy-jmcs9_kube-system_02a0eac8-4a75-11e8-afce-7aa5a78d07bd_0
 2af1fbfd581a        k8s.gcr.io/kube-apiserver-amd64@sha256:1ba863c8e9b9edc6d1329ebf966e4aa308ca31b42a937b4430caf65aa11bdd12            "kube-apiserver --..."   16 hours ago        Up 16 hours                             k8s_kube-apiserver_kube-apiserver-mdm-k8s-node2_kube-system_fee65b809c1e455cf1672ebe7efc4bc7_0
 63c214ac8d1b        k8s.gcr.io/kube-controller-manager-amd64@sha256:922ac89166ea228cdeff43e4c445a5dc4204972cc0e265a8762beec07b6238bf   "kube-controller-m..."   16 hours ago        Up 16 hours                             k8s_kube-controller-manager_kube-controller-manager-mdm-k8s-node2_kube-system_5ad7a10c5a8589117db7258c7d499a33_0
 324ff1a8d357        k8s.gcr.io/kube-scheduler-amd64@sha256:5f50a339f66037f44223e2b4607a24888177da6203a7bc6c8554e0f09bd2b644            "kube-scheduler --..."   16 hours ago        Up 16 hours                             k8s_kube-scheduler_kube-scheduler-mdm-k8s-node2_kube-system_aa8d5cab3ea096315de0c2003230d4f9_0
 dce77d944669        k8s.gcr.io/etcd-amd64@sha256:68235934469f3bc58917bcf7018bf0d3b72129e6303b0bef28186d96b2259317                      "etcd --listen-cli..."   16 hours ago        Up 16 hours                             k8s_etcd_etcd-mdm-k8s-node2_kube-system_59f847fe34319ab1263f0b3ee03df8a3_0
 2af621e52e11        k8s.gcr.io/pause-amd64:3.1                                                                                         "/pause"                 16 hours ago        Up 16 hours                             k8s_POD_kube-apiserver-mdm-k8s-node2_kube-system_fee65b809c1e455cf1672ebe7efc4bc7_0
 bdc64588b27d        k8s.gcr.io/pause-amd64:3.1                                                                                         "/pause"                 16 hours ago        Up 16 hours                             k8s_POD_kube-controller-manager-mdm-k8s-node2_kube-system_5ad7a10c5a8589117db7258c7d499a33_0
 14dd26427abf        k8s.gcr.io/pause-amd64:3.1                                                                                         "/pause"                 16 hours ago        Up 16 hours                             k8s_POD_kube-scheduler-mdm-k8s-node2_kube-system_aa8d5cab3ea096315de0c2003230d4f9_0
 17bfbb8af205        k8s.gcr.io/pause-amd64:3.1                                                                                         "/pause"                 16 hours ago        Up 16 hours                             k8s_POD_etcd-mdm-k8s-node2_kube-system_59f847fe34319ab1263f0b3ee03df8a3_0
#+END_EXAMPLE
** DONE One pod may have multiple containers
   CLOSED: [2018-06-19 Tue 14:31]
 If a pod has more than 1 containers then you need to provide the name of the specific container.
** DONE kubectl edit deployment parameters
   CLOSED: [2018-04-15 Sun 21:49]
 https://github.com/kubernetes/helm/issues/2464
 kubectl -n kube-system patch deployment tiller-deploy -p '{"spec": {"template": {"spec": {"automountServiceAccountToken": true}}}}'

 kubectl --namespace=kube-system edit deployment/tiller-deploy and changed automountServiceAccountToken to true.
** DONE [#A] k8s sidecar
   CLOSED: [2018-07-15 Sun 22:50]
 https://k8s.io/examples/admin/logging/two-files-counter-pod-streaming-sidecar.yaml
#+BEGIN_EXAMPLE
 apiVersion: v1
 kind: Pod
 metadata:
   name: counter
 spec:
   containers:
   - name: count
     image: busybox
     args:
     - /bin/sh
     - -c
     - >
       i=0;
       while true;
       do
         echo "$i: $(date)" >> /var/log/1.log;
         echo "$(date) INFO $i" >> /var/log/2.log;
         i=$((i+1));
         sleep 1;
       done
     volumeMounts:
     - name: varlog
       mountPath: /var/log
   - name: count-log-1
     image: busybox
     args: [/bin/sh, -c, 'tail -n+1 -f /var/log/1.log']
     volumeMounts:
     - name: varlog
       mountPath: /var/log
   - name: count-log-2
     image: busybox
     args: [/bin/sh, -c, 'tail -n+1 -f /var/log/2.log']
     volumeMounts:
     - name: varlog
       mountPath: /var/log
   volumes:
   - name: varlog
     emptyDir: {}
#+END_EXAMPLE
** TODO [#A] k8s debug why termination takes time
** TODO Kubernets availablity
*** TODO Building High-Availability Clusters: https://kubernetes.io/docs/admin/high-availability/
** TODO [#A] Blog: Kubernetes Service Type: NodePort, ClusterIP and Loadbalancer?
#+BEGIN_EXAMPLE
https://kubernetes.io/docs/concepts/services-networking/service/

Publishing services - service types
For some parts of your application (e.g. frontends) you may want to expose a Service onto an external (outside of your cluster) IP address.

Kubernetes ServiceTypes allow you to specify what kind of service you want. The default is ClusterIP.

Type values and their behaviors are:

ClusterIP: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster. This is the default ServiceType.
NodePort: Exposes the service on each Node's IP at a static port (the NodePort). A ClusterIP service, to which the NodePort service will route, is automatically created. You'll be able to contact the NodePort service, from outside the cluster, by requesting <NodeIP>:<NodePort>.
LoadBalancer: Exposes the service externally using a cloud provider's load balancer. NodePort and ClusterIP services, to which the external load balancer will route, are automatically created.
ExternalName: Maps the service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up. This requires version 1.7 or higher of kube-dns.
#+END_EXAMPLE
*** Type: Loadbalancer
*** Type: ClusterIP
*** Type: NodePort
If you set the type field to "NodePort", the Kubernetes master will allocate a port from a flag-configured range (default: 30000-32767)

*** TODO Now if i access IP:NodePort, will it balance the load across multiple pods ?
https://kubernetes.io/docs/tasks/access-application-cluster/load-balance-access-application-cluster/
#+BEGIN_EXAMPLE
Vivek Yadav [8:34 AM]
Hey Denny, quick question -

```
---
 apiVersion: v1
 kind: Service
 metadata:
   name: span
   labels:
     app: span
 spec:
   type: NodePort
   ports:
     - port: 80
       nodePort: 30080
   selector:
     app: spa

---
 apiVersion: apps/v1beta2
 kind: Deployment
 metadata:
   name: spa
 spec:
   replicas: 2
   selector:
     matchLabels:
       app: spa
   template:
     metadata:
       labels:
         app: spa
     spec:
       containers:
         - name: py
           image: viveky4d4v/local-simple-python:latest
           ports:
             - containerPort: 8080
         - name: nginx
           image: viveky4d4v/local-nginx-lb:latest
           ports:
             - containerPort: 80
       imagePullSecrets:
         - name: regsecret

```


Now if i access IP:NodePort, will it balance the load across multiple pods ?


Denny Zhang (Github . Blogger) [8:35 AM]
I don't think so
#+END_EXAMPLE
*** TODO How Does NodePort work behind the scene?
*** #  --8<-------------------------- separator ------------------------>8-- :noexport:
*** TODO How Loadbalancer is implemented in code?
*** #  --8<-------------------------- separator ------------------------>8-- :noexport:
*** TODO Does Loadbalancer works only for public cloud?
*** TODO How I configure Ingress?
** TODO [#A] NodePort VS clusterIP                                 :IMPORTANT:
https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types
http://weezer.su/kubernetes-1.html
https://docs.openshift.com/container-platform/3.3/dev_guide/getting_traffic_into_cluster.html

clusterIP: You can only access this service while inside the cluster.
** TODO [#A] k8s feature watch list
*** I want to check pod initContainer logs, but I don't want to specify initContainer by name
#+BEGIN_EXAMPLE
macs-MacBook-Pro:Scenario-401 mac$ kubectl logs my-jenkins-jenkins-89889ddb7-ct7jw -c 1
Error from server (BadRequest): container 1 is not valid for pod my-jenkins-jenkins-89889ddb7-ct7jw
macs-MacBook-Pro:Scenario-401 mac$ kubectl logs my-jenkins-jenkins-89889ddb7-ct7jw -c  copy-default-config
Error from server (BadRequest): container "copy-default-config" in pod "my-jenkins-jenkins-89889ddb7-ct7jw" is waiting to start: PodInitializing
macs-MacBook-Pro:Scenario-401 mac$ kubectl logs my-jenkins-jenkins-89889ddb7-ct7jw -c  copy-default-config
Error from server (BadRequest): container "copy-default-config" in pod "my-jenkins-jenkins-89889ddb7-ct7jw" is waiting to start: PodInitializing
#+END_EXAMPLE
*** Support using environment variables inside deployment yaml file
https://github.com/kubernetes/kubernetes/issues/52787
** TODO pod error: CreateContainerConfigError
https://github.com/kubernetes/minikube/issues/2256
#+BEGIN_EXAMPLE
bash-3.2$ kubectl get pod my-wordpress-wordpress-df987548d-btvf5
NAME                                     READY     STATUS                       RESTARTS   AGE
my-wordpress-wordpress-df987548d-btvf5   0/1       CreateContainerConfigError   0          2m
bash-3.2$
#+END_EXAMPLE

#+BEGIN_EXAMPLE
bash-3.2$ kubectl describe pod/my-wordpress-wordpress-df987548d-btvf5
Name:           my-wordpress-wordpress-df987548d-btvf5
Namespace:      default
Node:           minikube/192.168.99.102
Start Time:     Fri, 05 Jan 2018 16:41:27 -0600
Labels:         app=my-wordpress-wordpress
                pod-template-hash=895431048
Annotations:    kubernetes.io/created-by={"kind":"SerializedReference","apiVersion":"v1","reference":{"kind":"ReplicaSet","namespace":"default","name":"my-wordpress-wordpress-df987548d","uid":"910e01e0-f269-11e7-b6d8...
Status:         Pending
IP:             172.17.0.6
Created By:     ReplicaSet/my-wordpress-wordpress-df987548d
Controlled By:  ReplicaSet/my-wordpress-wordpress-df987548d
Containers:
  my-wordpress-wordpress:
    Container ID:
    Image:          bitnami/wordpress:4.9.1-r1
    Image ID:
    Ports:          80/TCP, 443/TCP
    State:          Waiting
      Reason:       CreateContainerConfigError
    Ready:          False
    Restart Count:  0
    Requests:
      cpu:      300m
      memory:   512Mi
    Liveness:   http-get http://:http/wp-login.php delay=120s timeout=5s period=10s #success=1 #failure=6
    Readiness:  http-get http://:http/wp-login.php delay=30s timeout=3s period=5s #success=1 #failure=3
    Environment:
      ALLOW_EMPTY_PASSWORD:         yes
      MARIADB_ROOT_PASSWORD:        <set to the key 'mariadb-root-password' in secret 'my-wordpress-mariadb'>  Optional: false
      MARIADB_HOST:                 my-wordpress-mariadb
      MARIADB_PORT_NUMBER:          3306
      WORDPRESS_DATABASE_NAME:      bitnami_wordpress
      WORDPRESS_DATABASE_USER:      bn_wordpress
      WORDPRESS_DATABASE_PASSWORD:  <set to the key 'mariadb-password' in secret 'my-wordpress-mariadb'>  Optional: false
      WORDPRESS_USERNAME:           admin
      WORDPRESS_PASSWORD:           <set to the key 'wordpress-password' in secret 'my-wordpress-wordpress'>  Optional: false
      WORDPRESS_EMAIL:              contact@dennyzhang.com
      WORDPRESS_FIRST_NAME:         FirstName
      WORDPRESS_LAST_NAME:          LastName
      WORDPRESS_BLOG_NAME:          My DevOps Blog!
      SMTP_HOST:
      SMTP_PORT:
      SMTP_USER:
      SMTP_PASSWORD:                <set to the key 'smtp-password' in secret 'my-wordpress-wordpress'>  Optional: false
      SMTP_USERNAME:
      SMTP_PROTOCOL:
    Mounts:
      /bitnami/apache from wordpress-data (rw)
      /bitnami/php from wordpress-data (rw)
      /bitnami/wordpress from wordpress-data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-tc8kd (ro)
Conditions:
  Type           Status
  Initialized    True
  Ready          False
  PodScheduled   True
Volumes:
  wordpress-data:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  my-wordpress-wordpress
    ReadOnly:   false
  default-token-tc8kd:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-tc8kd
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     <none>
Events:
  Type     Reason                 Age              From               Message
  ----     ------                 ----             ----               -------
  Normal   Scheduled              1m               default-scheduler  Successfully assigned my-wordpress-wordpress-df987548d-btvf5 to minikube
  Normal   SuccessfulMountVolume  1m               kubelet, minikube  MountVolume.SetUp succeeded for volume "pvc-910644d3-f269-11e7-b6d8-08002782d6cd"
  Normal   SuccessfulMountVolume  1m               kubelet, minikube  MountVolume.SetUp succeeded for volume "default-token-tc8kd"
  Normal   Pulled                 1s (x7 over 1m)  kubelet, minikube  Container image "bitnami/wordpress:4.9.1-r1" already present on machine
  Warning  Failed                 1s (x7 over 1m)  kubelet, minikube  Error: lstat /tmp/hostpath-provisioner/pvc-910644d3-f269-11e7-b6d8-08002782d6cd: no such file or directory
  Warning  FailedSync             1s (x7 over 1m)  kubelet, minikube  Error syncing pod
bash-3.2$
#+END_EXAMPLE
** TODO [#A] Certified Kubernetes Administrator (CKA)              :IMPORTANT:
https://www.cncf.io/certification/expert/

https://github.com/cncf/curriculum/blob/master/certified_kubernetes_administrator_exam_v1.8.0.pdf

It is an online, proctored, performance-based test that requires solving multiple issues from a command line.

Candidates have 3 hours to complete the tasks.
** HALF Difference in between selectors and labels
** TODO [#A] kubernetes mount a file to pod                        :IMPORTANT:
https://stackoverflow.com/questions/33415913/whats-the-best-way-to-share-mount-one-file-into-a-pod
https://www.linkedin.com/feed/update/urn:li:activity:6355445509146107904/
** TODO K8S label & Selector
https://github.com/dennyzhang/dennytest/tree/master/cheatsheet-kubernetes-A4][challenges-leetcode-interesting]]
* [#A] k8s metric server                                 :noexport:IMPORTANT:
Metrics Server is a cluster-wide aggregator of resource usage data.

Metrics Server registered in the main API server through Kubernetes aggregator.

https://github.com/kubernetes-incubator/metrics-server
https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B

https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/
| Name           | Summary                                                           |
|----------------+-------------------------------------------------------------------|
| Core metrics   | node/container level metrics; CPU, memory, disk and network, etc. |
| Custom metrics | refers to application metrics, e.g. HTTP request rate.            |

Today (Kubernetes 1.7), there are several sources of metrics within a Kubernetes cluster
| Name           | Summary                                                             |
|----------------+---------------------------------------------------------------------|
| Heapster       | k8s add-on                                                          |
| Cadvisor       | a standalone container/node metrics collection and monitoring tool. |
| Kubernetes API | does not track metrics. But can get real time metrics               |
** metric server
Resource Metrics API is an effort to provide a first-class Kubernetes API (stable, versioned, discoverable, available through apiserver and with client support) that serves resource usage metrics for pods and nodes.

- metric server is sort of a stripped-down version of Heapster
- The metrics-server will collect "Core" metrics from cAdvisor APIs (currently embedded in the kubelet) and store them in memory as opposed to in etcd.
- The metrics-server will provide a supported API for feeding schedulers and horizontal pod auto-scalers
- All other Kubernetes components will supply their own metrics in a Prometheus format
** Cadvisor
Cadvisor monitors node and container core metrics in addition to container events.
It natively provides a Prometheus metrics endpoint
The Kubernetes kublet has an embedded Cadvisor that only exposes the metrics, not the events.
** heapster
Heapster is an add on to Kubernetes that collects and forwards both node, namespace, pod and container level metrics to one or more "sinks" (e.g. InfluxDB).

It also provides REST endpoints to gather those metrics. The metrics are constrained to CPU, filesystem, memory, network and uptime.

Heapster queries the kubelet for its data.

Today, heapster is the source of the time-series data for the Kubernetes Dashboard.
** #  --8<-------------------------- separator ------------------------>8-- :noexport:
** TODO How to query metric server
** TODO Key scenarios of metric server
The metrics-server will provide a much needed official API for the internal components of Kubernetes to make decisions about the utilization and performance of the cluster.

- HPA(Horizontal Pod Autoscaler) need input to do good auto-scaling
** TODO There are plans for an "Infrastore", a Kubernetes component that keeps historical data and events
** #  --8<-------------------------- separator ------------------------>8-- :noexport:
** TODO why from heapster to k8s metric server?
** TODO kube-aggregator
** TODO what is promethues format?
#+BEGIN_EXAMPLE
Denny Zhang [12:34 AM]
An easy introduction about k8s metric server. (It will replace heapster)

https://blog.freshtracks.io/what-is-the-the-new-kubernetes-metrics-server-849c16aa01f4

> All other Kubernetes components will supply their own metrics in a Prometheus format

In logging domain, we can say `syslog` is the standard format

In metric domain, maybe we can choose `prometheus` as the standard format.
#+END_EXAMPLE
** Metrics Use Cases
https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/resource-metrics-api.md

https://docs.giantswarm.io/guides/kubernetes-heapster/

#+BEGIN_EXAMPLE
Horizontal Pod Autoscaler: It scales pods automatically based on CPU or custom metrics (not explained here). More information here.
Kubectl top: The command top of our beloved Kubernetes CLI display metrics directly in the terminal.
Kubernetes dashboard: See Pod and Nodes metrics integrated into the main Kubernetes UI dashboard. More info here
Scheduler: In the future Core Metrics will be considered in order to schedule best-effort Pods.
#+END_EXAMPLE
** useful link
https://blog.freshtracks.io/what-is-the-the-new-kubernetes-metrics-server-849c16aa01f4
https://blog.outlyer.com/monitoring-kubernetes-with-heapster-and-prometheus
https://www.outcoldman.com/en/archive/2017/07/09/kubernetes-monitoring-resources/
* k8s loadbalancer                                                 :noexport:
** DONE k8s service: loadbalancer
   CLOSED: [2018-06-19 Tue 13:51]
#+BEGIN_EXAMPLE
 cat > service.yml <<EOF
 apiVersion: v1
 kind: Service
 metadata:
   name: lb
   namespace: logging
 spec:
   selector:
     app: kibana
   ports:
   - protocol: TCP
     port: 5601
   type: LoadBalancer
 EOF
#+END_EXAMPLE
* k8s DaemonSet                                                    :noexport:
** DONE k8s daemonsets: ensures that all (or some) Nodes run a copy of a Pod.
   CLOSED: [2018-06-19 Tue 13:28]
 https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

 As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

 Some typical uses of a DaemonSet are:

 - running a cluster storage daemon, such as glusterd, ceph, on each node.
 - running a logs collection daemon on every node, such as fluentd or logstash.
   - running a node monitoring daemon on every node, such as Prometheus Node Exporter, collectd, Datadog agent, New Relic agent, or Ganglia gmond.
* [#A] etcd                                                        :noexport:
https://coreos.com/etcd/docs/latest/dev-guide/interacting_v3.html
https://coreos.com/etcd/docs/latest/v2/README.html
* [#B] k8s addons                                                  :noexport:
https://kubernetes.io/docs/concepts/cluster-administration/addons/
** DONE k8s install add-on: dashboard
  CLOSED: [2018-01-03 Wed 12:19]
- Install, then use kubectl-proxy to start
- Create user and binding, then use token to login

#+BEGIN_EXAMPLE
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
nohup kubectl proxy --port=8001 --address=0.0.0.0 &

curl http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

#+END_EXAMPLE

#+BEGIN_EXAMPLE
# https://github.com/kubernetes/dashboard/wiki/Creating-sample-user
cat > user.yaml <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
EOF
#+END_EXAMPLE

kubectl apply -f user.yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

https://github.com/kubernetes/dashboard#kubernetes-dashboard
https://blog.frognew.com/2017/09/kubeadm-install-kubernetes-1.8.html#8dashboard%E6%8F%92%E4%BB%B6%E9%83%A8%E7%BD%B2
*** DONE kubectl proxy listen on all network nics
  CLOSED: [2018-01-03 Wed 12:12]
https://github.com/kubernetes/kubectl/issues/142
kubectl proxy --port=8001 --address=0.0.0.0
* [#A] k8s volumes                                                 :noexport:
  CLOSED: [2017-12-01 Fri 22:45]
https://kubernetes.io/docs/concepts/storage/volumes
https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes

https://blog.couchbase.com/stateful-containers-kubernetes-amazon-ebs/
https://stackoverflow.com/questions/37555281/create-kubernetes-pod-with-volume-using-kubectl-run
https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/

▪Directory accessible to the containers in a pod
▪Volume outlives any containers in a pod
▪Common types
   hostPath
   nfs
   awsElasticBlockStore
   gcePersistentDisk

#+BEGIN_EXAMPLE
Creating and using a persistent volume is a three step process:
1. Provision: Administrator provision a networked storage in the cluster, such as AWS ElasticBlockStore volumes. This is called as PersistentVolume.
2. Request storage: User requests storage for pods by using claims. Claims can specify levels of resources (CPU and memory), specific sizes and access modes (e.g. can be mounted once read/write or many times write only).
This is called as PersistentVolumeClaim.
1. Use claim: Claims are mounted as volumes and used in pods for storage.
#+END_EXAMPLE
** DONE persistence.accessMode ReadWriteOnce or ReadOnly: https://github.com/kubernetes/charts/tree/master/cheatsheet-kubernetes-A4][challenges-leetcode-interesting]]
  CLOSED: [2018-01-02 Tue 16:52]
The access modes are:

ReadWriteOnce - the volume can be mounted as read-write by a single node
ReadOnlyMany - the volume can be mounted read-only by many nodes
ReadWriteMany - the volume can be mounted as read-write by many nodes
* [#B] k8s security: secrets, authentication & authorization       :noexport:
** what's service account: In contrast, service accounts are users managed by the Kubernetes API.
https://kubernetes.io/docs/admin/authentication/
https://github.com/kubernetes/kubernetes/blob/master/examples/elasticsearch/service-account.yaml
https://kubernetes.io/docs/admin/authorization/
** serviceaccount, clusterrolebinding
https://blog.frognew.com/2017/12/its-time-to-use-helm.html
#+BEGIN_EXAMPLE
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
#+END_EXAMPLE
** k8s secrets: intended to hold sensitive information, such as passwords, OAuth tokens, and ssh keys.
https://github.com/arun-gupta/vault-kubernetes/blob/master/secrets.yaml
http://kubernetesbyexample.com/secrets/

- Secrets are namespaced objects, that is, exist in the context of a namespace
- You can access them via a volume or an environment variable from a container running in a pod
- The secret data on nodes is stored in tmpfs volumes

kubectl create secret generic mysecret --from-literal=mysql_root_password=my-secret-pw
kubectl get secret mysecret

#+BEGIN_EXAMPLE
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
  - name: mycontainer
    image: redis
    env:
      - name: SECRET_USERNAME
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: username
      - name: SECRET_PASSWORD
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: password
  restartPolicy: Never
#+END_EXAMPLE
* HPA: Horizontal Pod Autoscaler                                   :noexport:
* Uncertainty & Uncomfortable things with K8S                      :noexport:
** Destroy namepsace takes more than 15 minutes, with nowhere to check
Testing in minikube
** Pod stucks in containercreating for a long time
* HALF kubectl apply to a list of folder: kubectl apply -R -f namespace-drain-manifests/manifests :noexport:
* GKE user access                                                  :noexport:
#+BEGIN_EXAMPLE
If y'all run into the following error: `is forbidden: attempt to grant extra privileges:` when trying to run `kubectl apply -R -f ~/workspace/namespace-drain/manifests/` against a GKE cluster, then run the following command.

```kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user $(gcloud config get-value account)```
#+END_EXAMPLE
* Blog: How Enterprise Do XXX in Container world?                  :noexport:
* TODO [#A] Blog: interview candidates for k8s experience          :noexport:
** Explain concepts
*** What's k8s context. Why we need it?
*** What's initContainer? Why we need it?
*** Network policy
** Comparision
*** configmap vs secrets
*** labels vs anonations
What are k8s Annotations? What differences it is compared with labels:

- Like labels, annotations are key/value pairs. Where labels have length limits, annotations can be quite large.
-  you can't query or select objects based on annotations.
- Are used for non-identifying information. Stuff not used internally by k8s.

https://codeengineered.com/blog/2017/kubernetes-labels-annotations/
https://vsupalov.com/kubernetes-labels-annotations-difference/ (edited)
*** clusterip, service, loadbalancer
*** ClusterRole vs Role
*** serviceaccount vs useraccount
** Scenarios/Experience
*** tell me about k8s security model
*** tell me about k8s scheduling model
*** tell me about k8s HA model
*** tell me about k8s trouble shooting experience
** Your Wish List
*** layer of yaml
*** ABBA on volumes
*** apply one configmap to all namespace
* k8s workflow: every 3 months has one new release                 :noexport:
https://github.com/kubernetes/kubeadm/blob/master/docs/release-cycle.md
* Blog: Kubernetes Limitation List                                 :noexport:
- Starting with Kubernetes 1.6 we support 5000 nodes clusters with 30 pods per node. ([[https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/metrics-server.md#scalability-limitations][link]])
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* DONE Why we need Static Pods                                     :noexport:
  CLOSED: [2019-01-04 Fri 15:04]
https://kubernetes.io/docs/tasks/administer-cluster/static-pod/
Denny Zhang [2:26 PM]
Fan, ever heard of `Static Pods` in k8s?

If yes, could you give me two use scenarios why I would use it.

Fan Zhang [3:00 PM]
我听说过
其实就是kubelet直接管理的pod

Denny Zhang [3:01 PM]
是的,文档是这么说的.

Fan Zhang [3:01 PM]
我觉得这个是DeamonSet的补充

Denny Zhang [3:01 PM]
我在尝试理解这个背后的应用场景

Fan Zhang [3:02 PM]
因为有时候在node上需要有一些particular的service,但又不希望被kubernetes的schecular 管理

Denny Zhang [3:02 PM]
将OS的进程容器化
但这些只是OS级别,而不是k8s系统或app应用级别的进程
可以这样理解吗？

Fan Zhang [3:03 PM]
否则 drain之后 就没有了
可以这样理解

Denny Zhang [3:04 PM]
所以drain node不会把static pod删掉？
* TODO Why need kubernetes/apiserver: https://github.com/kubernetes/apiserver :noexport:
Library for writing a Kubernetes-style API server.

https://github.com/kubernetes/kube-aggregator
* TODO [#A] Questions                                              :noexport:
** pod type
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#my-service-is-missing-endpoints
#+BEGIN_EXAMPLE
...
spec:
  - selector:
     name: nginx
     type: frontend
#+END_EXAMPLE

kubectl get pods --selector=name=nginx,type=frontend
** Containers inside a Pod can communicate with one another using localhost.
https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/

Networking
Each Pod is assigned a unique IP address. Every container in a Pod shares the network namespace, including the IP address and network ports. Containers inside a Pod can communicate with one another using localhost. When containers in a Pod communicate with entities outside the Pod, they must coordinate how they use the shared network resources (such as ports).
** How to restart a container inside a Pod?
https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/

Restarting a container in a Pod should not be confused with restarting the Pod. The Pod itself does not run, but is an environment the containers run in and persists until it is deleted.
** explain k8s components: apiserver, scheduler, controller-manager, kube-proxy
** get logs of failed container
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#my-pod-is-crashing-or-otherwise-unhealthy
#+BEGIN_EXAMPLE
If your container has previously crashed, you can access the previous container's crash log with:

$ kubectl logs --previous ${POD_NAME} ${CONTAINER_NAME}
#+END_EXAMPLE
** Why k8s dashboard get deprecated?
https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
* TODO k8s architecture                                            :noexport:
https://www.youtube.com/watch?v=_WfJz5VS_cU&list=PLj6h78yzYM2NGwRwkBPxigKio2r0XHPl9
* TODO k8s scenario problems                                       :noexport:
** TODO export k8s dashboard: kube proxy VS ingress
** TODO how to back and restore etcd
https://kubernetes-incubator.github.io/kube-aws/advanced-topics/etcd-backup-and-restore.html
* TODO Apply yamls file recursively                                :noexport:
#+BEGIN_SRC sh
# create
time ls -1 */*.yml | grep -v namespace | xargs -I{} kubectl apply -f {}

# delete
time ls -1r */*.yml | grep -v namespace | xargs -I{} kubectl delete -f {}
#+END_SRC
* TODO devstats: https://k8s.devstats.cncf.io/d/12/dashboards?refresh=15m&orgId=1 :noexport:
* TODO create a ingress service for clusterip service              :noexport:
* TODO kubectl -vvv                                                :noexport:
* TODO kubectl get application --all-namespaces                    :noexport:
* TODO kubectl delete namespace in GKE is extremely slow           :noexport:
* TODO try more with ReplicaSet                                    :noexport:
* TODO try PodDisruptionBudget: https://hackernoon.com/top-10-kubernetes-tips-and-tricks-27528c2d0222 :noexport:
* TODO [#A] k8s services                                           :noexport:
https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
* [#A] ClusterIP                                                   :noexport:
** TODO kubernetes clusterip
** TODO Is k8s ClusterIP SPOF?
 https://mp.weixin.qq.com/s?__biz=MzIzNjUxMzk2NQ==&mid=2247486025&idx=1&sn=1f95917918a3217bb92b97113c81b6c8&chksm=e8d7f58bdfa07c9dedbfbe4f39687ea5d467ec371ecb2dea5dd13101a46d3bb754d6738e481f&scene=27#wechat_redirect
** TODO Use ExternalName to avoid ClusterIP SPOF
* TODO k8s cpu 88m?                                                :noexport:
#+BEGIN_EXAMPLE
    Limits:
      cpu:	48m
      memory:	104Mi
    Requests:
      cpu:		48m
      memory:		104Mi

#+END_EXAMPLE
* TODO autoscaling pod: try auto scaling                           :noexport:
* TODO k8s volume: readwriteonce, readwritemany?                   :noexport:
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO grant more privileges to a given serviceaccount             :noexport:
kubectl get serviceaccount --all-namespaces

prometheus-1-prometheusserviceaccount-e1fd

system:kubelet-api-admin
* TODO Question: PodDisruptionBudget: https://docs.pivotal.io/runtimes/pks/1-2/troubleshoot-issues.html#upgrade-drain-hangs :noexport:
If Kubernetes is unable to unschedule a pod, then the drain hangs indefinitely. 

One reason why Kubernetes may be unable to unschedule the node is if
the PodDisruptionBudget object has been configured in a way that
allows 0 disruptions and only a single instance of the pod has been
scheduled.
* TODO k8s events                                                  :noexport:
https://solinea.com/blog/tapping-kubernetes-events
* TODO kubectl from worker vm, I don't seem to need a kubeconfig   :noexport:
* TODO kubectl apply -f -                                          :noexport:
* TODO How does "kubectl delete - f -" works?                      :noexport:
* TODO devstats: https://k8s.devstats.cncf.io/d/12/dashboards?refresh=15m&orgId=1 :noexport:
* TODO Is it possible to assign a DNS address to Kubernetes service :noexport:
* TODO k8s template templateinstance                               :noexport:
* TODO [#A] k8s yaml create a loadbalancer                         :noexport:
* TODO github improvememnt: update k8s cheatsheet: https://blog.billyc.io/notes/kubectl-notes/ :noexport:
https://kubernetes.io/docs/reference/kubectl/cheatsheet/
* [#A] Google Kubernetes                                 :noexport:IMPORTANT:
No.2 Kubernetes

Kubernetes是一个编排（orchestration）工具,类似运行于Apache Mesos之上的Marathon,但是它是专门为Docker容器而创建的.

Kubernetes is an open-source platform for automating deployment, scaling, and operations of application containers across clusters of hosts, providing container-centric infrastructure

Kubernetes来自Google,除了能在他们自己的Google Container Engine上工作之外,还支持VMware vSphere, Mesos, or Mesosphere DCOS,以及很多公有云,包括Amazon Web Services等.

Kubernetes 具备完善的集群管理能力,包括多层次的安全防护和准入机制`多租户应用支撑能力`透明的服务注册和服务发现机制`内建负载均衡器`故障发现和自我修复能力`服务滚动升级和在线扩容`可扩展的资源自动调度机制`多粒度的资源配额管理能力.

Kubernetes 还提供完善的管理工具,涵盖开发`部署测试`运维监控等各个环节.

每个API对象都有3大类属性:元数据metadata`规范spec和状态status

- Concepts: Pod, Service, Labels和单Pod单IP
** Installing and Setting Up kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl/

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
** kubectl --help
kubectl controls the Kubernetes cluster manager.

Find more information at https://github.com/kubernetes/kubernetes.

Basic Commands (Beginner):
  create         Create a resource by filename or stdin
  expose         Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
  run            Run a particular image on the cluster
  set            Set specific features on objects

Basic Commands (Intermediate):
  get            Display one or many resources
  explain        Documentation of resources
  edit           Edit a resource on the server
  delete         Delete resources by filenames, stdin, resources and names, or by resources and label selector

Deploy Commands:
  rollout        Manage a deployment rollout
  rolling-update Perform a rolling update of the given ReplicationController
  scale          Set a new size for a Deployment, ReplicaSet, Replication Controller, or Job
  autoscale      Auto-scale a Deployment, ReplicaSet, or ReplicationController

Cluster Management Commands:
  certificate    Modify certificate resources.
  cluster-info   Display cluster info
  top            Display Resource (CPU/Memory/Storage) usage.
  cordon         Mark node as unschedulable
  uncordon       Mark node as schedulable
  drain          Drain node in preparation for maintenance
  taint          Update the taints on one or more nodes

Troubleshooting and Debugging Commands:
  describe       Show details of a specific resource or group of resources
  logs           Print the logs for a container in a pod
  attach         Attach to a running container
  exec           Execute a command in a container
  port-forward   Forward one or more local ports to a pod
  proxy          Run a proxy to the Kubernetes API server
  cp             Copy files and directories to and from containers.
  auth           Inspect authorization
Advanced Commands:
  apply          Apply a configuration to a resource by filename or stdin
  patch          Update field(s) of a resource using strategic merge patch
  replace        Replace a resource by filename or stdin
  convert        Convert config files between different API versions

Settings Commands:
  label          Update the labels on a resource
  annotate       Update the annotations on a resource
  completion     Output shell completion code for the specified shell (bash or zsh)

Other Commands:
  api-versions   Print the supported API versions on the server, in the form of "group/version"
  config         Modify kubeconfig files
  help           Help about any command
  version        Print the client and server version information

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
** kubernetes: The connection to the server localhost:8080 was refused - did you specify the right host or port?
https://github.com/kubernetes/kubernetes/issues/23092
** Layers
- Nucleus: API And Execution
- Application layer: deployment and running
- Govermance layer: automation and policy enforcement
- Interface layer: client libraries and tools
- Ecosystem
** healthcheck: LivenessProbe, ReadinessProbe
** 核心组件
Kubernetes主要由以下几个核心组件组成:
- etcd保存了整个集群的状态;
- apiserver提供了资源操作的唯一入口,并提供认证`授权`访问控制`API注册和发现等机制;
- controller manager负责维护集群的状态,比如故障检测`自动扩展`滚动更新等;
- scheduler负责资源的调度,按照预定的调度策略将Pod调度到相应的机器上;
- kubelet负责维护容器的生命周期,同时也负责Volume（CVI）和网络（CNI）的管理;
- Container runtime负责镜像管理以及Pod和容器的真正运行（CRI）;
- kube-proxy负责为Service提供cluster内部的服务发现和负载均衡
** helloworld
https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/
** useful link
https://kubernetes.io
https://www.reddit.com/r/devops/comments/51ra9q/moving_from_docker_to_rkt/
http://blog.dataman-inc.com/67/
http://jpadilla.com/post/161144157937/update-kubernetes-deployment-after-pushing-image

http://www.oschina.net/news/70140/infoworlds-2016-technology-of-the-year-award-winners?p=3#comments
** DONE Principle: API的操作复杂度不能超过O(N)
   CLOSED: [2017-06-10 Sat 15:24]
https://kubernetes.feisky.xyz/architecture/concepts.html
API操作复杂度与对象数量成正比.这一条主要是从系统性能角度考虑,要保证整个系统随着系统规模的扩大,性能不会迅速变慢到无法使用,那么最低的限定就是API的操作复杂度不能超过O(N),N是对象的数量,否则系统就不具备水平伸缩性了.
** Principle: API对象状态不能依赖于网络连接状态
https://kubernetes.feisky.xyz/architecture/concepts.html
** #  --8<-------------------------- separator ------------------------>8--
** TODO [#A] fail to start minikube: "VBoxManage not found. Make sure VirtualBox is installed and VBoxManage is in the path".
root@totvsjenkins:/tmp# minikube start
Starting local Kubernetes v1.6.4 cluster...
Starting VM...
E0610 20:14:57.518198   27907 start.go:127] Error starting host: Error creating host: Error with pre-create check: "VBoxManage not found. Make sure VirtualBox is installed and VBoxManage is in the path".

 Retrying.
E0610 20:14:57.519201   27907 start.go:133] Error starting host:  Error creating host: Error with pre-create check: "VBoxManage not found. Make sure VirtualBox is installed and VBoxManage is in the path"
** TODO how kubernetes use etcd
** TODO how healthcheck is implemented
** TODO What about alerting and reporting
** TODO what's fluentd
** #  --8<-------------------------- separator ------------------------>8--
** TODO [#A] k8s support rolling deployment                       :IMPORTANT:
https://www.youtube.com/watch?v=7TOWLerX0Ps
Kubernetes: zero downtime update at 1 million requests per second
https://www.youtube.com/watch?v=9C6YeyyUUmI
Kubernetes: zero downtime update at 10 million QPS
** TODO [#A] How to scale Pods with volumes configured            :IMPORTANT:
** What is Kubernetes
https://www.youtube.com/watch?v=R-3dfURb2hA
What is Kubernetes

Deployment, Scaling, Monitoring
** DONE Kubernetes hellworld
  CLOSED: [2017-07-11 Tue 08:42]
https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/#create-a-minikube-cluster

# build image
docker build -t hello-node:v1 .

# create deployment
kubectl run hello-node --image=hello-node:v1 --port=8080

# View the Deployment
kubectl get deployments

# Create service
kubectl expose deployment hello-node --type=LoadBalancer
** TODO [#A] Install minikube in headless Ubuntu server           :IMPORTANT:
| Name            | Summary |
|-----------------+---------|
| minikube status |         |
** DONE [#A] Ubuntu install kubernetes for all-in-one POC: minikube
  CLOSED: [2017-07-11 Tue 08:43]
https://blog.jetstack.io/blog/k8s-getting-started-part2/
https://github.com/kubernetes/minikube
https://stackoverflow.com/questions/38528762/kubernetes-on-ubuntu-16-04
https://hxquangnhat.com/2016/12/21/tutorial-deploy-a-kubernetes-cluster-on-ubuntu-16-04/
*** TODO minikube fail to start
#+BEGIN_EXAMPLE
root@totvsjenkins:/home/denny/minikube# ./minikube start --vm-driver=none --use-vendored-driver
Starting local Kubernetes v1.6.4 cluster...
Starting VM...
Moving files into cluster...

Setting up certs...
Starting cluster components...
Connecting to cluster...
Setting up kubeconfig...
Kubectl is now configured to use the cluster.
===================
WARNING: IT IS RECOMMENDED NOT TO RUN THE NONE DRIVER ON PERSONAL WORKSTATIONS
        The 'none' driver will run an insecure kubernetes apiserver as root that may leave the host vulnerable to CSRF attacks
#+END_EXAMPLE
*** useful link
https://www.youtube.com/watch?v=PH-2FfFD2PU
Kubernetes in 5 mins
https://www.youtube.com/watch?v=DC7NECq3Ghs
Setting up and using a single node Kubernetes cluster.
https://www.youtube.com/watch?v=BDrcUjOczsE
Kubernetes - Local Testing

https://www.youtube.com/watch?v=R-3dfURb2hA
The Illustrated Children's Guide to Kubernetes

* TODO [#A] Run a task on every node in a cluster                  :noexport:
* TODO kubectl get all won't get psp                               :noexport:
#+BEGIN_EXAMPLE
root@009069ee-95d5-49a2-6b82-67aff8eb6737:/tmp/build/4ecf0f02# kubectl get all --all-namespaces
NAMESPACE                   NAME                                        READY     STATUS    RESTARTS   AGE
kube-system                 pod/heapster-6d5f964dbd-2xxcm               1/1       Running   0          1d
kube-system                 pod/kube-dns-6b697fcdbd-c4rmm               3/3       Running   0          1d
kube-system                 pod/kubernetes-dashboard-785584f46b-9wmqj   1/1       Running   0          1d
kube-system                 pod/metrics-server-6bbb689cf9-swtxc         1/1       Running   0          1d
kube-system                 pod/monitoring-influxdb-76fd8dcff6-qws9m    1/1       Running   0          1d
kube-system                 pod/wavefront-proxy-8498d5bbf4-gl6sw        4/4       Running   0          4m
test-afjogacpjsqfetejycxx   pod/busybox-io-ftpz8                        1/1       Running   0          1d

NAMESPACE                   NAME                               DESIRED   CURRENT   READY     AGE
test-afjogacpjsqfetejycxx   replicationcontroller/busybox-io   1         1         1         1d

NAMESPACE     NAME                           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
default       service/kubernetes             ClusterIP   10.100.200.1     <none>        443/TCP         1d
kube-system   service/heapster               ClusterIP   10.100.200.123   <none>        8443/TCP        1d
kube-system   service/kube-dns               ClusterIP   10.100.200.10    <none>        53/UDP,53/TCP   1d
kube-system   service/kubernetes-dashboard   NodePort    10.100.200.8     <none>        443:32433/TCP   1d
kube-system   service/metrics-server         ClusterIP   10.100.200.102   <none>        443/TCP         1d
kube-system   service/monitoring-influxdb    ClusterIP   10.100.200.89    <none>        8086/TCP        1d

NAMESPACE     NAME                                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/heapster               1         1         1            1           1d
kube-system   deployment.apps/kube-dns               1         1         1            1           1d
kube-system   deployment.apps/kubernetes-dashboard   1         1         1            1           1d
kube-system   deployment.apps/metrics-server         1         1         1            1           1d
kube-system   deployment.apps/monitoring-influxdb    1         1         1            1           1d
kube-system   deployment.apps/wavefront-proxy        1         1         1            1           4m

NAMESPACE     NAME                                              DESIRED   CURRENT   READY     AGE
kube-system   replicaset.apps/heapster-6d5f964dbd               1         1         1         1d
kube-system   replicaset.apps/kube-dns-6b697fcdbd               1         1         1         1d
kube-system   replicaset.apps/kubernetes-dashboard-785584f46b   1         1         1         1d
kube-system   replicaset.apps/metrics-server-6bbb689cf9         1         1         1         1d
kube-system   replicaset.apps/monitoring-influxdb-76fd8dcff6    1         1         1         1d
kube-system   replicaset.apps/wavefront-proxy-8498d5bbf4        1         1         1         4m
root@009069ee-95d5-49a2-6b82-67aff8eb6737:/tmp/build/4ecf0f02# kubectl get psp
NAME              PRIV      CAPS      SELINUX    RUNASUSER   FSGROUP    SUPGROUP   READONLYROOTFS   VOLUMES
kube-system-psp   false     *         RunAsAny   RunAsAny    RunAsAny   RunAsAny   false            configMap,emptyDir,projected,secret,downwardAPI
root@009069ee-95d5-49a2-6b82-67aff8eb6737:/tmp/build/4ecf0f02# kubectl get all --all-namespaces | grep kube-system-psp
#+END_EXAMPLE
* TODO where is k8s job log?                                       :noexport:
http://kubernetesbyexample.com/jobs/
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO kubectl logs --previous nginx-app-zibvs                     :noexport:
https://jimmysong.io/cheatsheets/kubernetes-kubectl
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO [#A] play with k8s ingress service                          :noexport:
* TODO Vanilla CNCF Certified Kubernetes                           :noexport:
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO [#A] try admission controller                               :noexport:
* HALF Accessing Kubernetes API from pods                          :noexport:
 curl -k -v --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://<mycluster>
* TODO k8s trainning course from linux foundation: https://training.linuxfoundation.org/training/introduction-to-kubernetes/ :noexport:
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO consolidate: https://codefresh.io/kubernetes-tutorial/page/4/ :noexport:
* TODO consolidate: https://info.shadow-soft.com/hubfs/Kubernetes-Cheatsheet-Mesosphere.pdf :noexport:
* TODO consolidate: https://kapeli.com/cheat_sheets/Kubernetes.docset/Contents/Resources/Documents/index :noexport:
* TODO consolidate: https://lzone.de/cheat-sheet/kubernetes        :noexport:
* TODO consolidate: http://www.productiondown.com/devops/2018/08/02/Kubernetes-Commands-Cheatsheet.html :noexport:
* TODO consolidate cheatsheet: https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/kubernetes.sh :noexport:
* TODO consolidate: http://kubernetesbyexample.com/                :noexport:
* TODO consolidate https://jimmysong.io/cheatsheets/kubernetes-tricks :noexport:
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* HALF use kubectl to pull docker images, instead of ssh to vm     :noexport:
* HALF use kubectl to cleanup docker images, instead of ssh to vm  :noexport:
https://github.com/onfido/k8s-cleanup/blob/master/docker-clean.yml
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO pv termination hangs there forever                          :noexport:
#+BEGIN_EXAMPLE
   /Users/zdenny/git_code/codecommit/devops_blog/k8s  kubectl get pv                                                                                                                                                  master ✘ ✹  ✔ 0
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS        CLAIM                           STORAGECLASS   REASON    AGE
db-pv-volume                               400Gi      RWO            Retain           Available                                                              12h
pvc-bbddb940-5f43-11e9-ba3c-42010a800085   1Gi        RWO            Delete           Bound         denny-websites/cdn-pv-claim     standard                 12h
website-pv-volume                          10Gi       RWO            Retain           Terminating   denny-websites/mysql-pv-claim   standard                 12h
#+END_EXAMPLE
* TODO k8s configmap can't be changed                              :noexport:
#+BEGIN_EXAMPLE
   /Users/zdenny/git_code/codecommit/devops_blog/k8s  kubectl logs -n denny-websites pod/nginx-b88c67f77-dkw64                                                                                                    master ✘ ✖ ✹ ✭  ✔ 0
Update /etc/nginx/conf.d/default.conf
+ echo 'Update /etc/nginx/conf.d/default.conf'
+ sed -i s/http_port_here/80/g /etc/nginx/conf.d/default.conf
sed: cannot rename /etc/nginx/conf.d/sedz2uuPB: Device or resource busy
#+END_EXAMPLE
* TODO [#A] k8s mount configmap file, then edit it when process boostrap :noexport:
* TODO gce disk: how and when the filesystem formating happens?    :noexport:
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO k8s pod share volume within containers                      :noexport:
* TODO gce use one disk in a small chunks                          :noexport:
* TODO k8s mount jenkins home volume, then dockerfile copy/jenkins groovy. How to align? :noexport:
COPY resources/jobs/ /usr/share/jenkins/ref/jobs/
* #  --8<-------------------------- separator ------------------------>8-- :noexport:
* TODO k8s: when jenkins pod gets recreated, jenkins secret parameters need to be reconfigured :noexport:
* TODO k8s: instruct application to run a clean shutdown or a safe restart :noexport:
https://support.cloudbees.com/hc/en-us/articles/115003926511-Best-Practices-for-Jenkins-Updates-Patches-and-Maintenance
