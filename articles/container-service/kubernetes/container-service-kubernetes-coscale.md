---
title: Bir Azure Kubernetes küme CoScale ile izleme
description: İzleyici CoScale kullanarak Azure kapsayıcı hizmeti Kubernetes kümede
services: container-service
author: fryckbos
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 16580307193bbb7eb9b401eb1b14356e8589d6e2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Azure kapsayıcı hizmeti Kubernetes küme CoScale ile izleme

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Bu makalede, nasıl dağıtılacağı gösteriyoruz [CoScale](https://www.coscale.com/) tüm düğümler ve Kubernetes kümenizdeki Azure kapsayıcı hizmeti kapsayıcı izlemek için aracı. Bu yapılandırma için CoScale sahip bir hesap gerekir. 


## <a name="about-coscale"></a>CoScale hakkında 

CoScale birkaç orchestration platformlarda tüm kapsayıcıları gelen ölçümleri ve olayları toplayan izleme bir platformdur. CoScale Kubernetes ortamlar için tam yığını izleme sunar. Tüm Katmanlar yığınında için görselleştirme ve analizi sağlar: işletim sistemi, Kubernetes, Docker ve kapsayıcıların içinde çalışan uygulamalar. Birkaç yerleşik izleme panoları coScale sunar ve işleçler ve geliştiricilerin altyapı ve uygulama sorunlarını hızlı bulmak izin vermek için yerleşik anomali algılama içeriyor.

![UI coScale](./media/container-service-kubernetes-coscale/coscale.png)

Bu makalede gösterildiği gibi SaaS çözümü olarak CoScale çalıştırmak için Kubernetes kümede aracıları yükleyebilirsiniz. Verilerinizi yerinde tutmak istiyorsanız, CoScale de şirket içi yükleme için kullanılabilir.


## <a name="prerequisites"></a>Önkoşullar

Öncelikle [CoScale hesabı oluşturma](https://www.coscale.com/free-trial).

Bu kılavuz, sahibi olduğunuzu varsayar [Azure kapsayıcı hizmeti kullanarak Kubernetes küme oluşturulan](container-service-kubernetes-walkthrough.md).

Ayrıca, sahibi olduğunuzu varsayar `az` Azure CLI ve `kubectl` araçları yüklü.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```azurecli
az --version
```

Sahip değilseniz `az` yüklü, aracı yönergeler vardır [burada](/cli/azure/install-azure-cli).

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```bash
kubectl version
```

Sahip değilseniz `kubectl` yüklü çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>DaemonSet ile CoScale Aracısı'nı yükleme
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) Kubernetes tarafından kümedeki her ana bilgisayarda bir kapsayıcı tek bir örneği çalıştırmak için kullanılır.
Bunlar CoScale aracı gibi izleme aracıları çalıştırmak için mükemmel.

CoScale için oturum açtıktan sonra Git [Aracısı sayfası](https://app.coscale.com/) bir DaemonSet kullanarak kümenizde CoScale aracıları yüklemek için. CoScale kullanıcı Arabirimi bir aracı oluşturun ve tam Kubernetes kümenizi izlemeye başlamak için Destekli yapılandırma adımları sağlar.

![CoScale Aracısı yapılandırması](./media/container-service-kubernetes-coscale/installation.png)

Kümede Aracısı'nı başlatmak için sağlanan komutu çalıştırın:

![CoScale Aracısı'nı Başlat](./media/container-service-kubernetes-coscale/agent_script.png)

İşte bu kadar! Aracılar ve çalışıyor olduktan sonra birkaç dakika içinde veri konsolunda görmeniz gerekir. Ziyaret [Aracısı sayfası](https://app.coscale.com/) kümenizi özetini görmek için ek yapılandırma adımları uygulayın ve panolar gibi bakın **Kubernetes küme genel bakış**.

![Kubernetes küme genel bakış](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

CoScale aracı kümedeki yeni makinelere otomatik olarak dağıtılır. Otomatik olarak yeni bir sürümü yayımlandığında aracı güncelleştirmeleri.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [CoScale belgelerine](http://docs.coscale.com/) ve [blog](https://www.coscale.com/blog) CoScale çözümlerini izleme hakkında daha fazla bilgi için. 

