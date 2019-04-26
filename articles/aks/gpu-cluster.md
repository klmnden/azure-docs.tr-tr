---
title: Azure Kubernetes Service'i (AKS) üzerinde GPU kullanma
description: Gpu'lar, yüksek performanslı işlem veya grafik kullanımlı iş yükleri Azure Kubernetes Service'teki (AKS) kullanmayı öğrenin
services: container-service
author: zr-msft
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/28/2019
ms.author: zarhoads
ms.openlocfilehash: 150eaa6a4df558ed0c737d99cbcc8010baf63e96
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466401"
---
# <a name="use-gpus-for-compute-intensive-workloads-on-azure-kubernetes-service-aks"></a>Yoğun işlem gücü kullanımlı iş yükleri Azure Kubernetes Service'teki (AKS) için GPU'ları kullanın

Grafik işlem birimleri (Gpu'lar), genellikle grafik ve görselleştirme iş yükleri gibi yoğun işlem gücü kullanımlı iş yükleri için kullanılır. AKS, bu yoğun işlem gücü kullanımlı iş yükleri Kubernetes'te çalıştırma için GPU etkin düğüm havuzları oluşturmayı destekler. Kullanılabilir GPU özellikli VM'ler hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş Azure VM boyutları][gpu-skus]. AKS düğümleri için boyutu en az öneririz *işler için standart_nc6*.

> [!NOTE]
> GPU özellikli VM'ler, daha yüksek fiyatlandırma ve bölge kullanılabilirliği tabi olan özel bir donanım içerir. Daha fazla bilgi için [fiyatlandırma] [ azure-pricing] aracı ve [bölge kullanılabilirliği][azure-availability].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Gpu'lar destekleyen düğümleri ile var olan bir AKS kümesi olduğunu varsayar. Kubernetes AKS kümenizi 1.10 veya üzeri çalıştırmanız gerekir. Bu makalenin ilk bölümünde bu gereksinimleri karşılayan bir AKS kümesi gerekirse bkz [AKS kümesi oluşturma](#create-an-aks-cluster).

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

(GPU etkin düğüm ve Kubernetes 1.10 veya sonraki bir sürümü) en düşük gereksinimlerini karşılayan bir AKS kümesi gerekirse, aşağıdaki adımları tamamlayın. Bu gereksinimleri karşılayan bir AKS kümesi zaten varsa [sonraki bölüme atlayın](#confirm-that-gpus-are-schedulable).

İlk olarak kullanarak küme için bir kaynak grubu oluşturun [az grubu oluşturma] [ az-group-create] komutu. Aşağıdaki örnek, bir kaynak grubu adı oluşturur *myResourceGroup* içinde *eastus* bölgesi:

```azurecli
az group create --name myResourceGroup --location eastus
```

Şimdi kullanarak bir AKS kümesi oluşturma [az aks oluşturma] [ az-aks-create] komutu. Aşağıdaki örnek, tek bir düğüm boyutu ile bir küme oluşturur `Standard_NC6`, ve Kubernetes sürümü 1.11.7 çalıştırır:

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-vm-size Standard_NC6 \
    --node-count 1 \
    --kubernetes-version 1.11.8
```

Kimlik bilgilerini kullanarak AKS kümenizin için alma [az aks get-credentials] [ az-aks-get-credentials] komutu:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="confirm-that-gpus-are-schedulable"></a>GPU zamanlanabilir olduğundan emin olun

AKS kümesi oluşturduğunuz, Gpu'lar Kubernetes'te zamanlanabilir olduğunu onaylayın. İlk olarak kullanarak küme düğümleri listesinde [kubectl alma düğümleri] [ kubectl-get] komutu:

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-28993262-0   Ready    agent   6m    v1.11.7
```

Artık [kubectl açıklayan düğüm] [ kubectl-describe] GPU'ları zamanlanabilir olduğunu onaylamak için komutu. Altında *kapasite* GPU bölümünde listesi olarak `nvidia.com/gpu:  1`. GPU'ları görmüyorsanız bkz [sorun giderme GPU kullanılabilirlik](#troubleshoot-gpu-availability) bölümü.

Aşağıdaki sıkıştırılmış örneğe bir GPU adlı düğüm üzerinde kullanılabilir olduğunu gösterir. *aks nodepool1 18821093 0*:

```
$ kubectl describe node aks-nodepool1-28993262-0

Name:               aks-nodepool1-28993262-0
Roles:              agent
Labels:             accelerator=nvidia

[...]

Capacity:
 cpu:                6
 ephemeral-storage:  30428648Ki
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             57713780Ki
 nvidia.com/gpu:     1
 pods:               110
Allocatable:
 cpu:                5916m
 ephemeral-storage:  28043041951
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             52368500Ki
 nvidia.com/gpu:     1
 pods:               110
System Info:
 Machine ID:                 9148b74152374d049a68436ac59ee7c7
 System UUID:                D599728C-96F3-B941-BC79-E0B70453609C
 Boot ID:                    a2a6dbc3-6090-4f54-a2b7-7b4a209dffaf
 Kernel Version:             4.15.0-1037-azure
 OS Image:                   Ubuntu 16.04.5 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://1.13.1
 Kubelet Version:            v1.11.7
 Kube-Proxy Version:         v1.11.7
PodCIDR:                     10.244.0.0/24
ProviderID:                  azure:///subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Compute/virtualMachines/aks-nodepool1-28993262-0
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                     CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                     ------------  ----------  ---------------  -------------  ---
  gpu-resources              nvidia-device-plugin-97zfc               0 (0%)        0 (0%)      0 (0%)           0 (0%)         2m4s

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

Kullanarak iş ilerleme durumunu izlemek [kubectl işleri Al] [ kubectl-get] komutunu `--watch` bağımsız değişken. Görüntünün ilk istek için birkaç dakika sürebilir ve veri kümesi işlem. Zaman *TAMAMLAMALARI* sütun gösterir *1/1*, işi başarıyla tamamlandı:

```
$ kubectl get jobs samples-tf-mnist-demo --watch

NAME                    COMPLETIONS   DURATION   AGE

samples-tf-mnist-demo   0/1           3m29s      3m29s
samples-tf-mnist-demo   1/1   3m10s   3m36s
```

GPU özellikli iş yükü çıktısına aramak için ilk ile pod'ların adını alın [kubectl pod'ları alma] [ kubectl-get] komutu:

```
$ kubectl get pods --selector app=samples-tf-mnist-demo

NAME                          READY   STATUS      RESTARTS   AGE
samples-tf-mnist-demo-smnr6   0/1     Completed   0          3m
```

Artık [kubectl günlükleri] [ kubectl-logs] pod günlükleri görüntülemek için komutu. Aşağıdaki örnek pod günlükleri uygun bir GPU cihazında bulunan onaylayın `Tesla K80`. Kendi pod adını sağlayın:

```
$ kubectl logs samples-tf-mnist-demo-smnr6

2019-02-28 23:47:34.749013: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2019-02-28 23:47:34.879877: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: 3130:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2019-02-28 23:47:34.879915: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: 3130:00:00.0, compute capability: 3.7)
2019-02-28 23:47:39.492532: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
Accuracy at step 40: 0.8685
Accuracy at step 50: 0.8864
Accuracy at step 60: 0.901
Accuracy at step 70: 0.905
Accuracy at step 80: 0.9103
Accuracy at step 90: 0.9126
Adding run metadata for 99
Accuracy at step 100: 0.9176
Accuracy at step 110: 0.9149
Accuracy at step 120: 0.9187
Accuracy at step 130: 0.9253
Accuracy at step 140: 0.9252
Accuracy at step 150: 0.9266
Accuracy at step 160: 0.9255
Accuracy at step 170: 0.9267
Accuracy at step 180: 0.9257
Accuracy at step 190: 0.9309
Adding run metadata for 199
Accuracy at step 200: 0.9272
Accuracy at step 210: 0.9321
Accuracy at step 220: 0.9343
Accuracy at step 230: 0.9388
Accuracy at step 240: 0.9408
Accuracy at step 250: 0.9394
Accuracy at step 260: 0.9412
Accuracy at step 270: 0.9422
Accuracy at step 280: 0.9436
Accuracy at step 290: 0.9411
Adding run metadata for 299
Accuracy at step 300: 0.9426
Accuracy at step 310: 0.9466
Accuracy at step 320: 0.9458
Accuracy at step 330: 0.9407
Accuracy at step 340: 0.9445
Accuracy at step 350: 0.9486
Accuracy at step 360: 0.9475
Accuracy at step 370: 0.948
Accuracy at step 380: 0.9516
Accuracy at step 390: 0.9534
Adding run metadata for 399
Accuracy at step 400: 0.9501
Accuracy at step 410: 0.9552
Accuracy at step 420: 0.9535
Accuracy at step 430: 0.9545
Accuracy at step 440: 0.9533
Accuracy at step 450: 0.9526
Accuracy at step 460: 0.9566
Accuracy at step 470: 0.9547
Accuracy at step 480: 0.9548
Accuracy at step 490: 0.9545
Adding run metadata for 499
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan ilişkili Kubernetes nesnelerini kaldırmak için [kubectl silme işi] [ kubectl delete] komutuyla şu şekilde:

```console
kubectl delete jobs samples-tf-mnist-demo
```

## <a name="troubleshoot-gpu-availability"></a>GPU kullanılabilirlik sorunlarını giderme

GPU'ları, düğümlerinde kullanılabilir olacak şekilde görmüyorsanız, NVIDIA cihaz eklentisi için bir DaemonSet dağıtma gerekebilir. Bu DaemonSet bir pod GPU'ları için gerekli sürücüleri sağlamak için her düğümünde çalıştırılır.

İlk olarak kullanarak bir ad oluşturun [kubectl ad alanı oluşturma] [ kubectl-create] komutu gibi *gpu kaynakları*:

```console
kubectl create namespace gpu-resources
```

Adlı bir dosya oluşturun *cihaz eklentisi ds.yaml NVIDIA* aşağıdaki YAML bildirimi yapıştırın. Güncelleştirme `image: nvidia/k8s-device-plugin:1.11` yarı-Kubernetes sürümünüzü eşleştirmek için bildirimi basılı yolu. AKS kümenizi Kubernetes sürümü 1.12 çalıştırıyorsa, örneğin, etikete güncelleştirme `image: nvidia/k8s-device-plugin:1.12`.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  labels:
    kubernetes.io/cluster-service: "true"
  name: nvidia-device-plugin
  namespace: gpu-resources
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
      - image: nvidia/k8s-device-plugin:1.11 # Update this tag to match your Kubernetes version
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

Artık [kubectl uygulamak] [ kubectl-apply] komutunu DaemonSet oluşturun:

```
$ kubectl apply -f nvidia-device-plugin-ds.yaml

daemonset "nvidia-device-plugin" created
```

Çalıştırma [kubectl düğüm tanımlamak] [ kubectl-describe] yeniden GPU düğüm üzerinde kullanılabilir olduğunu doğrulamak için komutu.

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

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-spark]: spark-job.md
[gpu-skus]: ../virtual-machines/linux/sizes-gpu.md
[install-azure-cli]: /cli/azure/install-azure-cli
