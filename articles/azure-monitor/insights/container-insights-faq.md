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
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: 70712b68cc26a461f702850dbf6064b65b4ff130
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494685"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Azure İzleyici kapsayıcılar hakkında sık sorulan sorular için

Bu Microsoft FAQ, kapsayıcılar için Azure İzleyici hakkında sık sorulan soruların listesini içerir. Çözümü hakkında ek sorularınız varsa, Git [tartışma forumuna](https://feedback.azure.com/forums/34192--general-feedback) ve sorularınızı gönderin. Sık sorulan bir soru, böylece hızla ve kolayca bulunabilir, bu makaleye ekleriz.

## <a name="why-dont-i-see-data-in-my-log-analytics-workspace"></a>Log Analytics çalışma Alanım içinde veri neden göremiyorum?

Log Analytics çalışma alanı bir, belirli bir zaman her gün içinde herhangi bir veri bulamıyorsanız, varsayılan 500 MB sınırı veya belirtilen günlük toplanacak veri miktarını denetlemek için günlük üst sınır ulaşmış olabilirsiniz. Sınırı gün için karşılandığında, veri toplamayı durdurur ve yalnızca sonraki günü sürdürür. Veri kullanımınızı gözden geçirmek ve, beklenen kullanım düzenlerini esas alarak farklı bir fiyatlandırma katmanına güncelleştirmek için bkz: [günlük veri kullanım ve maliyet](../platform/manage-cost-storage.md). 

## <a name="what-are-the-container-states-specified-in-the-containerinventory-table"></a>ContainerInventory tabloda belirtilen kapsayıcı durumları nelerdir?

ContainerInventory tablo durduruldu ve çalışan kapsayıcılar hakkında bilgi içerir. Tablo, bir iş akışı içinde (çalıştıran ve durduruldu) tüm kapsayıcılar için docker sorgulayan ve verileri Log Analytics çalışma alanı iletir aracı tarafından doldurulur.
 
## <a name="how-do-i-resolve-missing-subscription-registration-error"></a>Ne giderebilirim **eksik abonelik kaydı** hata?

Hatasını alırsanız **Microsoft.OperationsManagement için eksik abonelik kaydı**, kaynak Sağlayıcısı'na kaydederek çözebilirsiniz **Microsoft.OperationsManagement** içinde Çalışma alanı tanımlandığı abonelik. Bunu yapmak oluşturmaya ilişkin belgeler bulunabilir [burada](../../azure-resource-manager/resource-manager-register-provider-errors.md).

## <a name="is-there-support-for-rbac-enabled-aks-clusters"></a>RBAC etkin AKS kümeleri için destek var mı?

Kapsayıcı izleme çözümü RBAC desteklemez, ancak kapsayıcılar için Azure İzleyici ile desteklenir. Çözüm Ayrıntıları sayfasında doğru bilgileri bu kümeleri için verileri gösteren dikey pencereleri gösterilmeyebilir.

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

İçin sorgularken bu veri günlükleri için Azure İzleyici'de aşağıdaki örnekteki gibi görünür:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Sorunun ayrıntılı bilgi için aşağıdakileri gözden geçirin [github bağlantısı](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-ad-errors-when-i-enable-live-logs"></a>Ben Canlı günlükler etkinleştirdiğinizde, Azure AD hataları nasıl giderebilirim? 

Şu hatayı görebilirsiniz: **Yanıt URL'si istekte belirtilen uygulama için yapılandırılan yanıt URL'lerinden eşleşmiyor: ' < Uygulama Kimliği\>'**. Bunu çözmek için çözüm makalesinde bulunabilir [kapsayıcılar için Azure İzleyici ile kapsayıcı günlükleri gerçek zamanlı görüntüleme](container-insights-live-logs.md#configure-aks-with-azure-active-directory). 

## <a name="why-cant-i-upgrade-cluster-after-onboarding"></a>Neden ı ekledikten sonra küme yükseltemiyorum?

Bir AKS kümesi için kapsayıcılar için Azure İzleyici'ı etkinleştirdikten sonra Log Analytics çalışma alanını silerseniz Küme verilerini gönderme olduğu için kümeyi yükseltmeye çalışırken başarısız olur. Bu sorunu çözmek için izlemeyi devre dışı bırakın ve başka bir geçerli çalışma alanına aboneliğinizdeki başvuran yeniden etkinleştirmeniz gerekir. Küme yükseltme yeniden gerçekleştirmeye çalıştığınızda, işlem ve başarıyla tamamlanması gerekir.  

## <a name="next-steps"></a>Sonraki adımlar

AKS kümenizi izlemeye başlamak için gözden [nasıl eklenirim Azure izlemek için kapsayıcılar](container-insights-onboard.md) izlemeyi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemlerin anlaşılması. 