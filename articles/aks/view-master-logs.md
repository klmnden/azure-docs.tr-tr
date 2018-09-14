---
title: Azure Kubernetes Service (AKS) denetleyicisi günlüklerini görüntüle
description: Azure Kubernetes Service (AKS) için ana düğüm Kubernetes günlükleri görüntülemek ve etkinleştirme hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 07/26/2018
ms.author: iainfou
ms.openlocfilehash: 50de43fd6f9ca579b501c47514c9f8fca4f53ae8
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45540977"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Kubernetes Azure Kubernetes Service (AKS) ana düğüm günlüklerini gözden geçirin ve etkinleştirin

Azure Kubernetes Service (AKS) ile ana bileşenleri gibi *kube-apiserver* ve *kube Denetleyici Yöneticisi* yönetilen bir hizmet olarak sağlanır. Oluşturur ve yönetirken çalıştıran düğümlere *kubelet* ve kapsayıcı çalışma zamanı ve yönetilen Kubernetes API sunucusu üzerinden uygulamalarınızı dağıtın. Uygulama ve hizmetlerin gidermeye yardımcı olmak için bu ana bileşenler tarafından oluşturulan günlükleri görüntülemek gerekebilir. Bu makalede Azure Log Analytics etkinleştirmek ve Kubernetes ana bileşenleri günlüklerinden sorgulamak için nasıl kullanılacağı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makale, Azure hesabınızda çalışan mevcut bir AKS kümesi gerekir. Kullanarak bir AKS kümesi zaten yoksa, oluşturma [Azure CLI] [ cli-quickstart] veya [Azure portalında][portal-quickstart]. Log Analytics hem RBAC ile çalışır ve AKS küme RBAC olmayan etkin.

## <a name="enable-diagnostics-logs"></a>Tanılama günlüklerini etkinleştirme

Toplama ve birden çok kaynaktan veri gözden yardımcı olmak için Log Analytics etkilendiğine ortamınıza bir sorgu dili ve analiz altyapısı sağlar. Bir çalışma alanı collate ve verileri analiz etmek için kullanılır ve Application Insights ve Güvenlik Merkezi gibi diğer Azure hizmetleriyle tümleştirebilirsiniz. Bu günlükleri analiz etmek için farklı bir platform kullanmak için bunun yerine bir Azure depolama hesabına veya olay hub'ına tanılama günlükleri göndermek seçebilirsiniz. Daha fazla bilgi için [Azure Log Analytics nedir?] [log-analytics-overview].

Log Analytics etkin ve Azure portalında yönetilir. Kubernetes AKS kümenizde ana bileşenleri için günlük toplamayı etkinleştirmek için Azure portalında bir web tarayıcısında açın ve aşağıdaki adımları tamamlayın:

1. AKS kümenizi için kaynak grubunu seçin *myResourceGroup*. Gibi tek tek AKS kümesi kaynakları içeren kaynak grubunu seçmeyin *MC_myResourceGroup_myAKSCluster_eastus*.
1. Sol tarafındaki seçin **tanılama ayarları**.
1. AKS kümenizi gibi seçin *myAKSCluster*, sonra tercih **tanılamayı Aç**.
1. Gibi bir ad girin *myAKSLogs*, ardından seçeneğini **Log Analytics'e gönderme**.
    * Tercih *yapılandırma* Log Analytics, mevcut bir çalışma alanını seçin veya **yeni çalışma alanı oluştur**.
    * Bir çalışma alanı oluşturmanız gerekiyorsa, bir ad, bir kaynak grubu ve bir konum sağlayın.
1. Kullanılabilir günlükleri listesinde gibi etkinleştirmek istediğiniz günlükleri seçin *kube-apiserver*, *kube Denetleyici Yöneticisi*, ve *kube-Zamanlayıcı*. Dönün ve Log Analytics etkinleştirildikten sonra toplanan günlükleri değiştirin.
1. Hazır olduğunuzda seçin **Kaydet** seçili günlüklerin toplanmasını etkinleştirmek için.

Aşağıdaki örnekte portalı ekran görüntüsü gösterildiği *tanılama ayarları* penceresi ve OMS çalışma alanı oluşturma seçeneği:

![OMS çalışma alanı, Log Analytics AKS kümesi için etkinleştirme](media/view-master-logs/enable-oms-log-analytics.png)

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>Bir AKS kümesi test pod zamanlama

Bazı günlükler oluşturmak için yeni bir pod AKS kümenizi oluşturun. Aşağıdaki örnek YAML bildiriminin, temel bir NGINX örneği oluşturmak için kullanılabilir. Adlı bir dosya oluşturun `nginx.yaml` düzenleyicide seçim ve aşağıdaki içeriği yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: myfrontend
    image: nginx
    ports:
    - containerPort: 80
```

Pod ile oluşturma [kubectl oluşturma] [ kubectl-create] komutunu ve aşağıdaki örnekte gösterildiği gibi YAML dosyası belirtin:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Toplanan günlükleri görüntüleyin

Bu OMS çalışma alanınızda görünür ve etkin tanılama günlükleri için birkaç dakika sürebilir. Azure portalında Log Analytics çalışma alanınız için kaynak grubu gibi seçin *myResourceGroup*, Log Analytics kaynağınızı gibi ardından *myAKSLogs*.

![AKS kümenizin Log Analytics çalışma alanını seçin](media/view-master-logs/select-log-analytics-workspace.png)

Sol tarafındaki seçin **günlük araması**. Görüntülenecek *kube-apiserver*, metin kutusuna aşağıdaki sorguyu girin:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| project log_s
```

Çok sayıda günlüğü API sunucusu için büyük olasılıkla döndürülür. Önceki adımda oluşturduğunuz NGINX pod hakkında günlükleri görüntülemek için sorguyu aşağı kapsamını belirlemek için ek bir ekleme *burada* aramak için deyimi *pod'ların / nginx* aşağıdaki örnek sorguda gösterildiği gibi:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| where log_s contains "pods/nginx"
| project log_s
```

Aşağıdaki örnek ekran görüntüsünde gösterildiği gibi NGINX pod için özel günlüklerin görüntülenir:

![Log Analytics sorgu sonuçları için örnek NGINX pod](media/view-master-logs/log-analytics-query-results.png)

Ek günlükleri görüntülemek için sorguyu güncelleştirebilir *kategori* için ad *kube Denetleyici Yöneticisi* veya *kube-Zamanlayıcı*, ne bağlı olarak ek, günlükleri etkinleştirin. Ek *burada* deyimleri ardından aradığınız olayları iyileştirmek için kullanılabilir.

Sorgulamak ve günlük verilerinizi filtrelemek hakkında daha fazla bilgi için bkz. [görünümü veya Log Analytics günlük araması ile toplanan verileri analiz etmek][analyze-log-analytics].

## <a name="log-event-schema"></a>Günlüğü olay şeması

Günlük verilerini analiz etmek amacıyla, aşağıdaki tabloda her olay için kullanılan şemayı ayrıntıları:

| Alan adı               | Açıklama |
|--------------------------|-------------|
| *resourceId*             | Günlük üretilen azure kaynağı |
| *saat*                   | Günlük karşıya, zaman damgası |
| *Kategori*               | Kapsayıcı/bileşenin günlük oluşturma adı |
| *OperationName*          | Her zaman *Microsoft.ContainerService/managedClusters/diagnositicLogs/Read* |
| *Properties.log*         | Tam metin bileşeni günlüğü |
| *Properties.Stream*      | *stderr* veya *stdout* |
| *Properties.pod*         | Günlük geldiği pod adı |
| *properties.containerID* | Bu günlük geldiği docker kapsayıcının kimliği |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, etkinleştirmek ve Kubernetes AKS kümenizde ana bileşenleri için günlükleri gözden öğrendiniz. İzleme ve daha fazla sorun giderme için ayrıca [Kubelet günlüklerini görüntüleme] [ kubelet-logs] ve [SSH düğümü erişimi etkinleştirmek][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../log-analytics/log-analytics-overview.md
[analyze-log-analytics]: ../log-analytics/log-analytics-tutorial-viewdata.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
