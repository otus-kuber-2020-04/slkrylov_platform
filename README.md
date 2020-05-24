# Выполнено ДЗ №2

 - [X] Основное ДЗ
 - [X] Задание со *

# В процессе сделано:
 1. ## Установил под CentOS инструмент kubectl. Установку kubectl осуществлял по инструкции https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux. Инструкция по инструменту kubectl https://kubernetes.io/docs/reference/kubectl/overview/ 1
 2. ## Включил автодоплнение для kubectl в bash по инструкции https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-autocomplete
 3. ## Установил пакеты для KVM
	```
    yum install libvirt-daemon-kvm.x86_64 qemu-kvm-tools.x86_64 qemu-kvm.x86_64 qemu-kvm-common.x86_64
    yum install libvirt-bash-completion.x86_64 virt-*
	```
 4. ## Установил minicube по инструкции https://kubernetes.io/docs/tasks/tools/install-minikube/ для работы с KVM https://minikube.sigs.k8s.io/docs/drivers/kvm2/ Documentation: https://minikube.sigs.k8s.io/docs/reference/drivers/kvm2/
	```
    [dragon@dreamer-pc ~]$ minikube start --driver=kvm2
      minikube v1.9.2 on Centos 7.7.1908
      Using the kvm2 driver based on existing profile

      ❗  'kvm2' driver reported an issue: /bin/virsh domcapabilities --virttype kvm failed:

      Suggestion: Follow your Linux distribution instructions for configuring KVM
      Documentation: https://minikube.sigs.k8s.io/docs/reference/drivers/kvm2/

      Starting control plane node m01 in cluster minikube
      Updating the running kvm2 "minikube" VM ...
      Preparing Kubernetes v1.18.0 on Docker 19.03.8 ...
      Enabling addons: default-storageclass, storage-provisioner
      Done! kubectl is now configured to use "minikube"
      💾  Downloading driver docker-machine-driver-kvm2:
	
    [dragon@dreamer-pc ~]$ minikube status
      m01
      host: Running
      kubelet: Running
      apiserver: Running
      kubeconfig: Configured
      
    [dragon@dreamer-pc ~]$ minikube config set driver kvm2
    
    [dragon@dreamer-pc ~]$ kubectl config view
      apiVersion: v1
      clusters:
      - cluster:
	  certificate-authority: /home/dragon/.minikube/ca.crt
	  server: https://192.168.39.228:8443
	name: minikube
      contexts:
      - context:
	  cluster: minikube
	  user: minikube
	name: minikube
      current-context: minikube
      kind: Config
      preferences: {}
      users:
      - name: minikube
	user:
	  client-certificate: /home/dragon/.minikube/profiles/minikube/client.crt
	  client-key: /home/dragon/.minikube/profiles/minikube/client.key
	  
    [dragon@dreamer-pc ~]$ kubectl cluster-info 
      Kubernetes master is running at https://192.168.39.228:8443
      KubeDNS is running at https://192.168.39.228:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
      To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
	```
 5.  ## Установил Web UI (Dashboard) https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
     - ### kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
     - ### kubectl apply -f ../Documents/study/other/dashboard-adminuser.yaml 
		serviceaccount/admin-user created
     - ### kubectl apply -f ../Documents/study/other/dashboard-rolebinding.yaml 
		clusterrolebinding.rbac.authorization.k8s.io/admin-user created
     - ### kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
        ```
        Name:         admin-user-token-h66wl
        Namespace:    kubernetes-dashboard
        Labels:       <none>
        Annotations:  kubernetes.io/service-account.name: admin-user
            kubernetes.io/service-account.uid: 1b42d6cb-6ea9-48f2-84e5-f85422359f26

        Type:  kubernetes.io/service-account-token

        Data
        ====
        ca.crt:     1066 bytes
        namespace:  20 bytes
        token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IlJQYTRMcW1QaXN5Y3VEdU5yOGVLTTF2UFVBVmdvRWVac3F0ZlBWanRqSXMifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLWg2NndsIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIxYjQyZDZjYi02ZWE5LTQ4ZjItODRlNS1mODU0MjIzNTlmMjYiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.vOptADv5cZz_GQK1-ibtc66meHksT8s_k0h0LQ48zMtUhbSSL4uqulGpzXwHnCP8miVk1lAvh6p_63VZpipOIM0q1zx_NVMST0eyxir945P3_obLxn5gmtTUMojVSH0tUaoMQvJlEPnN4s9te8L3IxtXjVm5-qliwa7Is50sBAkMoGi8IiAxh6EqVGhPMUZwrjrK6wd5jkyIXHF5B39ZNB_25fdjxTgyYslwYMPijSLUIKLvdF0IPO4LGlukuw87Tan6FrkleFo9Z8TOfSL6PrIFKXdW6JgOmy6FgZ_iCkdKEOjkBmZORCjgOZSWkulQC8_mDBhekLZk-dgFdewaLg
        ```
	  - ### kubectl proxy
        ```
        Starting to serve on 127.0.0.1:8001
        ```
	  - ### доступ к дашборду можно получить по адресу http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
	
 6. ## Установил консольную утилиту k9s (https://github.com/derailed/k9s/blob/master/README.md)
 
 7. ## Работа на виртуальной машине minicube
 
    - ### соединение с виртуальной машиной minicube
      ```
      minikube ssh
      $ uname -a
      Linux minikube 4.19.107 #1 SMP Thu Mar 26 11:33:10 PDT 2020 x86_64 GNU/Linux
      ```
    - ### получил список запущенных контейнеров
      ```
      -a, --all             Show all containers (default shows just running)
      -q, --quiet           Only display numeric IDs
      ```
      ```
      $ docker ps -a -q
        145fb4daae8d
        49f7815e4997
        23466e0b6860
        c32d639015f8
        e087e64288ff
        7291310a46f7
        0fbcca2bf4b4
        edff6df73d88
        ed7eca588693
        18ca8f9f18f4
        69e69770aa20
        adc18a44e773
        f81361ed4cca
        863e89fb6c2d
        7abfeba89c8f
        25996f079947
        beb1b3604f08
        2aadf49babbb
        3619a5ce0c08
        0d4f7f2d3173
      ```
  8. ## работа с инструментом kubectl

      - ### получение состояния компонент кластера
        ```
        [dragon@dreamer-pc ~]$ kubectl get componentstatuses
          NAME                 STATUS    MESSAGE             ERROR
          controller-manager   Healthy   ok                  
          scheduler            Healthy   ok                  
          etcd-0               Healthy   {"health":"true"}
        ```
      - ### удаление всех PODs из NS kube-system
        ```
        [dragon@dreamer-pc ~]$ kubectl delete pod --all -n kube-system
          pod "coredns-66bff467f8-ctg4k" deleted
          pod "coredns-66bff467f8-krxzw" deleted
          pod "etcd-minikube" deleted
          pod "kube-apiserver-minikube" deleted
          pod "kube-controller-manager-minikube" deleted
          pod "kube-proxy-k7ws5" deleted
          pod "kube-scheduler-minikube" deleted
          pod "storage-provisioner" deleted
        ```
      - ### получение списка PODs в NS kube-system
        ```
        [dragon@dreamer-pc ~]$ kubectl get pods -o wide -n kube-system 
          NAME                               READY   STATUS    RESTARTS   AGE   IP               NODE       NOMINATED NODE   READINESS GATES
          coredns-66bff467f8-mwnpm           1/1     Running   0          58m   172.17.0.6       minikube   <none>           <none>
          coredns-66bff467f8-vgkrc           1/1     Running   0          58m   172.17.0.7       minikube   <none>           <none>
          etcd-minikube                      1/1     Running   0          58m   192.168.39.228   minikube   <none>           <none>
          kube-apiserver-minikube            1/1     Running   0          58m   192.168.39.228   minikube   <none>           <none>
          kube-controller-manager-minikube   1/1     Running   0          58m   192.168.39.228   minikube   <none>           <none>
          kube-proxy-xss5r                   1/1     Running   0          58m   192.168.39.228   minikube   <none>           <none>
          kube-scheduler-minikube            1/1     Running   0          58m   192.168.39.228   minikube   <none>           <none>
        ```
      - ### Q: почему все pod в namespace kube-system восстановились после удаления?
          - #### HINT: core-dns и, например, kube-apiserver , имеют различия в механизме запуска и восстанавливаются по разным причинам
          - #### A: предполагаю, что дело в контроллерах, которые отлеживают данные контейнеры и в парметрах Liveness
    
      - ### получить подробную информацию об Deployment для Pod coredns
        ```
        [dragon@dreamer-pc ~]$ kubectl describe deployment coredns --namespace kube-system
          Name:                   coredns
          Namespace:              kube-system
          CreationTimestamp:      Tue, 05 May 2020 23:41:10 +0300
          Labels:                 k8s-app=kube-dns
          Annotations:            deployment.kubernetes.io/revision: 1
          Selector:               k8s-app=kube-dns
          Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
          StrategyType:           RollingUpdate
          MinReadySeconds:        0
          RollingUpdateStrategy:  1 max unavailable, 25% max surge
          Pod Template:
          Labels:           k8s-app=kube-dns
          Service Account:  coredns
          Containers:
          coredns:
            Image:       k8s.gcr.io/coredns:1.6.7
            Ports:       53/UDP, 53/TCP, 9153/TCP
            Host Ports:  0/UDP, 0/TCP, 0/TCP
            Args:
              -conf
              /etc/coredns/Corefile
            Limits:
              memory:  170Mi
            Requests:
              cpu:        100m
              memory:     70Mi
            Liveness:     http-get http://:8080/health delay=60s timeout=5s period=10s #success=1 #failure=5
            Readiness:    http-get http://:8181/ready delay=0s timeout=1s period=10s #success=1 #failure=3
            Environment:  <none>
            Mounts:
              /etc/coredns from config-volume (ro)
          Volumes:
          config-volume:
            Type:               ConfigMap (a volume populated by a ConfigMap)
            Name:               coredns
            Optional:           false
          Priority Class Name:  system-cluster-critical
              Conditions:
          Type           Status  Reason
          ----           ------  ------
          Progressing    True    NewReplicaSetAvailable
          Available      True    MinimumReplicasAvailable
              OldReplicaSets:  <none>
              NewReplicaSet:   coredns-66bff467f8 (2/2 replicas created)
              Events:          <none>
        ```  
  9. ## работа с docker
      - ### создание образа из Dockerfile
        ```
        [root@dreamer-pc web]# docker build /home/dragon/Documents/study/slkrylov_platform/kubernetes-intro/web --tag kubernetes-intro:latest
        ```
      
      - ### просмотреть список доступных images
        ```
        [root@dreamer-pc web]# docker images 
        REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
        kubernetes-intro    latest              67b92f231ac9        39 minutes ago      19.9MB
        ```
      - ### просмотреть информацию о image
        ```
        [root@dreamer-pc]# docker inspect 67b92f231ac9
        ```
      
      - ### запустить контейнер с перенаправлением прослушиваемых портов 
        -d запуск контейнера в background
        ```
        [root@dreamer-pc]# docker run -p 127.0.0.1:8000:8000/tcp kubernetes-intro:latest
        ```
      - ### скопировать "неизвестный" файл с контейнера (можно экспортировать image в tarball затем в нем найти файл)
        ```
        [root@dreamer-pc]# docker container export compassionate_black -o /tmp/kubernetes-intro.tar
        ```
      - ### размещение образа в https://hub.docker.com (https://docs.docker.com/docker-hub/repos/)
        ```
          [root@dreamer-pc nginx]# docker login 
            Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
            Username: slkrylov
            Password: 
            WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
            Configure a credential helper to remove this warning. See
            https://docs.docker.com/engine/reference/commandline/login/#credentials-store
            Login Succeeded
            
          [root@dreamer-pc nginx]# docker commit -m "home work NO 1" condescending_noyce slkrylov/otus:kubernetes-intro
            sha256:861cb9dd8624f8f9b39ee1f6dbe2f9e9762e7a90d8df5e9fc3b6375a5c6ae86a
            
          [root@dreamer-pc nginx]# docker push slkrylov/otus:kubernetes-intro
            The push refers to repository [docker.io/slkrylov/otus]
            2e4dc21ef845: Pushed 
            0d40e64a8582: Pushed 
            3f0d8b571ad9: Pushed 
            3030023c51bf: Pushed 
            3810cc0c140f: Layer already exists 
            3e207b409db3: Layer already exists 
            kubernetes-intro: digest: sha256:870dedb7328d9ac6a54478cb4676ce569be4fd837f581e770ac61c97df12e100 size: 1568
        ```
      - ### удаление всех образов
        ```
          [root@dreamer-pc web]# docker rmi $(docker images -q)
        ```
      - ### скачать и запустить готовый образ из https://hub.docker.com
        ```
          [root@dreamer-pc nginx]# docker pull slkrylov/otus:kubernetes-intro
      
          [root@dreamer-pc web]# docker images 
            REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
            slkrylov/otus       kubernetes-intro    5f1ce74f0d0e        35 minutes ago      19.9MB
                
          [root@dreamer-pc nginx]# docker run -d -p 127.0.0.1:8000:8000/tcp slkrylov/otus:kubernetes-intro 
            d8c5b8e1ac13d58e0dd0a8cb9eb064ce0beaae98e811b2dbd9ab8ecad46f9cad  
      
          [root@dreamer-pc nginx]# docker ps
            CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS              PORTS                              NAMES
            d8c5b8e1ac13        slkrylov/otus:kubernetes-intro   "nginx -g 'daemon of…"   19 seconds ago      Up 18 seconds       80/tcp, 127.0.0.1:8000->8000/tcp   stupefied_greider
	
          [root@dreamer-pc nginx]# ps -aux | grep 'nginx' | grep -v grep
            1001     19021  0.0  0.0   5972  2620 ?        Ss   21:42   0:00 nginx: master process nginx -g daemon off;
            1001     19048  0.0  0.0   6412  1556 ?        S    21:42   0:00 nginx: worker process
            1001     19049  0.0  0.0   6412  1320 ?        S    21:42   0:00 nginx: worker process
            1001     19050  0.0  0.0   6412  1320 ?        S    21:42   0:00 nginx: worker process
            1001     19051  0.0  0.0   6412  1320 ?        S    21:42   0:00 nginx: worker process
        ```
  10. ## Рaбота с kubectl 

      - ### создание Pod из манифеста
        ```
          [dragon@dreamer-pc kubernetes-intro]$ kubectl apply -f web-pod.yaml 
            pod/web created

          [dragon@dreamer-pc kubernetes-intro]$ kubectl get pods
                NAME   READY   STATUS    RESTARTS   AGE
                web    1/1     Running   0          92s
        ```
      - ### получение манифеста уже созданого Pod
        ```
          [dragon@dreamer-pc kubernetes-intro]$ kubectl get pod web -o "yaml"
          [dragon@dreamer-pc kubernetes-intro]$ kubectl describe pods web
        ```
      - ### Добавление init-контейнера в Pod-манифест (https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)
      - ### Добавление в контейнеры volumeMounts (https://kubernetes.io/fr/docs/concepts/storage/volumes/)
        ```
          spec:
            containers:
            - name: web-app-no1
              image: slkrylov/otus:kubernetes-intro
              volumeMounts:
                - mountPath: /app
            name: app
            initContainers:
            - name: init-web-app-no1
              image: busybox:1.31.0
              command: ['sh', '-c', 'wget -O- https://tinyurl.com/otus-k8s-intro | sh']
              volumeMounts:
                - mountPath: /app
            name: app
            volumes:
            - name: app
              emptyDir: {}
        ```
      - ### Создание Pod из отредатированного манифеста и наблюдение за процессом создания
        ```
          [dragon@dreamer-pc kubernetes-intro]$ kubectl apply -f web-pod.yaml && kubectl get pods -w
            pod/web created
            NAME   READY   STATUS     RESTARTS   AGE
            web    0/1     Init:0/1   0          1s
            web    0/1     Init:0/1   0          8s
            web    0/1     PodInitializing   0          10s
            web    1/1     Running           0          11s
        ```
      - ### проброс локального порта на порт Pod-a
        ```
          [dragon@dreamer-pc kubernetes-intro]$ kubectl port-forward --address 0.0.0.0 pod/web 8000:8000
        ```
  10. ## Утилита kube-forwader не взлетела :( (https://kube-forwarder.pixelpoint.io/)
  
  11. ## Hipster Shop

      - ### [dragon@dreamer-pc other]$ git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
      - ### сборка образа frontend из microservices
      ```
        [dragon@dreamer-pc frontend]$  docker build . --tag microservices:frontend
          Sending build context to Docker daemon  4.227MB
          Step 1/16 : FROM golang:1.12-alpine as builder
          1.12-alpine: Pulling from library/golang
          c9b1b535fdd9: Pull complete
          Successfully built fc60e0b99009
          Successfully tagged microservices:frontend
      ```
      - ### [dragon@dreamer-pc frontend]$  docker images
      ```
        REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
        microservices       frontend            fc60e0b99009        21 minutes ago      43.3MB
        <none>              <none>              2750efb5ba83        25 minutes ago      1.27GB
        slkrylov/otus       kubernetes-intro    861cb9dd8624        24 hours ago        19.9MB
        alpine              latest              f70734b6a266        3 weeks ago         5.61MB
        golang              1.12-alpine         76bddfb5e55e        3 months ago        346MB
      ```
      - ### [dragon@dreamer-pc ]$ docker login
      - ### [dragon@dreamer-pc ]$ docker tag b1e18d2b1f55 slkrylov/otus:microservices-frontend
      - ### [dragon@dreamer-pc ]$ docker push slkrylov/otus:microservices-frontend
      - ### [dragon@dreamer-pc ]$ docker images
        ```
          REPOSITORY          TAG                      IMAGE ID            CREATED             SIZE
          microservices       frontend                 b1e18d2b1f55        22 hours ago        43.3MB
          slkrylov/otus       microservices-frontend   b1e18d2b1f55        22 hours ago        43.3MB
          <none>              <none>                   8f2bc86dbbc2        22 hours ago        1.27GB
          <none>              <none>                   2750efb5ba83        23 hours ago        1.27GB
          slkrylov/otus       kubernetes-intro         861cb9dd8624        47 hours ago        19.9MB
          alpine              latest                   f70734b6a266        3 weeks ago         5.61MB
          golang              1.12-alpine              76bddfb5e55e        3 months ago        346MB
        ```
      - ### [dragon@dreamer-pc kubernetes-intro]$ kubectl run frontend --image slkrylov/otus:microservices-frontend --restart=Never --dry-run=client -o yaml > frontend-pod.yaml
      - ### [dragon@dreamer-pc kubernetes-intro]$ kubectl apply -f frontend-pod.yaml
        ```
	        pod/frontend created
        ```
      - ### [dragon@dreamer-pc kubernetes-intro]$ kubectl get pods
        ```
          NAME       READY   STATUS    RESTARTS   AGE
          frontend   0/1     Error     0          75s
          web        1/1     Running   0          28h
        ```
      - ### [dragon@dreamer-pc kubernetes-intro]$ kubectl logs frontend
        ```
          {"message":"Tracing enabled.","severity":"info","timestamp":"2020-05-18T18:10:55.519155606Z"}
          {"message":"Profiling enabled.","severity":"info","timestamp":"2020-05-18T18:10:55.519248547Z"}
          {"message":"jaeger initialization disabled.","severity":"info","timestamp":"2020-05-18T18:10:55.519356832Z"}
          panic: environment variable "PRODUCT_CATALOG_SERVICE_ADDR" not set
          goroutine 1 [running]:
          main.mustMapEnv(0xc000342000, 0xb0f14d, 0x1c)
            /go/src/github.com/GoogleCloudPlatform/microservices-demo/src/frontend/main.go:259 +0x10e
          main.main()
            /go/src/github.com/GoogleCloudPlatform/microservices-demo/src/frontend/main.go:117 +0x4ff
		    ```

      - ### после в Dockerfile микросервиса frontend установил перменные окружения и снова пересоздал образ и Pod
	      - #### [dragon@dreamer-pc frontend]$ docker build . --tag kubernetes-intro:microservices-frontend-fix
	      - #### [dragon@dreamer-pc frontend]$ docker tag kubernetes-intro:microservices-frontend-fix slkrylov/otus:microservices-frontend-fix
	      - #### [dragon@dreamer-pc frontend]$ kubectl run frontend --image slkrylov/otus:microservices-frontend-fix --restart=Never --dry-run=client -o yaml > frontend-pod-healthy.yaml
	      - #### [dragon@dreamer-pc frontend]$ kubectl apply -f frontend-pod-healthy.yaml
          ``` 
	          pod/frontend created
          ```
	  - ### [dragon@dreamer-pc frontend]$ kubectl get pods
        ```
          NAME       READY   STATUS    RESTARTS   AGE
          frontend   1/1     Running   0          40s
          web        1/1     Running   0          30h
        ```
## Как запустить проект №1:
 - docker pull slkrylov/otus:kubernetes-intro
 - docker run -d -p 127.0.0.1:8000:8000/tcp slkrylov/otus:kubernetes-intro

## Как проверить работоспособность проекта №1:
 - перейти по ссылке http://localhost:8000

## Запустить проект *
 - kubectl run frontend --image slkrylov/otus:microservices-frontend-fix --restart=Never --dry-run=client -o yaml > frontend-pod.yaml
 - kubectl apply -f frontend-pod.yaml

## как проверить работоспособность проекта *
  - kubectl get pods


## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

-------------------------------------------------------------------------------
 
# Выполнено ДЗ №3

 - [] Основное ДЗ
 - [] Задание со *
 
# В процессе сделано:
  1.## Установил kind (https://kind.sigs.k8s.io/docs/user/quick-start)
    > kind creates and manages local Kubernetes clusters using Docker container 'nodes'
    
  ````
    [root@dreamer-pc ~]# curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.8.1/kind-$(uname)-amd64
	% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
				      Dload  Upload   Total   Spent    Left  Speed
      100    97  100    97    0     0    133      0 --:--:-- --:--:-- --:--:--   133
      100   629  100   629    0     0    406      0  0:00:01  0:00:01 --:--:--  1335
      100 9900k  100 9900k    0     0   372k      0  0:00:26  0:00:26 --:--:--  14
    
    [root@dreamer-pc ~]# chmod +x ./kind
    [root@dreamer-pc ~]# mv kind /usr/sbin/
    [root@dreamer-pc ~]# kind --version
      kind version 0.8.1

    [dragon@dreamer-pc slkrylov_platform]$ kind create cluster --config kubernetes-controllers/kind-config.yaml
      Creating cluster "kind" ...
      ✓ Ensuring node image (kindest/node:v1.18.2)
      ✓ Preparing node 
      ✓ Configuring the external load balancer
      ✓ Writing configuration 
      ✓ Starting control-plane
      ✓ Installing CNI
      ✓ Installing StorageClass
      ✓ Joining more control-plane nodes
      ✓ Joining worker nodes
      Set kubectl context to "kind-kind"
      You can now use your cluster with:
      kubectl cluster-info --context kind-kind
      Thanks for using kind!
      
    [dragon@dreamer-pc ~]$ kubectl cluster-info --context kind-kind
      Kubernetes master is running at https://127.0.0.1:34263
      KubeDNS is running at https://127.0.0.1:34263/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
      
    [dragon@dreamer-pc ~]$ kind get nodes
      kind-external-load-balancer
      kind-control-plane2
      kind-worker2
      kind-worker3
      kind-worker
      kind-control-plane
      kind-control-plane3
      
    [dragon@dreamer-pc ~]$ kind get clusters
      kind
      
    [dragon@dreamer-pc ~]$ kubectl get nodes
      NAME                  STATUS   ROLES    AGE   VERSION
      kind-control-plane    Ready    master   13m   v1.18.2
      kind-control-plane2   Ready    master   13m   v1.18.2
      kind-control-plane3   Ready    master   12m   v1.18.2
      kind-worker           Ready    <none>   11m   v1.18.2
      kind-worker2          Ready    <none>   11m   v1.18.2
      kind-worker3          Ready    <none>   11m   v1.18.2
    
    ! TIPS # посмотреть доступные контексты (кластеры)
    [dragon@dreamer-pc .kube]$ kubectl config get-contexts
      CURRENT   NAME        CLUSTER     AUTHINFO    NAMESPACE
      *         kind-kind   kind-kind   kind-kind   
		minikube    minikube    minikube
		
    ! TIPS # переключение между контекстами 
    [dragon@dreamer-pc ~]$ kubectl config use-context kind-kind
      Switched to context "kind-kind".
  ````
  