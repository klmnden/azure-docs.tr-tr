---
title: Uygulama yükseltme konuları Gelişmiş | Microsoft Docs
description: Bu makalede, Service Fabric uygulaması yükseltmeyle ilgili bazı gelişmiş konular yer almaktadır.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 168d944f72c1409b5b69c9ab7c07f7fcfa04c7c8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Service Fabric uygulama yükseltme: Gelişmiş konular
## <a name="adding-or-removing-service-types-during-an-application-upgrade"></a>Ekleme veya uygulama yükseltme sırasında hizmet türleri kaldırma
Yükseltme işleminin bir parçası olarak yayımlanan uygulama için yeni bir hizmet türünün eklediyseniz, yeni hizmet türü için dağıtılmış uygulamanın eklenir. Bu tür yükseltme zaten uygulamanın parçası olan hizmet örnekleri etkilemez, ancak eklenmiş olan hizmet türü örneği etkin olması yeni hizmet türü için oluşturulmuş olması gerekir (bkz [yeni ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)).

Benzer şekilde, yükseltme işleminin bir parçası olarak bir uygulamadan hizmet türleri kaldırılabilir. Ancak, to-edilecek kaldırmış hizmet türünün tüm hizmet örneklerini yükseltmeye devam etmeden önce kaldırılması gerekir (bkz [Kaldır ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice?view=azureservicefabricps)).

## <a name="manual-upgrade-mode"></a>El ile yükseltme moduna
> [!NOTE]
> *İzlenen* yükseltme modu için tüm Service Fabric yükseltmelerinin önerilir.
> *UnmonitoredManual* yükseltme modu başarısız veya askıya alınmış yükseltmeleri için yalnızca sayılacağı. 
>
>

İçinde *izlenen* modunu, Service Fabric uygulama yükseltme ilerledikçe sağlıklı olduğundan emin olmak için sistem durumu ilkeleri uygular. Sistem durumu ilkeleri ihlal sonra yükseltme askıya alındı veya otomatik olarak belirtilen bağlı olarak geri *FailureAction*.

İçinde *UnmonitoredManual* modu, uygulama Yöneticisi yükseltmenin ilerleyişini toplam denetime sahiptir. Bu mod kullanışlıdır özel sistem durumu değerlendirme ilkeleri uygulayarak veya durum tamamen izleme atlamak için geleneksel olmayan yükseltme gerçekleştirme (örn. uygulama veri kaybına zaten). Bu modda çalışan bir yükseltme kendisini her UD tamamladıktan sonra askıya alınır ve açıkça kullanarak devam ettirilebilir gerekir [Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps). Ne zaman yükseltme askıya alınır ve kullanıcı tarafından devam etmeye hazır, yükseltme durumu gösterecektir *RollforwardPending* (bkz [UpgradeState](https://docs.microsoft.com/dotnet/api/system.fabric.applicationupgradestate?view=azure-dotnet)).

Son olarak, *UnmonitoredAuto* modu hızlı yükseltme yineleme hizmeti geliştirme veya test sırasında hiçbir kullanıcı girişi gerekmez ve hiçbir uygulama sistem durumu ilkeleri değerlendirilir beri gerçekleştirmek için yararlıdır.

## <a name="upgrade-with-a-diff-package"></a>Diff paketi ile yükseltme
Bir tam uygulama paketi sağlama yerine yükseltme Ayrıca hizmet bildirimlerini tamamlamak ve yalnızca güncelleştirilmiş kod/config/veri paketleri tam uygulama bildirimi birlikte içeren fark paketleri sağlama tarafından gerçekleştirilebilir. Tam uygulama paketleri, yalnızca ilk kümeye bir uygulamanın yüklenmesi için gerekli. Sonraki yükseltmeleri, tam uygulama paketleri veya fark paketleri ya da olabilir.  

Uygulama bildirimi veya uygulama paketinde bulunan bir fark paketin hizmet bildirimlerini herhangi başvurusu şu anda sağlanan sürümüyle otomatik olarak değiştirilir.

Kullanarak bir fark paketi için senaryolar şunlardır:

* Büyük uygulama paketine sahip olduğunda, birkaç hizmet bildirim dosyaları ve/veya birkaç kod paketler, yapılandırma paketleri veya veri paketleri başvuruda bulunuyor.
* Varsa doğrudan uygulamanızdan yapı düzeni oluşturur bir dağıtım sistemi yapı işlemi. Bu durumda, kod değişmediğinden olsa bile, yeni oluşturulan derlemeleri farklı bir sağlama toplamı alın. Bir tam uygulama paketini kullanarak tüm kod paketler sürümüne güncelleştirmenizi gerektirir. Fark paketini kullanarak, yalnızca değiştirilen dosyaların ve sürüm değiştiği bildirim dosyaları sağlar.

Visual Studio kullanarak bir uygulama yükseltildiğinde, bir fark paketi otomatik olarak yayımlanır. Bir fark paketi el ile oluşturmak için uygulama bildirimi ve hizmet bildirimlerini güncelleştirilmesi gerekir, ancak yalnızca değiştirilen paket son uygulama paketinde eklenmelidir.

Örneğin, aşağıdaki uygulama (sürüm numaraları anlama kolaylığı için sağlanan) ile başlayalım:

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Diff paketini kullanarak yalnızca kod paketi service1 güncelleştirmek istediğinizi varsayalım. Güncelleştirilmiş uygulamanızı aşağıdaki sürüm değişiklikler vardır:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Bu durumda, 2.0.0 ve kod paketi güncelleştirmesini yansıtacak şekilde service1 için hizmet bildirimi için uygulama bildirimini güncelleştirin. Uygulama paketinizi klasörünü aşağıdaki yapıya sahip olur:

```text
app1/
  service1/
    code/
```

Diğer bir deyişle, bir tam uygulama paketi normalde oluşturun, sonra sürüm değişmediğini kod/config/veri paketi klasörleri kaldırın.

## <a name="rolling-back-application-upgrades"></a>Uygulama yükseltme geri alınıyor

Yükseltmeler İleri üç modlarından birini alınabilir sırada (*izlenen*, *UnmonitoredAuto*, veya *UnmonitoredManual*), bunlar yalnızca geri ya da alınabilmesiiçin*UnmonitoredAuto* veya *UnmonitoredManual* modu. Çalışırken geri *UnmonitoredAuto* modu İleri şu özel durum ile çalışırken olarak aynı şekilde çalışır, varsayılan değeri *UpgradeReplicaSetCheckTimeout* farklı - bkz [uygulama Yükseltme parametreleri](service-fabric-application-upgrade-parameters.md). Çalışırken geri *UnmonitoredManual* modu İleri alma olarak aynı şekilde çalışır - geri kendisini her UD tamamladıktan sonra askıya alınır ve açıkça kullanarak devam ettirilebilir gerekir [ Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) geri alma işlemine devam etmek için.

Geri alma tetiklenen otomatik olarak zaman sistem durumu ilkeleri yükseltme işleminin *izlenen* moduyla bir *FailureAction* , *geri alma* ihlal ( bakın[Uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md)) veya açıkça kullanarak [başlangıç ServiceFabricApplicationRollback](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback?view=azureservicefabricps).

Geri alma, değerini sırasında *UpgradeReplicaSetCheckTimeout* ve modu hala kullanarak her zaman değiştirilebilir [güncelleştirme ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade?view=azureservicefabricps).

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Uygulama yükseltme nasıl kullanılacağını öğrenerek uyumlu hale getirmek [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [sorun giderme uygulama yükseltmelerini](service-fabric-application-upgrade-troubleshooting.md).
