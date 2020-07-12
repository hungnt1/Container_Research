
## Sử dụng kubectl để khởi tạo một Deployment

### 1. Kubernetes Deployments
 
- Khi chạy một k8s cluster, có thể thực hiện deployment các ứng dụng trên này. Để làm điều này, cần định nghĩa một Deployment. Deployment cho phép khởi tạo và update các ứng dụng. Một khi ứng dụng được Deployment quản lý chúng sẽ được theo dõi lâu dài, nếu một node bị down Pod thuộc deployment trên node đấy sẽ được lập lịch khởi tạo trên node mới. Đây la cơ chế self-healing.

- Khởi tạo một deployment
```
[] kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

- Kiểm tra danh sách deployment
```
[] kubectl get deployments
kubectl get deployments
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   1/1     1            1           3m26s

```


### Pods and Nodes 

- Các Pod trong k8s đơn giản là một hoặc nhiều các container và cùng chia sẻ chung tài nguyên cho tất cả containr này. Các tài nguyên bao gồm:
  - Shared Storage
  - Networking, IP address
  - Thông tin về các container trong Pod đang chạy

- Cần lưu ý về Pod: " The containers in a Pod share an IP Address and port space, are always co-located and co-scheduled, and run in a shared context on the same Node."


- Các Pod thì chạy trên các Node. Mỗi node  được quản lý bởi Master. Một Nodes có thể chứa nhiều pods. Mỗi node sẽ bao gồm:
  - Kubelet, thực hiện liên hệ với node Master, thực hiện quản lý các Pod đang chạy trên node
  - Container runntime, thực hiện pulling, unpack và make run các image. 

- Xem danh sách các pod
```
[]  kubectl get pods
NAME                                   READY   STATUS      RESTARTS   AGE
frontend-7k58h                         1/1     Running     0          22h
frontend-kn2fh                         1/1     Running     0          22h
kubernetes-bootcamp-6f6656d949-4vhkq   1/1     Running     0          11m

```

- Xem thông tin về image và trnagj thái của Pod
```
[] kubectl describe kubernetes-bootcamp-6f6656d949-4vhkq
Name:         kubernetes-bootcamp-6f6656d949-4vhkq
Namespace:    default
Priority:     0
Node:         worker2.novalocal/10.10.204.45
Start Time:   Thu, 09 Jul 2020 13:51:05 +0700
Labels:       app=kubernetes-bootcamp
              pod-template-hash=6f6656d949
Annotations:  cni.projectcalico.org/podIP: 192.168.196.147/32
              cni.projectcalico.org/podIPs: 192.168.196.147/32
Status:       Running
IP:           192.168.196.147
IPs:
  IP:           192.168.196.147
Controlled By:  ReplicaSet/kubernetes-bootcamp-6f6656d949
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://fee4757aa58610ee59f82242f353f34c23611f68662dfb5046da802547c74fac
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://gcr.io/google-samples/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 09 Jul 2020 13:53:08 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-sph8h (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-sph8h:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-sph8h
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age        From                        Message
  ----    ------     ----       ----                        -------
  Normal  Scheduled  <unknown>  default-scheduler           Successfully assigned default/kubernetes-bootcamp-6f6656d949-4vhkq to worker2.novalocal
  Normal  Pulling    12m        kubelet, worker2.novalocal  Pulling image "gcr.io/google-samples/kubernetes-bootcamp:v1"
  Normal  Pulled     10m        kubelet, worker2.novalocal  Successfully pulled image "gcr.io/google-sa
```


- Xem log của pod
```
[] kubectl logs kubernetes-bootcamp-6f6656d949-4vhkq
Kubernetes Bootcamp App Started At: 2020-07-09T06:53:10.394Z | Running On:  kubernetes-bootcamp-6f6656d949-4vhkq

```

- Thực thi trong Pod
```
[] kubectl exec -ti  kubernetes-bootcamp-6f6656d949-4vhkq bash
kubectl exec -ti  kubernetes-bootcamp-6f6656d949-4vhkq bash
root@kubernetes-bootcamp-6f6656d949-4vhkq:/# ls
bin  boot  core  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  server.js  srv  sys  tmp  usr  var
root@kubernetes-bootcamp-6f6656d949-4vhkq:/# touch hi > kk.txt

```

## Administer a Cluster


## Using a Service to Expose Your App


### 1. Overivew of Service
- Pod trong K8S là không lâu dài, chúng nó vòng đời. Khi một node die, các Pod chạy trên node này sẽ được lập lịch để chạy trên các node khác để đảm bảo trạng thái desired state của các ReplicaSet. Trong quy trình phát triển ứng dụng, ví dụ front-end kết nối với backend, backend được replicate 3 Pod, tuy nhiên froent-end không quan tâm điều này, chũng chỉ cần biết kết nối với backend như này. Tuy nhiên mỗi Pod có 1 Ip riêng và chũng được thay đổi khi được rescheduler. Điềy này gây cản trở trong quá trình phát triển ứng dụng
- Một Service trong K8S là 1 khái niệm trừu tượng cho một set các Pod. Service cho phép kết nối lỏng lẽo giữa các Pod phụ thuộc vào nhau. Một Service được định nghĩa bởi YAML hoặc JSON. Các set Pod được Service gộp lại thường được xác định qua LableSelector.
- mặc dù mỗi Pod có 1 IP, tuy nhiên các IP của pod không thể truy cập từ ngoài vào. Service sẽ đảm nhiệm đứng trước để hứng và chuyển hướng các traffic vào các Service có LableSelector. Service có thể expose các Pod theo một số cách sau đâu:
  - ClusterIP ( default ): thực hiện expose service thông qua một địa chỉ IP trong cluster, tuy nhiên cách này chỉ cho phép service có thể truy cập ở trong cluster
  - Nodeport: service sẽ được expose ra sử dụng địa chỉ IP của các mỗi node của cluster. Cách này giúp người dùng truy cập vào Service thông qua <NodeIP>:<NodePort>
  - LoadBalancer : cách này cho phép khởi tạo một external LB của các nhà cloud-provider. cung cấp IP external cho Service
  - ExternalName: cung cấp CNAM record



### 2.Services and Labels

![](https://d33wubrfki0l68.cloudfront.net/b964c59cdc1979dd4e1904c25f43745564ef6bee/f3351/docs/tutorials/kubernetes-basics/public/images/module_04_labels.svg)

- Khởi tạo 1 Service ở mode Node port
```
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

- Xem danh sách service
```
[] kubectl get services

NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes-bootcamp   NodePort    10.102.68.67     <none>        8080:31908/TCP   19s

```
- Xem chi tiết service. ở đây port được expose random là 31908
```
[] kubectl describe services/kubernetes-bootcamp

Name:                     kubernetes-bootcamp
Namespace:                default
Labels:                   app=kubernetes-bootcamp
Annotations:              <none>
Selector:                 app=kubernetes-bootcamp
Type:                     NodePort
IP:                       10.102.68.67
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  31908/TCP
Endpoints:                192.168.196.147:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```


- Thực hiện curl vào địa chỉ đã được expose vào một địa chỉ IP Node
```
[] curl 10.10.204.58:31908
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-6f6656d949-4vhkq | v=1
```

- Scale up app
```
kubectl scale deployments/kubernetes-bootcamp --replicas=2
```

- Xem danh sách deployment
```
kubectl get deployments
```

- Scale down app
```
kubectl scale deployments/kubernetes-bootcamp --replicas=2

```

- Rolling update cho Pod
```
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```