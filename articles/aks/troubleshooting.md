---
title: Yaygın Azure Kubernetes Service sorunlarını giderme
description: Azure Kubernetes Service (AKS) kullanırken sık karşılaşılan sorunları çözmek ve sorun giderme hakkında bilgi edinin
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: f0b0ff3ff4ac742a7e850798c736eb31098f66e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65966385"
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

Azure CNI (Gelişmiş ağ) kullanılıyorsa, AKS ele IP preallocates "max-pod'ların" yapılandırılmış düğüm başına göre. Bir AKS kümesindeki düğüm sayısını, 110 ve 1 arasında herhangi bir yerde olabilir. Düğüm başına en fazla yapılandırılmış pod temel, alt ağ boyutu "product" düğüm ve düğüm başına en fazla pod numarasının büyük olmalıdır. Aşağıdaki temel eşitliği bu özetlenmektedir:

Alt ağ boyutu > (gelecekteki ölçeklendirme gereksinimlerini göz önünde bulundurarak) kümedeki düğümlerin sayısı * düğüm başına pod'ların maks.

Daha fazla bilgi için [kümeniz için planlama IP adresleme](configure-azure-cni.md#plan-ip-addressing-for-your-cluster).

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

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Günlükleri kubectl günlükleri kullanarak alamıyorum veya API sunucusuna bağlanamıyorum. Alıyorum "sunucu hatası: hata arama arka uç: tcp... arama". Ne yapmalıyım?

Varsayılan ağ güvenlik grubu değiştirilmez ve bağlantı noktası 22 API sunucusu bağlantısı için açık olduğundan emin olun. Denetleme olup olmadığını `tunnelfront` pod çalıştıran *kube-system* using namespace `kubectl get pods --namespace kube-system` komutu. Aksi takdirde, pod ve onu zorla silinmesini yeniden başlatılır.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Yükseltme veya ölçeklendirme hale getirmeye çalışıyorum ve alıyorum bir "ileti: "Imagereference' özelliğinin değiştirilmesine izin" bir hata oluştu. Bu sorunu nasıl düzeltebilirim?

AKS kümesi içindeki aracı düğümleri etiketleri değiştirdiğiniz çünkü bu hatayı alma. Değiştirme ve silme etiketleri ve diğer özellikleri MC_ * kaynak grubundaki kaynakların beklenmeyen sonuçlara neden olabilir. Aks'deki MC_ * grubundaki kaynakların değiştirilmesi, hizmet düzeyi hedefi (SLO) küme keser.

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Kümem başarısız durumda ve düzeltilene kadar yükseltme veya ölçeklendirme çalışmaz hata mesajı alıyorum

*Bu sorun giderme Yardımı gelen yönlendirilir https://aka.ms/aks-cluster-failed*

Küme başarısız durumda birden çok nedenlerle girin. Bu hata oluşur. Daha önce başarısız olan işlemi yeniden denemeden önce küme başarısız durumu çözmek için aşağıdaki adımları izleyin:

1. Küme dışı kadar `failed` durumu `upgrade` ve `scale` olmaz işlemleri başarılı. Ortak kök sorunlar ve çözümleri şunlardır:
    * İle ölçeklendirme **(CRP) yeterli işlem kotası**. Gidermek için ilk ölçek kümenizi kota içinde kararlı hedef duruma yedekleyin. Aşağıdaki adımları [işlem kotası istemeniz adımları artırmak](../azure-supportability/resource-manager-core-quotas-request.md) yeniden ilk kota sınırları ötesinde ölçeklendirme denemeden önce.
    * Gelişmiş ağ ile bir küme ölçeklendirme ve **yetersiz (ağ) alt ağ kaynaklarının**. Gidermek için ilk ölçek kümenizi kota içinde kararlı hedef duruma yedekleyin. Daha sonra izleyin [kaynak kotası istemek için bu adımları artırmak](../azure-resource-manager/resource-manager-quota-errors.md#solution) yeniden ilk kota sınırları ötesinde ölçeklendirme denemeden önce.
2. Kümenizi, hatanın temel nedenini yükseltme çözümlendiğinde durumu başarılı olması gerekir. Başarılı durumda doğrulandıktan sonra özgün işlemi yeniden deneyin.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Şu anda yükseltme yapmaya veya kümem durum ölçek durdurulmasını yüklenirken hataları alıyorum yükseltti veya yükseltmesi başarısız oldu

*Bu sorun giderme Yardımı gelen yönlendirilir https://aka.ms/aks-pending-upgrade*

Küme işlemlerini, etkin yükseltme gibi işlemler gerçekleştiğini veya yükseltmeye çalıştı, ancak sonradan başarısız sınırlıdır. Çalıştırma sorunu tanılamak için `az aks show -g myResourceGroup -n myAKSCluster -o table` kümenizdeki ayrıntılı durumu alınamadı. Sonuca bağlı:

* Kümenin etkin bir şekilde yükseltme işlemi sonlanana kadar bekleyin. Bu başarılı olursa, daha önce başarısız olan işlemi yeniden deneyin.
* Küme yükseltme başarısız oldu, yukarıda özetlenen adımları izleyin.

## <a name="can-i-move-my-cluster-to-a-different-subscription-or-my-subscription-with-my-cluster-to-a-new-tenant"></a>Kümem için farklı bir abonelik veya aboneliğime kümem için yeni bir kiracı ile taşıyabilirim?

AKS kümenizi farklı bir abonelik veya yeni bir kiracı için abonelik sahibi olan küme taşıdıysanız, küme işlevselliği kaybeden rol atamaları ve hizmet sorumluları hakları nedeniyle kaybedersiniz. **AKS abonelikleri veya kiracılar arasında taşıma kümelerini desteklemez** nedeniyle bu kısıtlama.

## <a name="im-receiving-errors-trying-to-use-features-that-require-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri gerektiren özelliklerini kullanmayı denemek hata mesajı alıyorum

*Bu sorun giderme Yardımı aka.ms/aks-vmss-etkinleştirme yönlendirilir*

AKS kümenizi aşağıdaki örnek gibi bir sanal makine ölçek kümesinde değil belirtmek hataları alabilirsiniz:

**AgentPool 'agentpool' de etkinleştirildiği şekilde, otomatik ölçeklendirme ayarlayın ancak sanal makine ölçek kümeleri üzerinde değil**

Küme otomatik ölçeklendiricinin veya birden çok düğüm gibi özellikleri kullanmak için havuzları, AKS küme sanal makine ölçek kümeleri kullanan oluşturulması gerekir. Sanal makine ölçek kümelerinde bağlı özelliklerini kullanmayı deneyin ve normal, sanal makine ölçek kümesi AKS kümesi hedef hatalar döndürülür. Sanal makine ölçek kümesi desteği şu anda önizleme olarak kullanılabilir.

İzleyin *başlamadan önce* özellik sanal makine ölçek kümesi için doğru olarak kaydetmek için uygun belgedeki adımları Önizleme ve AKS kümesi oluşturma:

* [Küme otomatik ölçeklendiricinin kullanın](cluster-autoscaler.md)
* [Oluşturma ve birden fazla düğüm havuzu kullanma](use-multiple-node-pools.md)
 
## <a name="what-naming-restrictions-are-enforced-for-aks-resources-and-parameters"></a>AKS kaynakları ve parametreleri için hangi adlandırma kısıtlamaları uygulanır?

*Bu sorun giderme Yardımı aka.ms/aks-adlandırma-kurallardan yönlendirilir*

Adlandırma kısıtlamaları Azure platformu ve AKS tarafından uygulanır. Bir kaynak adı ya da parametre kısıtlamalarının birini keserse, farklı giriş sağlamak isteyen bir hata döndürülür. Aşağıdaki ortak adlandırma yönergeleri uygulayın:

* AKS *MC_* kaynak grubu adı, kaynak grubu adı ve kaynak adı birleştirir. Otomatik olarak oluşturulan söz dizimi `MC_resourceGroupName_resourceName_AzureRegion` 80 karakterden büyük olması gerekir. Gerekli olursa, kaynak grubu adı veya AKS kümesinin adını kısaltın.
* *Dnsprefıx* ve alfa sayısal değerler ile bitmelidir. Geçerli karakterler alfasayısal değerler ve kısa çizgi (-) içerir. *Dnsprefıx* bir nokta (.) gibi özel karakterler içeremez.

## <a name="im-receiving-errors-when-trying-to-create-update-scale-delete-or-upgrade-cluster-that-operation-is-not-allowed-as-another-operation-is-in-progress"></a>Hataları oluşturmak, güncelleştirmek, ölçeklendirme, silmek veya başka bir işlem sürdüğünden, işleme izin verilmiyor Küme yükseltme çalışırken durumuyla karşılaşıyorum.

*Bu sorun giderme Yardımı aka.ms/aks-bekleyen-işlemden yönlendirilir*

Küme işlemlerini, bir önceki işlemi hala devam ederken sınırlıdır. Kümenizi ayrıntılı durumunu almak için kullanın `az aks show -g myResourceGroup -n myAKSCluster -o table` komutu. Kendi kaynak grubunun ve AKS kümesinin adını kullanın.

Küme durumunu üzerinde çıkışını temel:

* Bir küme dışındaki tüm sağlama durumunda ise *başarılı* veya *başarısız*, işlem kadar bekleyin (*yükseltme / güncelleştirme / oluşturma / ölçeklendirme / silme / taşıma*) sona erer. Önceki işlemi tamamlandığında, en son küme işleminizi yeniden deneyin.

* Küme başarısız bir yükseltmeyi varsa, verilen adımları izleyin [kümem hatalı durumda hataları alma ve yükseltme veya ölçeklendirme çalışmaz düzeltilene kadar](#im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed).