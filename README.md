# k8s setup in ubuntu


# 리눅스에 kubectl 설치 및 설정
#    https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/
##    리눅스에 kubectl 설치
        다음과 같은 방법으로 리눅스에 kubectl을 설치할 수 있다.

        리눅스에 curl을 사용하여 kubectl 바이너리 설치
        기본 패키지 관리 도구를 사용하여 설치
        다른 패키지 관리 도구를 사용하여 설치


## 리눅스에서 curl을 사용하여 kubectl 바이너리 설치 
    1. 다음 명령으로 최신 릴리스를 다운로드한다.
        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

    2. 바이너리를 검증한다. (선택 사항)
        kubectl 체크섬(checksum) 파일을 다운로드한다.
            curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

        kubectl 바이너리를 체크섬 파일을 통해 검증한다.
            echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
                검증이 성공한다면, 출력은 다음과 같다.
                    kubectl: OK
    3. kubectl 설치
        sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

    4. 설치한 버전이 최신인지 확인한다.
        kubectl version --client


## kubectl 구성 확인 
    kubectl cluster-info



        kubectl cluster-info
        URL 응답이 표시되면, kubectl이 클러스터에 접근하도록 올바르게 구성된 것이다.

        다음과 비슷한 메시지가 표시되면, kubectl이 올바르게 구성되지 않았거나 쿠버네티스 클러스터에 연결할 수 없다.

        The connection to the server <server-name:port> was refused - did you specify the right host or port?
        예를 들어, 랩톱에서 로컬로 쿠버네티스 클러스터를 실행하려면, Minikube와 같은 도구를 먼저 설치한 다음 위에서 언급한 명령을 다시 실행해야 한다.

        kubectl cluster-info가 URL 응답을 반환하지만 클러스터에 접근할 수 없는 경우, 올바르게 구성되었는지 확인하려면 다음을 사용한다.

        kubectl cluster-info dump




        IN Windows PowerShell

            PS C:\Users\sangbinlee9> ssh sangbinlee9@192.168.0.8 -p 22
            sangbinlee9@master:~$ sudo su
            root@master:/home/sangbinlee9# minikube start --force
            🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

            root@master:/home/sangbinlee9# kubectl get pods --all-namespaces -o wide
            NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
            kube-system   coredns-787d4945fb-6wc2q           1/1     Running   0          12s   10.244.0.2     minikube   <none>           <none>
            kube-system   etcd-minikube                      1/1     Running   0          26s   192.168.49.2   minikube   <none>           <none>
            kube-system   kube-apiserver-minikube            1/1     Running   0          26s   192.168.49.2   minikube   <none>           <none>
            kube-system   kube-controller-manager-minikube   1/1     Running   0          27s   192.168.49.2   minikube   <none>           <none>
            kube-system   kube-proxy-jg6n7                   1/1     Running   0          12s   192.168.49.2   minikube   <none>           <none>
            kube-system   kube-scheduler-minikube            1/1     Running   0          24s   192.168.49.2   minikube   <none>           <none>
            kube-system   storage-provisioner                1/1     Running   0          23s   192.168.49.2   minikube   <none>           <none>
            root@master:/home/sangbinlee9# kubectl cluster-info
            Kubernetes control plane is running at https://192.168.49.2:8443
            CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

            To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
            
            root@master:/home/sangbinlee9#





            root@master:/home/sangbinlee9#  kubectl cluster-info
            Kubernetes control plane is running at https://192.168.49.2:8443
            CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

            To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
            root@master:/home/sangbinlee9#

# 현재 사용하고 있는 쉘 확인하기

    root@master:~# grep root /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    root@master:~#










## 리눅스에서 curl을 사용하여 kubectl 바이너리 설치 
## 리눅스에서 curl을 사용하여 kubectl 바이너리 설치 
## 리눅스에서 curl을 사용하여 kubectl 바이너리 설치 
    설치순서
    ✔  공통 설치(master node, worker node 에 설치)

    1) CRI 설치 - 컨테이너 런타임

    2) kubeadm, kubectl, kubelet 설치

    3) CNI 설치

    

    ✔  master node

    - kubeadm init 실행

    

    ✔  worker node

    - kubeadm join 실행

    





















# master, worker1, worker2



    ## 필수 포트 확인 - 컴퓨터의 특정 포트들 개방.

        포트와 프로토콜
            물리적 네트워크 방화벽이 있는 온프레미스 데이터 센터 또는 퍼블릭 클라우드의 가상 네트워크와 같이 네트워크 경계가 엄격한 환경에서 쿠버네티스를 실행할 때, 쿠버네티스 구성 요소에서 사용하는 포트와 프로토콜을 알고 있는 것이 유용하다.

        컨트롤 플레인
            프로토콜	방향	포트 범위	용도	사용 주체
            TCP	인바운드	6443	쿠버네티스 API 서버	전부
            TCP	인바운드	2379-2380	etcd 서버 클라이언트 API	kube-apiserver, etcd
            TCP	인바운드	10250	Kubelet API	Self, 컨트롤 플레인
            TCP	인바운드	10259	kube-scheduler	Self
            TCP	인바운드	10257	kube-controller-manager	Self
            etcd 포트가 컨트롤 플레인 섹션에 포함되어 있지만, 외부 또는 사용자 지정 포트에서 자체 etcd 클러스터를 호스팅할 수도 있다.

        워커 노드
            프로토콜	방향	포트 범위	용도	사용 주체
            TCP	인바운드	10250	Kubelet API	Self, 컨트롤 플레인
            TCP	인바운드	30000-32767	NodePort 서비스†	전부

        # Master
            sudo ufw enable
            sudo ufw allow 6443/tcp
            sudo ufw allow 2379:2380/tcp
            sudo ufw allow 10250/tcp
            sudo ufw allow 10259/tcp
            sudo ufw allow 10257/tcp
            sudo ufw status

        # Worker
            sudo ufw enable
            sudo ufw allow 10250/tcp
            sudo ufw allow 30000:32767/tcp
            sudo ufw status
            ## IPv4를 포워딩하여 iptables가 브리지된 트래픽을 보게 하기



    # Swap off

        sudo swapoff -a && sudo sed -i '/swap/s/^/#/' /etc/fstab




# 컨테이너 런타임

    # 파드가 노드에서 실행될 수 있도록 클러스터의 각 노드에 컨테이너 런타임을 설치해야 한다. 


        ### Installing containerd
            $ tar Cxzvf /usr/local containerd-1.6.2-linux-amd64.tar.gz






            cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
            overlay
            br_netfilter
            EOF

            sudo modprobe overlay
            sudo modprobe br_netfilter

            # 필요한 sysctl 파라미터를 설정하면, 재부팅 후에도 값이 유지된다.
            cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
            net.bridge.bridge-nf-call-iptables  = 1
            net.bridge.bridge-nf-call-ip6tables = 1
            net.ipv4.ip_forward                 = 1
            EOF

            # 재부팅하지 않고 sysctl 파라미터 적용하기
            sudo sysctl --system





# master
    sudo kubeadm init


        Your Kubernetes control-plane has initialized successfully!

        To start using your cluster, you need to run the following as a regular user:

        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config

        Alternatively, if you are the root user, you can run:

        export KUBECONFIG=/etc/kubernetes/admin.conf

        You should now deploy a pod network to the cluster.
        Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
        https://kubernetes.io/docs/concepts/cluster-administration/addons/

        Then you can join any number of worker nodes by running the following on each as root:

        kubeadm join 192.168.0.8:6443 --token g0w2aj.90u5ctdqjvw9wzuc \
                --discovery-token-ca-cert-hash sha256:ba53a230869d5abfa5d023ebfbbe19557cf984ec978f49468bbb758a61bebea5



# worker1, worker2
        kubeadm join 192.168.0.8:6443 --token g0w2aj.90u5ctdqjvw9wzuc \
                --discovery-token-ca-cert-hash sha256:ba53a230869d5abfa5d023ebfbbe19557cf984ec978f49468bbb758a61bebea5





# sangbinlee9@master:~$  kubeadm reset






    root@master:~# kubeadm init
    [init] Using Kubernetes version: v1.27.2
    [preflight] Running pre-flight checks
    [preflight] Pulling images required for setting up a Kubernetes cluster
    [preflight] This might take a minute or two, depending on the speed of your internet connection
    [preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
    W0520 08:22:20.887915    3221 images.go:80] could not find officially supported version of etcd for Kubernetes v1.27.2, falling back to the nearest etcd version (3.5.7-0)
    W0520 08:22:20.996404    3221 checks.go:835] detected that the sandbox image "registry.k8s.io/pause:3.8" of the container runtime is inconsistent with that used by kubeadm. It is recommended that using "registry.k8s.io/pause:3.9" as the CRI sandbox image.
    [certs] Using certificateDir folder "/etc/kubernetes/pki"
    [certs] Generating "ca" certificate and key
    [certs] Generating "apiserver" certificate and key
    [certs] apiserver serving cert is signed for DNS names [kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local master] and IPs [10.96.0.1 192.168.0.8]
    [certs] Generating "apiserver-kubelet-client" certificate and key
    [certs] Generating "front-proxy-ca" certificate and key
    [certs] Generating "front-proxy-client" certificate and key
    [certs] Generating "etcd/ca" certificate and key
    [certs] Generating "etcd/server" certificate and key
    [certs] etcd/server serving cert is signed for DNS names [localhost master] and IPs [192.168.0.8 127.0.0.1 ::1]
    [certs] Generating "etcd/peer" certificate and key
    [certs] etcd/peer serving cert is signed for DNS names [localhost master] and IPs [192.168.0.8 127.0.0.1 ::1]
    [certs] Generating "etcd/healthcheck-client" certificate and key
    [certs] Generating "apiserver-etcd-client" certificate and key
    [certs] Generating "sa" key and public key
    [kubeconfig] Using kubeconfig folder "/etc/kubernetes"
    [kubeconfig] Writing "admin.conf" kubeconfig file
    [kubeconfig] Writing "kubelet.conf" kubeconfig file
    [kubeconfig] Writing "controller-manager.conf" kubeconfig file
    [kubeconfig] Writing "scheduler.conf" kubeconfig file
    [kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
    [kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
    [kubelet-start] Starting the kubelet
    [control-plane] Using manifest folder "/etc/kubernetes/manifests"
    [control-plane] Creating static Pod manifest for "kube-apiserver"
    [control-plane] Creating static Pod manifest for "kube-controller-manager"
    [control-plane] Creating static Pod manifest for "kube-scheduler"
    [etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
    W0520 08:22:24.124819    3221 images.go:80] could not find officially supported version of etcd for Kubernetes v1.27.2, falling back to the nearest etcd version (3.5.7-0)
    [wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
    [apiclient] All control plane components are healthy after 5.506057 seconds
    [upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
    [kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
    [upload-certs] Skipping phase. Please see --upload-certs
    [mark-control-plane] Marking the node master as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
    [mark-control-plane] Marking the node master as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
    [bootstrap-token] Using token: r3zhpf.wwfpbhenjnerkqlg
    [bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
    [bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to get nodes
    [bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
    [bootstrap-token] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
    [bootstrap-token] Configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
    [bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
    [kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
    [addons] Applied essential addon: CoreDNS
    [addons] Applied essential addon: kube-proxy

    Your Kubernetes control-plane has initialized successfully!

    To start using your cluster, you need to run the following as a regular user:

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

    Alternatively, if you are the root user, you can run:

    export KUBECONFIG=/etc/kubernetes/admin.conf

    You should now deploy a pod network to the cluster.
    Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
    https://kubernetes.io/docs/concepts/cluster-administration/addons/

    Then you can join any number of worker nodes by running the following on each as root:

    kubeadm join 192.168.0.8:6443 --token r3zhpf.wwfpbhenjnerkqlg \
            --discovery-token-ca-cert-hash sha256:fabf5cec4f7e24fb881de176c1b03c88c7b7aedc246636f587f47081f82f5787
    root@master:~#


kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml


