---
title: Azure Red Hat OpenShift için sık sorulan sorular | Microsoft Docs
description: Microsoft Azure Red Hat OpenShift hakkında sık sorulan soruların yanıtları aşağıdadır.
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 77e0e11582808901b10877d0d9284637145aa6f2
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078678"
---
# <a name="azure-red-hat-openshift-faq"></a>Azure Red Hat OpenShift ile ilgili SSS

Bu makalede, Microsoft Azure Red Hat OpenShift hakkında sık sorulan sorular (SSS) yöneliktir.

## <a name="which-azure-regions-are-supported"></a>Azure hangi bölgeler desteklenir?

Bkz: [desteklenen kaynakları](supported-resources.md#azure-regions) Azure Red Hat OpenShift desteklendiği genel bölgelerin bir listesi için.

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>Ben, mevcut bir sanal makineyi bir kümede dağıtabilirsiniz?

Evet. Bir küme oluştururken bir sanal ağınız bir Red Hat OpenShift Azure kümesine dağıtabilirsiniz. Bkz: [kümenin sanal ağ mevcut bir sanal ağa bağlama ](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) Ayrıntılar için.

## <a name="what-cluster-operations-are-available"></a>Hangi küme işlemlerini kullanılabilir mi?

Yalnızca yukarı veya aşağı işlem düğümü sayısını ölçeklendirebilirsiniz. Herhangi bir değişiklik izin verilen `Microsoft.ContainerService/openShiftManagedClusters` oluşturulduktan sonra kaynak. İşlem düğümleri sayısı 20'ye sınırlıdır.

## <a name="what-virtual-machine-sizes-can-i-use"></a>Hangi sanal makine boyutları kullanabilir miyim?

Bkz: [Azure Red Hat OpenShift sanal makine boyutları](supported-resources.md#virtual-machine-sizes) ile bir Azure Red Hat OpenShift kümesini kullanabileceğiniz sanal makine boyutlarının listesi.

## <a name="is-data-on-my-cluster-encrypted"></a>Veri şifrelenmiş kümem mi?

Varsayılan olarak, bekleme sırasında şifreleme yoktur. Azure depolama platformu ve alma önce çözer devam ettirmeden önce verilerinizi otomatik olarak şifreler. Bkz: [bekleyen veriler için Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) Ayrıntılar için.

## <a name="can-i-use-prometheusgrafana-to-monitor-containers-and-manage-capacity"></a>Kapsayıcıları izlemek ve kapasitesini yönetmek için Prometheus/Grafana kullanabilir miyim?

Hayır, geçerli anda değil.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>Harici olarak Jenkins gibi araçlarla kullanabilmeniz için Docker kayıt defteri kullanılabilir mi?

Docker kayıt defteri kullanılabilir `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` ancak güçlü depolama dayanıklılık garantisi sağlanmadı. Ayrıca [Azure Container Registry](https://azure.microsoft.com/services/container-registry/).

## <a name="is-cross-namespace-networking-supported"></a>Çapraz-namespace ağ destekleniyor mu?

Müşteri ve her bir proje yöneticileri özelleştirme arası ad alanı ağı (bunu reddetme dahil) kullanarak bir proje başına temelinde `NetworkPolicy` nesneleri.

## <a name="can-an-admin-manage-users-and-quotas"></a>Bir yönetici, kullanıcıları ve kotalar yönetebilir miyim?

Evet. Azure Red Hat OpenShift yönetici kullanıcıları ve projeleri oluşturulan tüm kullanıcı erişim ek kotalar yönetebilirsiniz.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>Yalnızca belirli Azure AD kullanıcılarının bir küme kısıtlayabilir miyim?

Evet. Hangi Azure kısıtlayabilirsiniz AD kullanıcıları oturum açabilir bir küme için Azure AD uygulaması yapılandırarak. Ayrıntılar için bkz [nasıl yapılır: Uygulamanızı bir kullanıcı kümesini sınırlamak](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>Bir küme için işlem düğümlerini Azure bölgelerinde sahip?

Hayır. Azure Red Hat OpenShift kümedeki tüm düğümlerin aynı Azure bölgesinden kaynaklanan gerekir.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ile olduğu gibi ana ve altyapı düğümleri hemen soyutlanır?

Hayır. Küme Yöneticisi dahil tüm kaynakları, bir müşteri aboneliğinde çalıştırın. Bu kaynak türlerinin bir salt okunur kaynak grubuna yerleştirilir.
