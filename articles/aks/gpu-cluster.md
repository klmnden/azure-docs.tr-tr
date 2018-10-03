---
title: GPU'ları Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service'i (AKS) üzerinde GPU kullanma
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/05/2018
ms.author: laevenso
ms.custom: mvc
ms.openlocfilehash: 231d7b875a7163aaa532be4a6477ca4e2eb67286
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043671"
---
# <a name="using-gpus-on-aks"></a>AKS üzerinde GPU kullanma

AKS, GPU etkin düğüm havuzlarının oluşturulmasını destekler. Azure şu an için tekli veya çoklu GPU etkin VM'ler sunmaktadır. GPU etkin VM'ler işlemci, grafik ve görselleştirme talebi yoğun olan iş yükleri için tasarlanmıştır. GPU listesini özellikli VM'ler bulunabilir [burada](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

GPU'ları, genellikle grafik açısından yoğun gibi yoğun işlem gücü kullanımlı iş yükleri ve görselleştirme iş yükleri için gereklidir. Aşağıdaki başvuru [belge](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu) iş yükünüz için doğru VM boyutunu belirlemek için.
Boyutu en az öneririz `Standard_NC6` , Azure Kubernetes Service (AKS) düğümler için.

> [!NOTE]
> GPU etkin sanal makineleri fiyatlandırması ve bölge daha yüksek kullanılabilirlik tabi olan özel bir donanım içerir. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/) aracı ve [bölge kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/) site daha fazla bilgi için.


Bu en düşük öneri karşılayan bir AKS kümesi gerekirse, aşağıdaki komutları çalıştırın.

Küme için bir kaynak grubu oluşturun.

```azurecli
az group create --name myGPUCluster --location eastus
```

Boyutu düğümleri ile bir AKS kümesi oluşturma `Standard_NC6`.

```azurecli
az aks create --resource-group myGPUCluster --name myGPUCluster --node-vm-size Standard_NC6
```

AKS kümeye bağlanın.

```azurecli
az aks get-credentials --resource-group myGPUCluster --name myGPUCluster
```

## <a name="confirm-gpus-are-schedulable"></a>GPU zamanlanabilir onaylayın

GPU'ları Kubernetes zamanlanabilir onaylamak için aşağıdaki komutları çalıştırın.

Geçerli düğüm listesini alın.

```
$ kubectl get nodes
NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-22139053-0   Ready     agent     10h       v1.9.6
aks-nodepool1-22139053-1   Ready     agent     10h       v1.9.6
aks-nodepool1-22139053-2   Ready     agent     10h       v1.9.6
```

GPU'ları zamanlanabilir onaylamak için düğümlerinden biri açıklanmaktadır. Bu, altında bulunabilir `Capacity` bölümü. Örneğin, `nvidia.com/gpu:  1`. GPU'ları görmüyorsanız başvurun **sorun giderme** bölümüne bakın.

```
$ kubectl describe node aks-nodepool1-22139053-0
Name:               aks-nodepool1-22139053-0
Roles:              agent
Labels:             agentpool=nodepool1
                    beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=Standard_NC6
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/region=eastus
                    failure-domain.beta.kubernetes.io/zone=1
                    kubernetes.azure.com/cluster=MC_myGPUCluster_myGPUCluster
                    kubernetes.io/hostname=aks-nodepool1-22139053-0
                    kubernetes.io/role=agent
                    storageprofile=managed
                    storagetier=Standard_LRS
Annotations:        node.alpha.kubernetes.io/ttl=0
                    volumes.kubernetes.io/controller-managed-attach-detach=true
Taints:             <none>
CreationTimestamp:  Thu, 05 Apr 2018 12:13:20 -0700
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Thu, 05 Apr 2018 12:15:07 -0700   Thu, 05 Apr 2018 12:15:07 -0700   RouteCreated                 RouteController created a route
  OutOfDisk            False   Thu, 05 Apr 2018 22:14:33 -0700   Thu, 05 Apr 2018 12:13:20 -0700   KubeletHasSufficientDisk     kubelet has sufficient disk space available
  MemoryPressure       False   Thu, 05 Apr 2018 22:14:33 -0700   Thu, 05 Apr 2018 12:13:20 -0700   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Thu, 05 Apr 2018 22:14:33 -0700   Thu, 05 Apr 2018 12:13:20 -0700   KubeletHasNoDiskPressure     kubelet has no disk pressure
  Ready                True    Thu, 05 Apr 2018 22:14:33 -0700   Thu, 05 Apr 2018 12:15:10 -0700   KubeletReady                 kubelet is posting ready status. AppArmor enabled
Addresses:
  InternalIP:  10.240.0.4
  Hostname:    aks-nodepool1-22139053-0
Capacity:
 nvidia.com/gpu:                  1
 cpu:                             6
 memory:                          57691688Ki
 pods:                            110
Allocatable:
 nvidia.com/gpu:                  1
 cpu:                             6
 memory:                          57589288Ki
 pods:                            110
System Info:
 Machine ID:                 2eb0e90bd1fe450ba3cf83479443a511
 System UUID:                CFB485B6-CB49-A545-A2C9-8E4C592C3273
 Boot ID:                    fea24544-596d-4246-b8c3-610fc7ac7280
 Kernel Version:             4.13.0-1011-azure
 OS Image:                   Debian GNU/Linux 9 (stretch)
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://1.13.1
 Kubelet Version:            v1.9.6
 Kube-Proxy Version:         v1.9.6
PodCIDR:                     10.244.1.0/24
ExternalID:                  /subscriptions/8ecadfc9-d1a3-4ea4-b844-0d9f87e4d7c8/resourceGroups/MC_myGPUCluster_myGPUCluster/providers/Microsoft.Compute/virtualMachines/aks-nodepool1-22139053-0
Non-terminated Pods:         (2 in total)
  Namespace                  Name                       CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ---------                  ----                       ------------  ----------  ---------------  -------------
  kube-system                kube-proxy-pwffr           100m (1%)     0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-svc-redirect-mkpf4    0 (0%)        0 (0%)      0 (0%)           0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ------------  ----------  ---------------  -------------
  100m (1%)     0 (0%)      0 (0%)           0 (0%)
Events:         <none>
```

## <a name="run-a-gpu-enabled-workload"></a>GPU etkin iş yükü çalıştırma

GPU'ları gerçekten de çalıştığınız göstermek için uygun kaynak isteğiyle iş yükü zamanlama bir GPU etkin. Bu örnekte çalışacak bir [Tensorflow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners) karşı iş [MNIST dataset](http://yann.lecun.com/exdb/mnist/).

Kaynak sınırı aşağıdaki iş bildirimi içeren `nvidia.com/gpu: 1`. 

Bildirim kopyalayın ve Farklı Kaydet **tf mnıst demo.yaml örnekleri**.
```
apiVersion: batch/v1
kind: Job
metadata:
  labels:
    app: samples-tf-mnist-demo
  name: samples-tf-mnist-demo
spec:
  template:
    metadata:
      labels:
        app: samples-tf-mnist-demo
    spec:
      containers:
      - name: samples-tf-mnist-demo
        image: microsoft/samples-tf-mnist-demo:gpu
        args: ["--max_steps", "500"]
        imagePullPolicy: IfNotPresent
        resources:
          limits:
           nvidia.com/gpu: 1
      restartPolicy: OnFailure
```

Kullanım [kubectl uygulamak] [ kubectl-apply] işi çalıştırmak için komutu. Bu komut, bildirim dosyasını ayrıştırır ve tanımlanmış Kubernetes nesnelerini oluşturur.
```
$ kubectl apply -f samples-tf-mnist-demo.yaml
job "samples-tf-mnist-demo" created
```

Başarılı tamamlama kullanarak kadar işinin ilerleme durumunu izlemek [kubectl işleri Al] [ kubectl-get] komutunu `--watch` bağımsız değişken.
```
$ kubectl get jobs samples-tf-mnist-demo --watch
NAME                    DESIRED   SUCCESSFUL   AGE
samples-tf-mnist-demo   1         0            8s
samples-tf-mnist-demo   1         1            35s
```

Tamamlanan gösteren pod'ları tarafından günlükleri görüntülemek için pod adını belirleyin.
```
$ kubectl get pods --selector app=samples-tf-mnist-demo --show-all
NAME                          READY     STATUS      RESTARTS   AGE
samples-tf-mnist-demo-smnr6   0/1       Completed   0          4m
```

