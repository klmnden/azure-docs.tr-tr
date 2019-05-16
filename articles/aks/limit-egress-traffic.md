---
title: Azure Kubernetes Service (AKS) çıkış trafiği kısıtlama
description: Hangi bağlantı noktalarını ve adresi denetimi çıkış trafiği Azure Kubernetes Service (AKS) için gerekli olduğunu öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: de0ba13a527569e446a44c275b7323d4487f53b6
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780308"
---
# <a name="preview---limit-egress-traffic-for-cluster-nodes-and-control-access-to-required-ports-and-services-in-azure-kubernetes-service-aks"></a>Önizleme - sınırı çıkış trafiği için küme düğümlerini ve erişimi denetlemek için gerekli bağlantı noktaları ve hizmetler Azure Kubernetes Service (AKS)

Varsayılan olarak, AKS kümeleri sınırsız (çıkış) giden internet erişimi vardır. Bu ağ erişim düzeyi, düğümleri ve gerektiği gibi dış kaynaklara erişmek için çalıştırdığınız hizmetleri sağlar. Çıkış trafiği kısıtlamak istiyorsanız, bağlantı noktaları ve adres sınırlı sayıda sağlıklı küme bakım görevleri korumak için erişilebilir olmalıdır. Kümenizi, ardından yalnızca Microsoft kapsayıcı kayıt defteri (MCR) veya Azure Container Registry (ACR), genel olmayan dış depolar temel sistem kapsayıcı görüntülerini kullanmak üzere yapılandırılır.

Bu makalede, hangi ağ bağlantı noktaları ve tam etki alanı adlarını (FQDN) gerekli ve isteğe bağlı bir AKS kümesindeki çıkış trafiği kısıtlamak isterseniz ayrıntıları.  Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis ve kabul etme. Görüş ve hata topluluğumuza toplamak üzere önizlemeleri sağlanır. Ancak, Azure teknik destek birimi tarafından desteklenmez. Bir küme oluşturun veya var olan kümeleri için bu özellikleri ekleyin, bu özellik artık Önizleme aşamasındadır ve genel kullanılabilirlik (GA) mezunu kadar bu küme desteklenmiyor.
>
> Önizleme özellikleri sorunlarla karşılaşırsanız [AKS GitHub deposunda bir sorun açın] [ aks-github] hata başlığı önizleme özelliğini adı.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

Çıkış trafiği sınırlayabilirsiniz bir AKS kümesi oluşturmak için önce aboneliğinizde özellik bayrağı etkinleştirin. Bu özellik kaydı MCR veya ACR temel sistem kapsayıcı görüntülerini kullanmak için oluşturduğunuz herhangi bir AKS kümeleri yapılandırır. Kaydedilecek *AKSLockingDownEgressPreview* özellik bayrağı, kullanın [az özelliği kayıt] [ az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az feature register --name AKSLockingDownEgressPreview --namespace Microsoft.ContainerService
```

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kullanarak kayıt durumu denetleyebilirsiniz [az özellik listesi] [ az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSLockingDownEgressPreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısı [az provider register] [ az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="egress-traffic-overview"></a>Çıkış trafiği genel bakış

Yönetim ve işletimsel amaçları için bir AKS kümesindeki düğümlere belirli bağlantı noktalarını ve tam etki alanı adlarını (FQDN) erişmesi gerekir. Bu Eylemler, API sunucusu ile iletişim kurmak veya indir ve ana Kubernetes küme bileşenleri ve düğüm güvenlik güncelleştirmeleri yüklemeyi olabilir. Varsayılan olarak, çıkış (giden) internet trafiği bir AKS kümesindeki düğümler için sınırlı değildir. Küme temel sistemi dış depolardaki kapsayıcı görüntülerini çekerken.

AKS kümenizin güvenliğini artırmak için çıkış trafiği kısıtlamak isteyebilirsiniz. Küme temel sistemi MCR veya ACR için kapsayıcı görüntüleri çekmek için yapılandırılır. Çıkış trafiği bu şekilde kilitlemek, belirli bağlantı noktaları ve doğru şekilde gerekli dış hizmetlerle iletişim AKS düğümleri izin vermek için FQDN'leri tanımlamanız gerekir. Bu yetkili bağlantı noktaları ve FQDN'ler, AKS düğümlerinizi API sunucusu ile iletişim kurmak veya çekirdek bileşenlerini yükleyin.

Kullanabileceğiniz [Azure Güvenlik Duvarı] [ azure-firewall] veya 3. taraf güvenlik duvarı Gereci, çıkış trafiği güvenli hale getirme ve bunlar gerekli tanımlamak için bağlantı noktaları ve yöneliktir.

AKS, iki bağlantı noktaları ve adresleri kümesi vardır:

* [Gerekli bağlantı noktaları ve adres için AKS kümeleri](#required-ports-and-addresses-for-aks-clusters) yetkili çıkış trafiği için en düşük gereksinimler ayrıntıları.
* [İsteğe bağlı önerilen adresler ve bağlantı noktaları AKS kümeleri için](#optional-recommended-addresses-and-ports-for-aks-clusters) gibi Azure İzleyici düzgün çalışmaz senaryoları dışında tüm diğer hizmetler ile tümleştirme için gerekli değildir. Bu isteğe bağlı bağlantı noktaları ve FQDN'ler listesini gözden geçirin ve herhangi bir AKS kümenizde kullanılan bileşenleri ve Hizmetleri yetkilendirmek.

> [!NOTE]
> Çıkış trafiği sınırlama, yalnızca özellik bayrağı kaydı etkinleştirme işleminden sonra oluşturulan yeni AKS kümelerinde çalışır. Var olan kümeleri için [Küme yükseltme işlemi] [ aks-upgrade] kullanarak `az aks upgrade` çıkış trafiği sınırlamak önce komutu.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Gerekli bağlantı noktaları ve AKS küme için adresleri

Şu giden bağlantı noktalarını / ağ kuralları bir AKS kümesi için gereklidir:

* TCP bağlantı noktası *443*
* TCP bağlantı noktası *9000*

Aşağıdaki FQDN / uygulama kuralları gereklidir:

| FQDN                      | Port      | Kullanım      |
|---------------------------|-----------|----------|
| *.azmk8s.io               | HTTPS:443 | API sunucusu uç adresidir. |
| aksrepos.azurecr.io       | HTTPS:443 | Bu adres, erişim görüntülerini Azure Container Registry (ACR) gereklidir. |
| *.blob.core.windows.net   | HTTPS:443 | Bu adres, ACR içinde depolanan görüntülerin arka uç deposudur. |
| mcr.microsoft.com         | HTTPS:443 | Bu adres Microsoft kapsayıcı kayıt defteri (MCR) erişim görüntüleri için gerekli değildir. |
| Management.Azure.com      | HTTPS:443 | Bu adres, Kubernetes GET/PUT işlemleri için gereklidir. |
| login.microsoftonline.com | HTTPS:443 | Bu adres Azure Active Directory kimlik doğrulaması için gerekli değildir. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>İsteğe bağlı adresler ve bağlantı noktaları AKS kümeleri için önerilen

Şu giden bağlantı noktalarını / ağ kuralları AKS kümeleri düzgün çalışması için gerekli değildir, ancak önerilir:

* UDP bağlantı noktası *123* NTP zaman eşitleme
* UDP bağlantı noktası *53* DNS

Aşağıdaki FQDN / uygulama kuralları düzgün çalışması AKS kümeleri için önerilir:

| FQDN                                    | Port      | Kullanım      |
|-----------------------------------------|-----------|----------|
| *.ubuntu.com                            | HTTP:80   | Bu adres, güncelleştirmeleri ve gerekli güvenlik düzeltme ekleri indirmek Linux küme düğümleri sağlar. |
| Packages.microsoft.com                  | HTTPS:443 | Bu adres kullanılan Microsoft paketleri depodur önbelleğe için *apt-get* operations. |
| dc.services.visualstudio.com            | HTTPS:443 | Doğru ölçümler için önerilen ve Azure İzleyici'nin kullanımını izleme. |
| *.opinsights.azure.com                  | HTTPS:443 | Doğru ölçümler için önerilen ve Azure İzleyici'nin kullanımını izleme. |
| *.monitoring.azure.com                  | HTTPS:443 | Doğru ölçümler için önerilen ve Azure İzleyici'nin kullanımını izleme. |
| gov-prod-policy-data.trafficmanager.net | HTTPS:443 | Bu adres, Azure İlkesi'nin doğru işlem (şu anda önizlemede aks'deki) için kullanılır. |
| apt.dockerproject.org                   | HTTPS:443 | Bu adres, doğru sürücü yüklemesi ve GPU tabanlı düğümlerde işlemi için kullanılır. |
| nvidia.github.io                        | HTTPS:443 | Bu adres, doğru sürücü yüklemesi ve GPU tabanlı düğümlerde işlemi için kullanılır. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, hangi bağlantı noktaları ve küme için çıkış trafiği kısıtlamak, izin vermek için adresleri öğrendiniz. Pod'ların kendileri nasıl iletişim kurabilir ve hangi kısıtlamaları da tanımlayabilirsiniz küme içinde sahip oldukları. Daha fazla bilgi için [güvenli ağ ilkeleri kullanarak AKS pod'ları arasındaki trafiği][network-policy].

<!-- LINKS - external -->
[aks-github]: https://github.com/azure/aks/issues]

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-upgrade]: upgrade-cluster.md
