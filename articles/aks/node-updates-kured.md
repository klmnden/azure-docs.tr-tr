---
title: Güncelleştirme ve Linux düğümleri kured Azure Kubernetes Service (AKS) ile yeniden Başlat
description: Linux düğümleri güncelleştirmek ve otomatik olarak kured Azure Kubernetes Service (AKS) ile yeniden hakkında bilgi edinin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 02/28/2019
ms.author: mlearned
ms.openlocfilehash: 580d1316c2bfc6514a148ed6fba78a8e77bd880e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614914"
---
# <a name="apply-security-and-kernel-updates-to-linux-nodes-in-azure-kubernetes-service-aks"></a>Linux düğümleri Azure Kubernetes Service (AKS) için geçerli güvenlik ve çekirdek güncelleştirmeleri

Kümelerinize korumak için güvenlik güncelleştirmeleri otomatik olarak aks'deki Linux düğümleri için uygulanır. Bu güncelleştirmeleri, işletim sistemi güvenlik düzeltmeleri veya çekirdek güncelleştirmeleri içerir. Bazı güncelleştirmeler, işlemi tamamlamak için bir düğümü yeniden başlatma gerektirir. AKS güncelleştirme işlemini tamamlamak için bu Linux düğümleri otomatik olarak yeniden değil.

Windows Server düğümleri (şu anda önizlemede aks'deki) güncel tutmak için işlemi biraz farklıdır. Windows Server düğümler günlük güncelleştirmeleri almaz. Bunun yerine, yeni düğümlerin en son temel Windows Server görüntüsünü ve düzeltme ekleri dağıtır AKS yükseltmesi gerçekleştirin. Windows Server düğümlerini kullanma AKS kümeler için bkz: [aks'deki bir düğüm havuzunu yükseltme][nodepool-upgrade].

Bu makalede, açık kaynaklı kullanmayı gösterir [kured (KUbernetes yeniden arka plan programı)][kured] yeniden başlatılmasını gerektirir ve otomatik pod'ların ve düğüm çalıştıran kullanılabilirliğiyle işlemek Linux düğümleri yeniden başlatma işlemi için izlemek için.

> [!NOTE]
> `Kured` bir açık kaynak projesi tarafından Weaveworks ' dir. Destek aks'deki bu proje için bir en iyi çaba ilkesine göre sağlanır. Ek destek #weave topluluk slack kanalına bulunabilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>AKS düğümü güncelleştirme deneyimini anlama

Bir AKS kümesinde, Kubernetes düğümleri Azure sanal makineleri (VM'ler) olarak çalıştırın. Bu Linux tabanlı sanal makineler ile işletim sistemi güncelleştirmeleri her gece otomatik olarak denetlemek için yapılandırılmış bir Ubuntu görüntüsünü kullanın. Güvenlik veya çekirdek güncelleştirmeleri varsa, bunlar otomatik olarak indirilip yüklendiği.

![AKS düğümü güncelleştirin ve kured işlemle yeniden başlatma](media/node-updates-kured/node-reboot-process.png)

Çekirdek güncelleştirmeleri gibi bazı güvenlik güncelleştirmeleri işlemi sonlandırmak için bir düğümü yeniden başlatma gerektirir. Yeniden başlatma gerektiren bir bir Linux düğümü adlı bir dosya oluşturur */var/run/reboot-required*. Bu yeniden başlatma işlemi otomatik olarak gerçekleşmez.

Düğümü yeniden başlatma işlemlerini işlemek veya kullanmak için kendi iş akışları ve işlemleri kullanabilirsiniz `kured` işlemini düzenlemek için. İle `kured`, [DaemonSet][DaemonSet] dağıtılır, bir pod kümedeki her Linux düğümünde çalıştırılır. DaemonSet bu pod'ların varlığını izleyin */var/run/reboot-required* dosyasını açın ve ardından bir işlem düğümlerini yeniden başlatmanız başlatır.

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
kubectl apply -f https://github.com/weaveworks/kured/releases/download/1.2.0/kured-1.2.0-dockerhub.yaml
```

Ek parametreler için de yapılandırabilirsiniz `kured`, Prometheus veya Slack ile tümleştirme gibi. Ek yapılandırma parametreleri hakkında daha fazla bilgi için bkz. [kured yükleme docs][kured-install].

## <a name="update-cluster-nodes"></a>Küme düğümlerini güncelleştirme

Varsayılan olarak, Linux düğümleri aks'deki tüm Akşam güncelleştirmeleri denetleyin. Beklemek istemiyorsanız, el ile denetlemek için bir güncelleştirme yapabilirsiniz `kured` düzgün çalışır. İlk olarak, adımları [AKS düğümlerinizi birinde SSH][aks-ssh]. Linux düğümü için bir SSH bağlantısı oluşturduktan sonra güncelleştirmeleri denetleyin ve bunları aşağıdaki gibi uygulayın:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Düğümü yeniden başlatma gerektiren güncelleştirmeler uygulandıysa, bir dosyaya yazılan */var/run/reboot-required*. `Kured` Varsayılan olarak 60 dakikada bir yeniden başlatma gerektiren bir düğüm olup olmadığını denetler.

## <a name="monitor-and-review-reboot-process"></a>İzleme ve yeniden başlatma işlemini gözden geçirin

DaemonSet içinde çoğaltmalarından birini bir düğümü yeniden başlatma gerekli olduğunu algıladığında, kilit Kubernetes API düğümünden yerleştirilir. Bu kilit düğümde zamanlanmasını ek pod'lar engeller. Kilit de aynı anda yalnızca bir düğümün başlatılması gösterir. Düğümle kordonlanır çalışan pod'ların düğümden boşaltılır ve düğüm yeniden.

Kullanarak düğümlerin durumunun izleyebilirsiniz [kubectl alma düğümleri][kubectl-get-nodes] komutu. Aşağıdaki örnek çıktıda bir düğüm durumu ile gösterilmektedir *SchedulingDisabled* düğümü yeniden başlatma işlemine hazırlanır gibi:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-28993262-0   Ready,SchedulingDisabled   agent     1h        v1.11.7
```

Güncelleştirme işlemi tamamlandıktan sonra kullanarak düğümlerin durumunun görüntüleyebileceğiniz [kubectl alma düğümleri][kubectl-get-nodes] komutunu `--output wide` parametresi. Bu ek çıkış bir fark görmenizi sağlayan *çekirdek sürümü* aşağıdaki örnek çıktıda gösterildiği gibi temel alınan düğümleri. *Aks nodepool1 28993262 0* bir önceki adımda ve gösterir çekirdek sürümü güncelleştirildiği *4.15.0-1039-azure*. Düğüm *aks nodepool1 28993262 1* , güncelleştirilmiş gösterir çekirdek sürümü olmamıştır *4.15.0-1037-azure*.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-28993262-0   Ready     agent     1h        v1.11.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS   4.15.0-1039-azure   docker://3.0.4
aks-nodepool1-28993262-1   Ready     agent     1h        v1.11.7   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1037-azure   docker://3.0.4
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede nasıl kullanılacağı ayrıntılı `kured` Linux düğümleri güvenlik güncelleştirme işleminin bir parçası olarak otomatik olarak yeniden başlatmak için. Kubernetes en son sürüme yükseltmek için [AKS kümenizi yükseltme][aks-upgrade].

Windows Server düğümlerini kullanma AKS kümeler için bkz: [aks'deki bir düğüm havuzunu yükseltme][nodepool-upgrade].

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
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
