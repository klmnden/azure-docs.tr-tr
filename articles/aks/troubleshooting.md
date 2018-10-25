---
title: Yaygın Azure Kubernetes Service sorunlarını giderme
description: Azure Kubernetes Service (AKS) kullanırken sık karşılaşılan sorunları çözmek ve sorun giderme hakkında bilgi edinin
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 1fd8f7c8499b7f9223939b8d426f274e79fd190e
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025359"
---
# <a name="aks-troubleshooting"></a>AKS sorunlarını giderme
Oluşturduğunuz veya NLB Yöneticisi'ni AKS, bazen sorunlarla karşılaşabilirsiniz. Bu makalede bazı yaygın sorunlar ve sorun giderme adımları ayrıntılı olarak açıklanmaktadır.

### <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-issues"></a>Genel olarak, Kubernetes sorunları hata ayıklama hakkında nereden bilgi?

[Burada](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/) kubernetes kümelerini sorun giderme için resmi bir bağlantıdır.
[Burada](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md) pod'ları, düğümler, kümeleri, vb. sorun giderme geçici olarak bir Microsoft mühendisi tarafından yayımlanan bir sorun giderme kılavuzu bir bağlantıdır.

### <a name="i-am-getting-a-quota-exceeded-error-during-create-or-upgrade-what-should-i-do"></a>Oluşturma veya yükseltme sırasında bir kota aşıldı hatası alıyorum. Ne yapmalıyım? 

Çekirdek istemek ihtiyacınız olacak [burada](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

### <a name="what-is-the-max-pods-per-node-setting-for-aks"></a>AKS için düğüm ayarı başına maksimum pod'ları nedir?

Azure portalında AKS kümesi dağıtıyorsanız, düğüm başına en fazla pod'ların varsayılan olarak 30 ayarlanır.
Azure CLI'yı bir AKS kümesinde dağıtırsanız, düğüm başına en fazla pod'ların 110 olarak varsayılan olarak ayarlanır. (Azure CLI'ın en son sürümü kullandığınızdan emin olun). Bu varsayılan ayarı kullanılarak değiştirilebilir. max-düğüm-başına-pod bayrağı az aks create komutu.

### <a name="i-am-getting-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Gelişmiş ağ ile bir AKS kümesi dağıtırken "insufficientSubnetSize" hatası alıyorum. Ne yapmalıyım?

AKS sırasında ağ oluşturur için seçilen özel VNET seçeneğinde Azure CNI IPAM için kullanılır. Bir AKS kümesindeki düğüm sayısını, 1 ile 100 arasında herhangi bir yerde olabilir. 2 sırasında bağlı olarak) boyutu alt düğüm sayısını ve düğüm alt ağ boyutu başına en fazla pod çarpımını büyük olmalıdır > kümedeki düğümlerin sayısı * düğüm başına pod'ların maks.

### <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>My pod 'CrashLoopBackOff' modunda takıldı. Ne yapmalıyım?

Bu modda takılması pod çeşitli nedenleri olabilir. İçine bak isteyebilirsiniz. 
* Pod kullanma `kubectl describe pod <pod-name>`
* Kullanarak günlükleri  `kubectl log <pod-name>`

### <a name="i-am-trying-to-enable-rbac-on-an-existing-cluster-can-you-tell-me-how-i-can-do-that"></a>Var olan bir kümede RBAC etkinleştirme hale getirmeye çalışıyorum. Ver nasıl yapabilirim?

Ne yazık ki RBAC mevcut kümelerinde etkinleştirme, şu anda desteklenmiyor. Açıkça yeni bir küme oluşturmanız gerekir. CLI'yı kullanıyorsanız, bunu etkinleştirmek için iki durumlu bir düğmenin AKS Portalı'nda kullanılabilir ancak RBAC varsayılan olarak etkindir iş akışı oluşturun.

### <a name="i-created-a-cluster-using-the-azure-cli-with-defaults-or-the-azure-portal-with-rbac-enabled-and-numerous-warnings-in-the-kubernetes-dashboard-the-dashboard-used-to-work-before-without-any-warnings-what-should-i-do"></a>Ben, kubernetes Pano ile RBAC etkin varsayılanları ya da Azure portal ile Azure CLI kullanarak bir küme ve çok sayıda uyarı oluşturuldu. Tüm uyarılar olmadan önce çalışmak üzere kullanılan Pano. Ne yapmalıyım?

Panoda uyarılar alma nedeni ile RBAC'ed şimdi etkinleştirildi ve erişimi varsayılan olarak devre dışı bırakıldı ' dir. Genel olarak, Pano varsayılan açığa kümenin tüm kullanıcılara güvenlik tehditlerine neden olabileceği bu yaklaşım iyi uygulama olarak kabul edilir. Pano etkinleştirmek istiyorsanız, bunu izleyin [blog](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/) etkinleştirin.

### <a name="i-cant-seem-to-connect-to-the-dashboard-what-should-i-do"></a>Panoya bağlanmak için gerçekleştiremiyorum. Ne yapmalıyım?

Küme dışında hizmetinize erişmek için en kolay yolu, localhost 8001 numaralı bağlantı noktası Kubernetes API sunucusu için proxy isteklerini olacak kubectl proxy çalıştırmaktır. Buradan, apiserver hizmetinize proxy olabilir: http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#! / düğüm? ad alanı = default

Kubernetes panosunu görmüyorsanız, kube-proxy pod kube-system ad alanında çalışıp çalışmadığını denetleyin. Çalışır durumda değilse, pod silin ve yeniden başlatılır.

### <a name="i-could-not-get-logs-using-kubectl-logs-or-cannot-connect-to-the-api-server-getting-the-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Kubectl günlüklerini kullanarak günlükleri alınamadı veya API alınırken sunucu bağlanılamıyor "sunucu hatası: hata arama arka uç: tcp... arama". Ne yapmalıyım?

Varsayılan NSG değiştirilmez ve bağlantı noktası 22 olduğundan bağlantı API sunucusu için açık emin olun. Tunnelfront pod kube-system ad alanında çalışıp çalışmadığını denetleyin. Yüklü değilse, onu zorla silin ve yeniden başlatılır.

### <a name="i-am-trying-to-upgrade-or-scale-and-am-getting-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-issue"></a>Yükseltme veya ölçeklendirme hale getirmeye çalışıyorum ve "ileti" alıyorum: "'Imagereference' özelliğinin değiştirilmesine verilmez." Hata.  Bu sorunu nasıl düzeltebilirim?

AKS kümesi içindeki aracı düğümleri etiketleri değiştiğinden bu hatayı alıyorsanız mümkündür. Değiştirme ve silme etiketleri ve diğer özellikleri MC_ * kaynak grubundaki kaynakların beklenmeyen sonuçlara neden olabilir. MC_ * AKS kümesinde kaynaklarınıza değiştirme SLO keser.


