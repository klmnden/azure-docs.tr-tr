---
title: CoScale ile bir Azure Kubernetes kümesini izleme
description: CoScale kullanarak Azure Container Service içindeki bir Kubernetes kümesini izleme
services: container-service
author: fryckbos
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: e9896a34e835646e17328482c07d8031c624e858
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45544039"
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>CoScale ile bir Azure Container Service Kubernetes kümesini izleme

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Bu makalede, nasıl dağıtılacağı gösteriyoruz [CoScale](https://www.coscale.com/) tüm düğümleri ve Azure Container Service'te Kubernetes kümenizde kapsayıcıları İzleme Aracısı. Bu yapılandırma için bir hesabıyla CoScale ihtiyacınız var. 


## <a name="about-coscale"></a>CoScale hakkında 

CoScale birkaç düzenleme platformlarda tüm kapsayıcılardan ölçümleri ve olayları toplayan bir izleme platformudur. CoScale, Kubernetes ortamlar için tam yığın izlemesini sunar. Yığınındaki tüm katmanlar için görselleştirmeler ve analiz sağlar: OS, Kubernetes, Docker ve kapsayıcılarınızı içinde çalışan uygulamalar. CoScale birkaç yerleşik izleme panoları sunar ve yerleşik anomali algılama ve geliştiricilerin altyapı ve uygulama sorunları hızla bulmak izin vermek için vardır.

![CoScale kullanıcı Arabirimi](./media/container-service-kubernetes-coscale/coscale.png)

Bu makalede gösterilen şekilde bir SaaS çözümü olarak CoScale çalıştırmak için bir Kubernetes kümesinde aracıları yükleyebilirsiniz. CoScale verilerinizi yerinde tutmak istiyorsanız, ayrıca şirket içi yükleme için kullanılabilir.


## <a name="prerequisites"></a>Önkoşullar

Öncelikle [CoScale hesabı oluşturma](https://www.coscale.com/free-trial).

Bu izlenecek yol, sahibi olduğunuzu varsayar [Azure Container Service kullanan bir Kubernetes kümesi oluşturuldu](container-service-kubernetes-walkthrough.md).

Ayrıca, sahibi olduğunuzu varsayar `az` Azure CLI ve `kubectl` araçlarının yüklü.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```azurecli
az --version
```

Öğeniz yoksa `az` yüklü aracı yönergeler sunulmaktadır [burada](/cli/azure/install-azure-cli).

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```bash
kubectl version
```

Öğeniz yoksa `kubectl` yüklü çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>İle bir DaemonSet CoScale Aracısı'nı yükleme
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) Kubernetes tarafından kümedeki her bir konakta bir kapsayıcıyı tek bir örneğini çalıştırmak için kullanılır.
Bunlar CoScale aracı gibi izleme aracılarını çalıştırmak için ideal.

CoScale için oturum açtıktan sonra Git [Aracısı sayfası](https://app.coscale.com/) bir DaemonSet kullanarak kümenizde CoScale aracıları yüklemek için. CoScale kullanıcı Arabirimi, bir aracı oluşturun ve tüm Kubernetes kümenizi izlemeye başlamak için Kılavuzlu yapılandırma adımlarını sağlar.

![CoScale aracı yapılandırması](./media/container-service-kubernetes-coscale/installation.png)

Kümede Aracısı'nı başlatmak için belirtilen komutu çalıştırın:

![CoScale Aracısı'nı başlatın](./media/container-service-kubernetes-coscale/agent_script.png)

İşte bu kadar! Aracılar hazır ve çalışır olduğunda birkaç dakika içinde veri konsolunda görmeniz gerekir. Ziyaret [Aracısı sayfası](https://app.coscale.com/) kümenizi özetini görmek için ek yapılandırma adımlarını gerçekleştirin ve panolar gibi bakın **Kubernetes küme genel bakış**.

![Kubernetes kümesine genel bakış](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

CoScale Aracısı kümedeki yeni makinelere otomatik olarak dağıtılır. Otomatik olarak yeni bir sürümü yayımlandığında, aracı güncelleştirmeleri.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [CoScale belgeleri](http://docs.coscale.com/) ve [blog](https://www.coscale.com/blog) CoScale izleme çözümleri hakkında daha fazla bilgi. 

