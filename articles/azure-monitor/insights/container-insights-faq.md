---
title: Sık sorulan sorular kapsayıcılar için Azure İzleyici | Microsoft Docs
description: Kapsayıcılar için Azure İzleyici, Azure Container Instances'da ve AKS küme durumunu izleyen bir çözümdür. Bu makalede, sık sorulan soruları yanıtlar.
services: azure-monitor
author: mgoedtel
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2019
ms.author: magoedte
ms.openlocfilehash: 27a191bb62ae59aa154167a22c99d3e699f3eb5a
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418623"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Azure İzleyici kapsayıcılar hakkında sık sorulan sorular için
Bu Microsoft FAQ, kapsayıcılar için Azure İzleyici hakkında sık sorulan soruların listesini içerir. Çözümü hakkında ek sorularınız varsa, Git [tartışma forumuna](https://feedback.azure.com/forums/34192--general-feedback) ve sorularınızı gönderin. Sık sorulan bir soru, böylece hızla ve kolayca bulunabilir, bu makaleye ekleriz.

## <a name="why-am-i-not-seeing-data-if-log-analytics-workspace-is-configured-with-the-free-pricing-tier"></a>Log Analytics çalışma alanı ücretsiz fiyatlandırma katmanı ile yapılandırılmışsa, neden veri görüyorum değil mi? 

Varsayılan 500 MB sınırına veya günlük toplanacak veri miktarını denetlemek için günlük üst sınır belirtilen olabilir. Veri kullanımınızı yönetmek ve denetlemek için bkz: [günlük veri kullanım ve maliyet](../platform/manage-cost-storage.md). 

## <a name="what-are-the-states-of-containers-specified-in-the-containerinventory-table"></a>Kapsayıcıları ContainerInventory tabloda belirtilen durumları nelerdir?
ContainerInventory tablo durduruldu ve çalışan kapsayıcılar hakkında bilgi içerir. Tablo, bir iş akışı içinde (çalıştıran ve durduruldu) tüm kapsayıcılar için docker sorgulayan ve verileri Log Analytics çalışma alanı iletir aracı tarafından doldurulur.
 
## <a name="how-do-i-resolve-errors-related-to-missing-subscription-registration-for-microsoftoperationsmanagement"></a>Nasıl ilgili hataları giderebilirim **Microsoft.OperationsManagement için eksik abonelik kaydı**?
Hatayı gidermek için kaynak sağlayıcısını kaydedin **Microsoft.OperationsManagement** çalışma tanımlandığı aboneliği. Bunu yapmak oluşturmaya ilişkin belgeler bulunabilir [burada](../../azure-resource-manager/resource-manager-register-provider-errors.md).

## <a name="does-azure-monitor-for-containers-include-support-for-rbac-enabled-aks-clusters"></a>Kapsayıcılar için Azure İzleyici, RBAC özelliği etkin AKS kümeleri için destek içeriyor mu?
RBAC etkin AKS kümeleri şu anda desteklenmiyor çözüm tarafından. Çözüm Ayrıntıları sayfasında doğru bilgileri bu kümeleri için verileri gösteren dikey pencereleri gösterilmeyebilir.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Günlük toplama kube-system ad alanında Helm ile kapsayıcılar için nasıl etkinleştirebilirim?
Günlük toplama kapsayıcılardan kube-system ad varsayılan olarak devre dışıdır. Günlük toplama omsagent bir ortam değişkenini ayarlayarak etkinleştirilebilir. Daha fazla bilgi için [kapsayıcılar için Azure İzleyici](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers) GitHub sayfası. 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>En son yayımlanan hali omsagent nasıl güncelleştirebilirim?
Aracı yükseltme öğrenmek için bkz. [aracı Yönetimi](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>Çok satırlı günlüğünü nasıl etkinleştirebilirim?
Çok satırlı günlük kapsayıcılar için Azure İzleyici şu anda desteklememektedir ancak geçici çözümler vardır. JSON biçiminde yazmak için tüm hizmetleri yapılandırabilir ve ardından Docker/Moby bunları tek bir satır olarak yazar.

Örneğin, bir örnek node.js uygulaması için aşağıdaki örnekte gösterildiği gibi bir JSON nesnesi olarak günlüğünüzün kayabilir:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

İçin sorgulama yapıldığında bu veri günlükleri için Azure İzleyici'de aşağıdaki gibi görünür:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Sorunun ayrıntılı bilgi için aşağıdakileri gözden geçirin [github bağlantısı](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-active-directory-errors-when-i-enable-live-logs"></a>Ben Canlı günlükler etkinleştirdiğinizde, Azure Active Directory hatalarını nasıl giderebilirim? 
Şu hatayı görebilirsiniz: **Yanıt URL'si istekte belirtilen uygulama için yapılandırılan yanıt URL'lerinden eşleşmiyor: '60b4dec7-5a69-4165-a211-12c40b5c0435'**. Sorunun makalesinde bulunabilir [kapsayıcılar için Azure İzleyici ile kapsayıcı günlükleri gerçek zamanlı görüntüleme](container-insights-live-logs.md#configure-aks-with-azure-active-directory). 

## <a name="next-steps"></a>Sonraki adımlar
AKS kümenizi izlemeye başlamak için gözden [nasıl eklenirim Azure izlemek için kapsayıcılar](container-insights-onboard.md) izlemeyi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemlerin anlaşılması. 