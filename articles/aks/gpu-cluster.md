---
title: Azure Kubernetes Service'i (AKS) üzerinde GPU kullanma
description: Gpu'lar, yüksek performanslı işlem veya grafik kullanımlı iş yükleri Azure Kubernetes Service'teki (AKS) kullanmayı öğrenin
services: container-service
author: zr-msft
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/16/2019
ms.author: zarhoads
ms.openlocfilehash: c92762b53b0f5b50ea08f2f78998a3ccecbed990
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061062"
---
# <a name="use-gpus-for-compute-intensive-workloads-on-azure-kubernetes-service-aks"></a>Yoğun işlem gücü kullanımlı iş yükleri Azure Kubernetes Service'teki (AKS) için GPU'ları kullanın

Grafik işlem birimleri (Gpu'lar), genellikle grafik ve görselleştirme iş yükleri gibi yoğun işlem gücü kullanımlı iş yükleri için kullanılır. AKS, bu yoğun işlem gücü kullanımlı iş yükleri Kubernetes'te çalıştırma için GPU etkin düğüm havuzları oluşturmayı destekler. Kullanılabilir GPU özellikli VM'ler hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş Azure VM boyutları][gpu-skus]. AKS düğümleri için boyutu en az öneririz *işler için standart_nc6*.

> [!NOTE]
> GPU özellikli VM'ler, daha yüksek fiyatlandırma ve bölge kullanılabilirliği tabi olan özel bir donanım içerir. Daha fazla bilgi için [fiyatlandırma] [ azure-pricing] aracı ve [bölge kullanılabilirliği][azure-availability].

Şu anda GPU etkin düğüm havuzları kullanarak yalnızca Linux düğüm havuzları için kullanılabilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Gpu'lar destekleyen düğümleri ile var olan bir AKS kümesi olduğunu varsayar. Kubernetes AKS kümenizi 1.10 veya üzeri çalıştırmanız gerekir. Bu makalenin ilk bölümünde bu gereksinimleri karşılayan bir AKS kümesi gerekirse bkz [AKS kümesi oluşturma](#create-an-aks-cluster).

Ayrıca Azure CLI Sürüm 2.0.64 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

(GPU etkin düğüm ve Kubernetes 1.10 veya sonraki bir sürümü) en düşük gereksinimlerini karşılayan bir AKS kümesi gerekirse, aşağıdaki adımları tamamlayın. Bu gereksinimleri karşılayan bir AKS kümesi zaten varsa [sonraki bölüme atlayın](#confirm-that-gpus-are-schedulable).

İlk olarak kullanarak küme için bir kaynak grubu oluşturun [az grubu oluşturma] [ az-group-create] komutu. Aşağıdaki örnek, bir kaynak grubu adı oluşturur *myResourceGroup* içinde *eastus* bölgesi:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Şimdi kullanarak bir AKS kümesi oluşturma [az aks oluşturma] [ az-aks-create] komutu. Aşağıdaki örnek, tek bir düğüm boyutu ile bir küme oluşturur `Standard_NC6`:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-vm-size Standard_NC6 \
    --node-count 1
```

Kimlik bilgilerini kullanarak AKS kümenizin için alma [az aks get-credentials] [ az-aks-get-credentials] komutu:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="install-nvidia-drivers"></a>NVIDIA sürücüleri yükleyin

GPU'ları düğüm içinde kullanılabilmesi için önce bir DaemonSet NVIDIA cihaz eklentisi için dağıtmanız gerekir. Bu DaemonSet bir pod GPU'ları için gerekli sürücüleri sağlamak için her düğümünde çalıştırılır.

İlk olarak kullanarak bir ad oluşturun [kubectl ad alanı oluşturma] [ kubectl-create] komutu gibi *gpu kaynakları*:

```console
kubectl create namespace gpu-resources
```

Adlı bir dosya oluşturun *cihaz eklentisi ds.yaml NVIDIA* aşağıdaki YAML bildirimi yapıştırın. Bu bildirimi bir parçası olarak sağlanan [Kubernetes projesi için NVIDIA cihaz eklentisi][nvidia-github].

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: nvidia-device-plugin-daemonset
  namespace: kube-system
spec:
  updateStrategy:
    type: RollingUpdate
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
      - key: nvidia.com/gpu
        operator: Exists
        effect: NoSchedule
      containers:
      - image: nvidia/k8s-device-plugin:1.11
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
```

Artık [kubectl uygulamak] [ kubectl-apply] DaemonSet oluşturun ve NVIDIA cihaz eklentisi başarıyla, aşağıdaki örnek çıktıda gösterildiği gibi oluşturduğunuz onaylamak için komut:

```console
$ kubectl apply -f nvidia-device-plugin-ds.yaml

daemonset "nvidia-device-plugin" created
```

## <a name="confirm-that-gpus-are-schedulable"></a>GPU zamanlanabilir olduğundan emin olun

AKS kümesi oluşturduğunuz, Gpu'lar Kubernetes'te zamanlanabilir olduğunu onaylayın. İlk olarak kullanarak küme düğümleri listesinde [kubectl alma düğümleri] [ kubectl-get] komutu:

```console
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-28993262-0   Ready    agent   13m   v1.12.7
```

Artık [kubectl açıklayan düğüm] [ kubectl-describe] GPU'ları zamanlanabilir olduğunu onaylamak için komutu. Altında *kapasite* GPU bölümünde listesi olarak `nvidia.com/gpu:  1`.

Aşağıdaki sıkıştırılmış örneğe bir GPU adlı düğüm üzerinde kullanılabilir olduğunu gösterir. *aks nodepool1 18821093 0*:

```console
$ kubectl describe node aks-nodepool1-28993262-0

Name:               aks-nodepool1-28993262-0
Roles:              agent
Labels:             accelerator=nvidia

[...]

Capacity:
 attachable-volumes-azure-disk:  24
 cpu:                            6
 ephemeral-storage:              101584140Ki
 hugepages-1Gi:                  0
 hugepages-2Mi:                  0
 memory:                         57713784Ki
 nvidia.com/gpu:                 1
 pods:                           110
Allocatable:
 attachable-volumes-azure-disk:  24
 cpu:                            5916m
 ephemeral-storage:              93619943269
 hugepages-1Gi:                  0
 hugepages-2Mi:                  0
 memory:                         51702904Ki
 nvidia.com/gpu:                 1
 pods:                           110
System Info:
 Machine ID:                 b0cd6fb49ffe4900b56ac8df2eaa0376
 System UUID:                486A1C08-C459-6F43-AD6B-E9CD0F8AEC17
 Boot ID:                    f134525f-385d-4b4e-89b8-989f3abb490b
 Kernel Version:             4.15.0-1040-azure
 OS Image:                   Ubuntu 16.04.6 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://1.13.1
 Kubelet Version:            v1.12.7
 Kube-Proxy Version:         v1.12.7
PodCIDR:                     10.244.0.0/24
ProviderID:                  azure:///subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Compute/virtualMachines/aks-nodepool1-28993262-0
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                     CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                     ------------  ----------  ---------------  -------------  ---
  kube-system                nvidia-device-plugin-daemonset-bbjlq     0 (0%)        0 (0%)      0 (0%)           0 (0%)         2m39s

[...]
```

## <a name="run-a-gpu-enabled-workload"></a>GPU özellikli bir iş yükü çalıştırma

GPU iş başında görmek için uygun kaynak isteğiyle GPU özellikli bir iş yükü zamanlayın. Bu örnekte, çalıştıralım bir [Tensorflow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners) karşı iş [MNIST dataset](http://yann.lecun.com/exdb/mnist/).

Adlı bir dosya oluşturun *tf mnıst demo.yaml örnekleri* aşağıdaki YAML bildirimi yapıştırın. Kaynak sınırı aşağıdaki iş bildirimi içeren `nvidia.com/gpu: 1`:

> [!NOTE]
> CUDA sürücü sürümü CUDA çalışma zamanı sürümü için yeterli değil gibi bir türde bir eşleşmeme hatası sürücüleri çağırırken alırsanız, NVIDIA sürücüsü matris uyumluluk grafiği gözden geçirin- [https://docs.nvidia.com/deploy/cuda-compatibility/index.html](https://docs.nvidia.com/deploy/cuda-compatibility/index.html)

```yaml
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

Kullanım [kubectl uygulamak] [ kubectl-apply] işi çalıştırmak için komutu. Bu komut bildirim dosyasını ayrıştırır ve tanımlanmış Kubernetes nesnelerini oluşturur:

```console
kubectl apply -f samples-tf-mnist-demo.yaml
```

## <a name="view-the-status-and-output-of-the-gpu-enabled-workload"></a>Durum ve GPU özellikli iş yükü çıkışını görüntüleyin

Kullanarak iş ilerleme durumunu izlemek [kubectl işleri Al] [ kubectl-get] komutunu `--watch` bağımsız değişken. Görüntünün ilk istek için birkaç dakika sürebilir ve veri kümesi işlem. Zaman *TAMAMLAMALARI* sütun gösterir *1/1*, işi başarıyla tamamlandı. Çıkış `kubetctl --watch` komutunu *Ctrl-C*:

```console
$ kubectl get jobs samples-tf-mnist-demo --watch

NAME                    COMPLETIONS   DURATION   AGE

samples-tf-mnist-demo   0/1           3m29s      3m29s
samples-tf-mnist-demo   1/1   3m10s   3m36s
```

GPU özellikli iş yükü çıktısına aramak için ilk ile pod adını alın [kubectl pod'ları alma] [ kubectl-get] komutu:

```console
$ kubectl get pods --selector app=samples-tf-mnist-demo

NAME                          READY   STATUS      RESTARTS   AGE
samples-tf-mnist-demo-mtd44   0/1     Completed   0          4m39s
```

Artık [kubectl günlükleri] [ kubectl-logs] pod günlükleri görüntülemek için komutu. Aşağıdaki örnek pod günlükleri uygun bir GPU cihazında bulunan onaylayın `Tesla K80`. Kendi pod adını sağlayın:

```console
$ kubectl logs samples-tf-mnist-demo-smnr6

2019-05-16 16:08:31.258328: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2019-05-16 16:08:31.396846: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties: 
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: 2fd7:00:00.0
totalMemory: 11.17GiB freeMemory: 11.10GiB
2019-05-16 16:08:31.396886: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: 2fd7:00:00.0, compute capability: 3.7)
2019-05-16 16:08:36.076962: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.1081
Accuracy at step 10: 0.7457
Accuracy at step 20: 0.8233
Accuracy at step 30: 0.8644
Accuracy at step 40: 0.8848
Accuracy at step 50: 0.8889
Accuracy at step 60: 0.8898
Accuracy at step 70: 0.8979
Accuracy at step 80: 0.9087
Accuracy at step 90: 0.9099
Adding run metadata for 99
Accuracy at step 100: 0.9125
Accuracy at step 110: 0.9184
Accuracy at step 120: 0.922
Accuracy at step 130: 0.9161
Accuracy at step 140: 0.9219
Accuracy at step 150: 0.9151
Accuracy at step 160: 0.9199
Accuracy at step 170: 0.9305
Accuracy at step 180: 0.9251
Accuracy at step 190: 0.9258
Adding run metadata for 199
Accuracy at step 200: 0.9315
Accuracy at step 210: 0.9361
Accuracy at step 220: 0.9357
Accuracy at step 230: 0.9392
Accuracy at step 240: 0.9387
Accuracy at step 250: 0.9401
Accuracy at step 260: 0.9398
Accuracy at step 270: 0.9407
Accuracy at step 280: 0.9434
Accuracy at step 290: 0.9447
Adding run metadata for 299
Accuracy at step 300: 0.9463
Accuracy at step 310: 0.943
Accuracy at step 320: 0.9439
Accuracy at step 330: 0.943
Accuracy at step 340: 0.9457
Accuracy at step 350: 0.9497
Accuracy at step 360: 0.9481
Accuracy at step 370: 0.9466
Accuracy at step 380: 0.9514
Accuracy at step 390: 0.948
Adding run metadata for 399
Accuracy at step 400: 0.9469
Accuracy at step 410: 0.9489
Accuracy at step 420: 0.9529
Accuracy at step 430: 0.9507
Accuracy at step 440: 0.9504
Accuracy at step 450: 0.951
Accuracy at step 460: 0.9512
Accuracy at step 470: 0.9539
Accuracy at step 480: 0.9533
Accuracy at step 490: 0.9494
Adding run metadata for 499
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan ilişkili Kubernetes nesnelerini kaldırmak için [kubectl silme işi] [ kubectl delete] komutuyla şu şekilde:

```console
kubectl delete jobs samples-tf-mnist-demo
```

## <a name="next-steps"></a>Sonraki adımlar

Apache Spark işleri çalıştırma hakkında bilgi için bkz: [AKS çalıştırma Apache Spark işleri][aks-spark].

Azure'da Kubernetes machine learning (ML) iş yükleri çalıştırma hakkında daha fazla bilgi için bkz. [Kubeflow Labs][kubeflow-labs].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubeflow-labs]: https://github.com/Azure/kubeflow-labs
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[azure-pricing]: https://azure.microsoft.com/pricing/
[azure-availability]: https://azure.microsoft.com/global-infrastructure/services/
[nvidia-github]: https://github.com/NVIDIA/k8s-device-plugin

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-spark]: spark-job.md
[gpu-skus]: ../virtual-machines/linux/sizes-gpu.md
[install-azure-cli]: /cli/azure/install-azure-cli
