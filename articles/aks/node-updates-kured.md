---
title: Güncelleştirin ve düğümleri kured Azure Kubernetes Service (AKS) ile yeniden Başlat
description: Düğümleri güncelleştirmek ve otomatik olarak kured Azure Kubernetes Service (AKS) ile yeniden hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: 75057f6bd92fbdc805da2e0e36dc2bff7b069f26
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60465003"
---
# <a name="apply-security-and-kernel-updates-to-nodes-in-azure-kubernetes-service-aks"></a>Düğümleri Azure Kubernetes Service (AKS) için geçerli güvenlik ve çekirdek güncelleştirmeleri

Kümelerinize korumak için güvenlik güncelleştirmeleri otomatik olarak AKS düğümleri için uygulanır. Bu güncelleştirmeleri, işletim sistemi güvenlik düzeltmeleri veya çekirdek güncelleştirmeleri içerir. Bazı güncelleştirmeler, işlemi tamamlamak için bir düğümü yeniden başlatma gerektirir. AKS düğümlerini güncelleştirme işlemini tamamlamak için otomatik olarak yeniden başlatmanız değil.

Bu makalede, açık kaynaklı kullanmayı gösterir [kured (KUbernetes yeniden arka plan programı)] [ kured] kullanılabilirliğiyle pod'ların ve düğümü yeniden başlatma çalıştıran, yeniden başlatma gerektiren sonra otomatik olarak düğümleri işlemek için izlemek için işlem.

> [!NOTE]
> `Kured` bir açık kaynak projesi tarafından Weaveworks ' dir. Destek aks'deki bu proje için bir en iyi çaba ilkesine göre sağlanır. Ek destek #weave topluluk slack kanalına bulunabilir,

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>AKS düğümü güncelleştirme deneyimini anlama

Bir AKS kümesinde, Kubernetes düğümleri Azure sanal makineleri (VM'ler) olarak çalıştırın. Bu Linux tabanlı sanal makineler ile işletim sistemi güncelleştirmeleri her gece otomatik olarak denetlemek için yapılandırılmış bir Ubuntu görüntüsünü kullanın. Güvenlik veya çekirdek güncelleştirmeleri varsa, bunlar otomatik olarak indirilip yüklendiği.

![AKS düğümü güncelleştirin ve kured işlemle yeniden başlatma](media/node-updates-kured/node-reboot-process.png)

Çekirdek güncelleştirmeleri gibi bazı güvenlik güncelleştirmeleri işlemi sonlandırmak için bir düğümü yeniden başlatma gerektirir. Yeniden başlatma gerektiren bir düğümü adlı bir dosya oluşturur */var/run/reboot-required*. Bu yeniden başlatma işlemi otomatik olarak gerçekleşmez.

Düğümü yeniden başlatma işlemlerini işlemek veya kullanmak için kendi iş akışları ve işlemleri kullanabilirsiniz `kured` işlemini düzenlemek için. İle `kured`, [DaemonSet] [ DaemonSet] dağıtılır, bir pod kümedeki her düğümünde çalıştırılır. DaemonSet bu pod'ların varlığını izleyin */var/run/reboot-required* dosyasını açın ve ardından bir işlem düğümlerini yeniden başlatmanız başlatır.

### <a name="node-upgrades"></a>Düğümde yükseltme

Olanak sağlayan, AKS içinde başka bir işlem yoktur *yükseltme* bir küme. Bir yükseltme genellikle ise daha yeni bir Kubernetes sürümüne taşımak, yalnızca düğümün güvenlik güncelleştirmelerini uygulamak için. Bir AKS upgrade komutunu aşağıdaki eylemleri gerçekleştirir:

* Yeni bir düğüm en son güvenlik güncelleştirmelerini ve uygulanan Kubernetes sürümü ile dağıtılır.
* Eski bir düğüm kordonlanır ve boşaltılır.
* Pod'ların yeni düğümde zamanlanır.
* Eski düğümü silinir.

Aynı Kubernetes sürümüne yükseltme sırasında olay kalamaz. Kubernetes daha yeni bir sürümü belirtmeniz gerekir. Kubernetes en son sürüme yükseltmek için [AKS kümenizi yükseltme][aks-upgrade].

## <a name="deploy-kured-in-an-aks-cluster"></a>Bir AKS kümesindeki kured dağıtma

Dağıtılacak `kured` DaemonSet, YAML bildirim kendi GitHub proje sayfasından aşağıdaki örnek uygulayın. Bu bildirim için bir rol ve küme rolünü, bağlamalar ve bir hizmet hesabı oluşturur, ardından bu DaemonSet kullanarak dağıtır `kured` 1.1.0 sürümü 1.9 veya üzeri AKS kümelerini destekler.

```console
kubectl apply -f https://github.com/weaveworks/kured/releases/download/1.1.0/kured-1.1.0.yaml
```

Ek parametreler için de yapılandırabilirsiniz `kured`, Prometheus veya Slack ile tümleştirme gibi. Ek yapılandırma parametreleri hakkında daha fazla bilgi için bkz. [kured yükleme docs][kured-install].

## <a name="update-cluster-nodes"></a>Küme düğümlerini güncelleştirme

Varsayılan olarak, güncelleştirmeleri tüm Akşam AKS düğümleri denetleyin. Beklemek istemiyorsanız, el ile denetlemek için bir güncelleştirme yapabilirsiniz `kured` düzgün çalışır. İlk olarak, adımları [AKS düğümlerinizi birinde SSH][aks-ssh]. Düğüm için bir SSH bağlantısı oluşturduktan sonra güncelleştirmeleri denetleyin ve bunları aşağıdaki gibi uygulayın:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Düğümü yeniden başlatma gerektiren güncelleştirmeler uygulandıysa, bir dosyaya yazılan */var/run/reboot-required*. `Kured` Varsayılan olarak 60 dakikada bir yeniden başlatma gerektiren bir düğüm olup olmadığını denetler.

## <a name="monitor-and-review-reboot-process"></a>İzleme ve yeniden başlatma işlemini gözden geçirin

DaemonSet içinde çoğaltmalarından birini bir düğümü yeniden başlatma gerekli olduğunu algıladığında, kilit Kubernetes API düğümünden yerleştirilir. Bu kilit düğümde zamanlanmasını ek pod'lar engeller. Kilit de aynı anda yalnızca bir düğümün başlatılması gösterir. Düğümle kordonlanır çalışan pod'ların düğümden boşaltılır ve düğüm yeniden.

Kullanarak düğümlerin durumunun izleyebilirsiniz [kubectl alma düğümleri] [ kubectl-get-nodes] komutu. Aşağıdaki örnek çıktıda bir düğüm durumu ile gösterilmektedir *SchedulingDisabled* düğümü yeniden başlatma işlemine hazırlanır gibi:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-28993262-0   Ready,SchedulingDisabled   agent     1h        v1.11.7
```

Güncelleştirme işlemi tamamlandıktan sonra kullanarak düğümlerin durumunun görüntüleyebileceğiniz [kubectl alma düğümleri] [ kubectl-get-nodes] komutunu `--output wide` parametresi. Bu ek çıkış bir fark görmenizi sağlayan *çekirdek sürümü* aşağıdaki örnek çıktıda gösterildiği gibi temel alınan düğümleri. *Aks nodepool1 28993262 0* bir önceki adımda ve gösterir çekirdek sürümü güncelleştirildiği *4.15.0-1039-azure*. Düğüm *aks nodepool1 28993262 1* , güncelleştirilmiş gösterir çekirdek sürümü olmamıştır *4.15.0-1037-azure*.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-28993262-0   Ready     agent     1h        v1.11.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS   4.15.0-1039-azure   docker://3.0.4
aks-nodepool1-28993262-1   Ready     agent     1h        v1.11.7   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1037-azure   docker://3.0.4
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede nasıl kullanılacağı ayrıntılı `kured` düğümleri güvenlik güncelleştirme işleminin bir parçası otomatik olarak yeniden başlatmak için. Kubernetes en son sürüme yükseltmek için [AKS kümenizi yükseltme][aks-upgrade].

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://github.com/weaveworks/kured#installation
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md
