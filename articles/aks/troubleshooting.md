---
title: Yaygın Azure Kubernetes Service sorunlarını giderme
description: Azure Kubernetes Service (AKS) kullanırken sık karşılaşılan sorunları çözmek ve sorun giderme hakkında bilgi edinin
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: fd3d1c464c6f2d4cbecd715db0689581ca141769
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53654079"
---
# <a name="aks-troubleshooting"></a>AKS sorunlarını giderme

Oluştururken veya Azure Kubernetes Service (AKS) kümelerini yönetme bazen sorunlarla karşılaşabilirsiniz. Bu makalede bazı yaygın sorunlar ve sorun giderme adımları ayrıntılı olarak açıklanmaktadır.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>Genel olarak, Kubernetes sorunlarda hata ayıklaması hakkında nereden bilgi?

Deneyin [Kubernetes kümelerini sorun giderme resmi Kılavuzu](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).
Ayrıca bir [sorun giderme kılavuzu](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md), pod'ları, düğümler, kümeler ve diğer özellikleri sorun giderme için bir Microsoft mühendisi tarafından yayımlanmış.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Oluşturma veya yükseltme sırasında bir "Kota aşıldı" hatası alıyorum. Ne yapmalıyım? 

Şunları yapmanız [istek çekirdek](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>AKS için en fazla düğüm başına pod'ların ayarı nedir?

Azure portalında AKS kümesi dağıtıyorsanız en fazla düğüm başına pod'ların varsayılan olarak 30 ayardır.
Azure clı'da AKS kümesi dağıtıyorsanız en fazla düğüm başına pod'ların 110 varsayılan ayardır. (Azure CLI'ın en son sürümü kullandığınızdan emin olun). Bu varsayılan ayarı kullanarak değiştirilebilir `–-max-pods` bayrağını `az aks create` komutu.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Gelişmiş ağ ile bir AKS kümesi dağıtırken bir insufficientSubnetSize hata alıyorum. Ne yapmalıyım?

AKS oluşturma sırasında ağ iletişimi için özel Azure sanal ağı seçeneğinde Azure kapsayıcı ağ arabirimi (CNI) IP adresi Yönetimi (IPAM) için kullanılır. Bir AKS kümesindeki düğüm sayısını, 1 ile 100 arasında herhangi bir yerde olabilir. Önceki bölümde üzerinde bağlı olarak, alt ağ boyutunu düğüm ve düğüm başına en fazla pod'ların sayısını çarpımını büyük olmalıdır. İlişkinin bu şekilde ifade edilebilir: alt ağ boyutu > kümedeki düğümlerin sayısı * düğüm başına en fazla pod'ları.

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>My pod CrashLoopBackOff modunda takıldı. Ne yapmalıyım?

Bu modda takılması pod çeşitli nedenleri olabilir. İçine benzeyebilir:

* Kullanarak pod kendisini `kubectl describe pod <pod-name>`.
* Kullanarak günlükleri `kubectl log <pod-name>`.

Pod sorunların nasıl giderileceği hakkında daha fazla bilgi için bkz. [uygulamalarında hata ayıklama](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Var olan bir kümede RBAC etkinleştirme hale getirmeye çalışıyorum. Ne yapabilirim?

Ne yazık ki, rol tabanlı erişim denetimi (RBAC) mevcut kümelerinde etkinleştirme, şu anda desteklenmiyor. Yeni küme açıkça oluşturmalısınız. RBAC, CLI'yı kullanırsanız, varsayılan olarak etkindir. AKS portalı kullanıyorsanız, RBAC etkinleştirmek için iki durumlu bir düğmenin oluşturma iş akışı içinde kullanılabilir.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Varsayılanları ya da Azure portal ile Azure CLI kullanarak etkin RBAC ile bir küme oluşturduğum ve çok sayıda uyarı Kubernetes Panoda artık görüyorum. Tüm uyarılar çalışmak için kullanılan Pano. Ne yapmalıyım?

Panoda uyarı nedeni küme ile RBAC şimdi etkinleştirildi ve erişimi varsayılan olarak devre dışı bırakıldı ' dir. Genel olarak, Pano varsayılan açığa kümenin tüm kullanıcılara güvenlik tehditlerine neden olabileceği için bu yaklaşım yararlı olur. Pano etkinleştirmek istiyorsanız, adımları [bu blog gönderisini](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Panoya bağlanamıyorum. Ne yapmalıyım?

Küme dışında hizmetinize erişmek için en kolay yolu çalıştırmaktır `kubectl proxy`, hangi proxy'leri istekleri, localhost 8001 numaralı bağlantı noktası Kubernetes API sunucusuna gönderilen. Burada, API sunucusu için proxy hizmetinize: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Kubernetes panosunu görmüyorsanız denetleyin olmadığını `kube-proxy` pod çalıştıran `kube-system` ad alanı. Çalışır durumda değilse, pod silin ve yeniden başlatılır.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Günlükleri kubectl günlükleri kullanarak alamıyorum veya API sunucusuna bağlanamıyorum. Alıyorum "sunucu hatası: hata arama arka uç: arama tcp..." Ne yapmalıyım?

Varsayılan ağ güvenlik grubu (NSG) değiştirilmez ve bağlantı noktası 22 API sunucusu bağlantısı için açık olduğundan emin olun. Denetleme olmadığını `tunnelfront` pod çalıştıran `kube-system` ad alanı. Aksi takdirde, pod ve onu zorla silinmesini yeniden başlatılır.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-problem"></a>Yükseltme veya ölçeklendirme hale getirmeye çalışıyorum ve alıyorum bir "ileti: "Imagereference' özelliğinin değiştirilmesine izin" bir hata oluştu.  Bu sorunu nasıl düzeltebilirim?

AKS kümesi içindeki aracı düğümleri etiketleri değiştirdiğiniz çünkü bu hatayı alma. Değiştirme ve silme etiketleri ve diğer özellikleri MC_ * kaynak grubundaki kaynakların beklenmeyen sonuçlara neden olabilir. Aks'deki MC_ * grubundaki kaynakların değiştirilmesi, hizmet düzeyi hedefi (SLO) küme keser.

## <a name="how-do-i-renew-the-service-principal-secret-on-my-aks-cluster"></a>My AKS kümesinde hizmet sorumlusu gizli anahtarı nasıl yenileyebilirim?

Varsayılan olarak, bir yıllık sona erme zamanına sahip bir hizmet sorumlusu AKS kümeleri oluşturulur. Sona erme tarihine yakın,, ek bir süre için hizmet sorumlusu genişletmek için kimlik bilgilerini sıfırlayabilirsiniz.

Aşağıdaki örnek, aşağıdaki adımları gerçekleştirir:

1. Hizmet sorumlusu kimliği kümenizin kullanarak alır [az aks show](/cli/azure/aks#az-aks-show) komutu.
1. Hizmet sorumlusu istemci parolasını kullanarak listeleri [az ad sp kimlik bilgileri listesi](/cli/azure/ad/sp/credential#az-ad-sp-credential-list).
1. Hizmet sorumlusunu kullanarak başka bir yıl boyunca genişletir [az ad sp reset kimlik bilgisi](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) komutu. Hizmet sorumlusu istemci parolası düzgün şekilde çalışması AKS kümesi için aynı kalmalıdır.

```azurecli
# Get the service principal ID of your AKS cluster.
sp_id=$(az aks show -g myResourceGroup -n myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)

# Get the existing service principal client secret.
key_secret=$(az ad sp credential list --id $sp_id --query [].keyId -o tsv)

# Reset the credentials for your AKS service principal and extend for one year.
az ad sp credential reset \
    --name $sp_id \
    --password $key_secret \
    --years 1
```