Yukarıdaki komut çıktısından pod adını kullanarak uygun bir GPU cihazında bu durumda bulunan olduğunu onaylamak için pod günlüklere bakın `Tesla K80`.
```
$ kubectl logs samples-tf-mnist-demo-smnr6
2018-04-13 04:11:08.710863: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2018-04-13 04:11:15.824349: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: 04e1:00:00.0
totalMemory: 11.17GiB freeMemory: 11.10GiB
2018-04-13 04:11:15.824394: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: 04e1:00:00.0, compute capability: 3.7)
2018-04-13 04:11:20.891910: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.0487
Accuracy at step 10: 0.6571
Accuracy at step 20: 0.8111
Accuracy at step 30: 0.8562
Accuracy at step 40: 0.8786
Accuracy at step 50: 0.8911
Accuracy at step 60: 0.8986
Accuracy at step 70: 0.9017
Accuracy at step 80: 0.9049
Accuracy at step 90: 0.9114
Adding run metadata for 99
Accuracy at step 100: 0.9109
Accuracy at step 110: 0.9143
Accuracy at step 120: 0.9188
Accuracy at step 130: 0.9194
Accuracy at step 140: 0.9237
Accuracy at step 150: 0.9231
Accuracy at step 160: 0.9158
Accuracy at step 170: 0.9259
Accuracy at step 180: 0.9303
Accuracy at step 190: 0.9315
Adding run metadata for 199
Accuracy at step 200: 0.9334
Accuracy at step 210: 0.9342
Accuracy at step 220: 0.9359
Accuracy at step 230: 0.9353
Accuracy at step 240: 0.933
Accuracy at step 250: 0.9353
Accuracy at step 260: 0.9408
Accuracy at step 270: 0.9396
Accuracy at step 280: 0.9406
Accuracy at step 290: 0.9444
Adding run metadata for 299
Accuracy at step 300: 0.9453
Accuracy at step 310: 0.946
Accuracy at step 320: 0.9464
Accuracy at step 330: 0.9472
Accuracy at step 340: 0.9516
Accuracy at step 350: 0.9473
Accuracy at step 360: 0.9502
Accuracy at step 370: 0.9483
Accuracy at step 380: 0.9481
Accuracy at step 390: 0.9467
Adding run metadata for 399
Accuracy at step 400: 0.9477
Accuracy at step 410: 0.948
Accuracy at step 420: 0.9496
Accuracy at step 430: 0.9501
Accuracy at step 440: 0.9534
Accuracy at step 450: 0.9551
Accuracy at step 460: 0.9518
Accuracy at step 470: 0.9562
Accuracy at step 480: 0.9583
Accuracy at step 490: 0.9575
Adding run metadata for 499
```

## <a name="cleanup"></a>Temizleme
Bu adımda oluşturduğunuz ilişkili Kubernetes nesnelerini kaldırın.
```
$ kubectl delete jobs samples-tf-mnist-demo
job "samples-tf-mnist-demo" deleted
```

## <a name="troubleshoot"></a>Sorun giderme

Bazı senaryolarda, kapasite GPU kaynaklarınıza göremeyebilirsiniz. Örneğin: sonra sürüm 1.10 Kubernetes Küme yükseltme veya yeni bir Kubernetes sürümü 1.10 küme, beklenen oluşturma `nvidia.com/gpu` kaynaktır eksik `Capacity` çalıştırırken `kubectl describe node <node-name>`. 

Görürsünüz bu sorunu çözmek için aşağıdaki daemonset post sağlama veya yükseltme, geçerli `nvidia.com/gpu` zamanlanabilir bir kaynak olarak. 

Bildirim kopyalayın ve Farklı Kaydet **cihaz eklentisi ds.yaml NVIDIA**. Görüntü etiketi için `image: nvidia/k8s-device-plugin:1.10` aşağıda etiketi Kubernetes sürümünüzü güncelleştirin. Örneğin, etiket kullanmak `1.11` Kubernetes sürümü 1.11 için.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  labels:
    kubernetes.io/cluster-service: "true"
  name: nvidia-device-plugin
  namespace: kube-system
spec:
  template:
    metadata:
      # Mark this pod as a critical add-on; when enabled, the critical add-on scheduler
      # reserves resources for critical add-on pods so that they can be rescheduled after
      # a failure.  This annotation works in tandem with the toleration below.
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ""
      labels:
        name: nvidia-device-plugin-ds
    spec:
      tolerations:
      # Allow this pod to be rescheduled while the node is in "critical add-ons only" mode.
      # This, along with the annotation above marks this pod as a critical add-on.
      - key: CriticalAddonsOnly
        operator: Exists
      containers:
      - image: nvidia/k8s-device-plugin:1.10 # Update this tag to match your Kubernetes version
        name: nvidia-device-plugin-ctr
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
        volumeMounts:
          - name: device-plugin
            mountPath: /var/lib/kubelet/device-plugins
      volumes:
        - name: device-plugin
          hostPath:
            path: /var/lib/kubelet/device-plugins
      nodeSelector:
        beta.kubernetes.io/os: linux
        accelerator: nvidia
```

Kullanım [kubectl uygulamak] [ kubectl-apply] komutunu daemonset oluşturun.

```
$ kubectl apply -f nvidia-device-plugin-ds.yaml
daemonset "nvidia-device-plugin" created
```

## <a name="next-steps"></a>Sonraki adımlar

Azure'da Kubernetes Machine Learning iş yükleri çalıştırma ister misiniz? Daha fazla ayrıntı için Kubeflow labs bakın.

> [!div class="nextstepaction"]
> [Kubeflow Laboratuvarları][kubeflow-labs]

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubeflow-labs]: https://github.com/Azure/kubeflow-labs
