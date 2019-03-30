---
title: Gelişmiş uygulama yükseltme konuları | Microsoft Docs
description: Bu makalede, Service Fabric uygulaması yükseltme hakkında bazı gelişmiş konular ele alınmaktadır.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 3cdddac74552b56dfe3567adf30f1a05b6eb8e24
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663800"
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Service Fabric uygulama yükseltme: Gelişmiş konular
## <a name="adding-or-removing-service-types-during-an-application-upgrade"></a>Ekleme veya uygulama yükseltme sırasında hizmet türlerini kaldırma
Daha sonra yeni bir hizmet türü bir yükseltmesinin bir parçası olarak yayımlanan bir uygulamanın eklenen, yeni hizmet türü dağıtılan uygulamanın konumuna eklenir. Böyle bir yükseltme herhangi bir uygulamanın parçası olan hizmet örnekleri etkilemez, ancak etkin olmak için yeni hizmet türü eklenen hizmet türü örneği oluşturulmalıdır (bkz [New-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)).

Benzer şekilde, hizmet türlerinin bir yükseltmesinin bir parçası olarak bir uygulamadan kaldırılabilir. Ancak, How-to-edilecek kaldırıldı hizmet türünün tüm hizmet örneklerini yükseltmeye devam etmeden önce kaldırılması gerekir (bkz [Remove-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice?view=azureservicefabricps)).

## <a name="manual-upgrade-mode"></a>El ile yükseltme modu
> [!NOTE]
> *İzlenen* yükseltme modu, tüm Service Fabric yükseltmelerinin için önerilir.
> *UnmonitoredManual* yükseltme modu için başarısız olan veya askıya alınmış yükseltmeleri yalnızca sayılacağı. 
>
>

İçinde *izlenen* modu, Service Fabric uygulaması yükseltme ilerledikçe sağlıklı olduğundan emin olmak için sistem durumu ilkeleri uygular. Sistem durumu ilkeleri ihlal sonra yükseltme askıya veya otomatik olarak belirtilen bağlı olarak geri *FailureAction*.

İçinde *UnmonitoredManual* modu, uygulama yöneticisi olan yükseltmenin ilerleme üzerinde tam denetim sahibi. Bu mod kullanışlıdır özel sistem durumu değerlendirme ilkeleri uygulama veya sistem durumu izleme tamamen atlamak için geleneksel olmayan yükseltmeleri gerçekleştirme (örneğin uygulama veri kaybı zaten). Bu modda çalışan bir yükseltme kendisini her UD tamamladıktan sonra askıya alırız ve açıkça kullanarak devam ettirilebilir gerekir [sürdürme ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps). Ne zaman yükseltme askıya alınır ve kullanıcı tarafından devam ettirilebilir hazır, yükseltme durumunu gösterecektir *RollforwardPending* (bkz [UpgradeState](https://docs.microsoft.com/dotnet/api/system.fabric.applicationupgradestate?view=azure-dotnet)).

Son olarak, *UnmonitoredAuto* modu, hızlı yükseltme yineleme hizmeti geliştirme veya test sırasında kullanıcı müdahalesi gerekli değildir ve hiçbir uygulama sistem durumu ilkeleri değerlendirilir gerçekleştirmek için kullanışlıdır.

## <a name="upgrade-with-a-diff-package"></a>Bir fark paketi ile yükseltme
Tam uygulama paketini sağlama yerine yükseltme de sağlama yalnızca güncelleştirilmiş kodun/config/veri paketleri tam uygulama bildiriminin yanı sıra içeren ve hizmet bildirimleri tamamlamak fark paketlerinin tarafından gerçekleştirilebilir. Tüm uygulama paketleri, yalnızca bir uygulamayı kümeye ilk yüklenmesi için gereklidir. Sonraki yükseltmeleri, tam uygulama paketleri veya fark paketleri ya da olabilir.  

Herhangi bir uygulama bildirimi ya da hizmet bildirimleri, uygulama paketinde bulunamayan fark paketi başvurusu otomatik olarak şu anda sağlanan sürümle değiştirilir.

Bir fark paket kullanmaya yönelik senaryolar şunlardır:

* Büyük uygulama paketine sahip olduğunuzda, birkaç hizmet bildirim dosyaları ve/veya birkaç kod paketleri, yapılandırma paketlerini veya veri paketleri başvuruyor.
* Varsa doğrudan uygulama içinden yapı düzenini oluşturan bir dağıtım sistem işlemi oluşturun. Bu durumda, kod değişmediğinden olsa da, yeni oluşturulan derlemeler farklı bir sağlama toplamı alın. Tam uygulama paketini kullanarak, tüm kod paketlerinin sürümüne güncelleştirmenizi gerektirir. Bir fark paketini kullanarak, yalnızca değişen dosyaları ve sürüm değiştiği bildirim dosyaları sağlar.

Visual Studio kullanarak bir uygulama yükseltildiğinde fark paket otomatik olarak yayımlanır. Bir fark paketi el ile oluşturmak için uygulama bildiriminin ve hizmet bildirimleri güncelleştirilmesi gerekir, ancak yalnızca değiştirilen paketler son uygulama paketinde eklenmelidir.

Örneğin, aşağıdaki uygulama (anlama kolaylığı için sağlanan sürüm numaraları) ile başlayalım:

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Yalnızca bir fark paketi kullanarak kod paketi service1 örneklerini güncelleştirmek istediğinizi varsayalım. Güncelleştirilen uygulamanızı aşağıdaki sürüm değişikliklerini sahiptir:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Bu durumda, 2.0.0 ve kod Paketi güncelleştirmesi yansıtacak şekilde service1 için hizmet bildiriminin uygulama bildirimini güncelleştirin. Klasör, uygulama paketi için aşağıdaki yapıya sahip olur:

```text
app1/
  service1/
    code/
```

Diğer bir deyişle, bir tam uygulama paketi normalde oluşturun, ardından sürüm değiştirilmedi herhangi bir kod/config/veri paketi klasör kaldırın.

## <a name="rolling-back-application-upgrades"></a>Uygulama yükseltmeleri geri alınıyor

Yükseltmeler İleri üç moddan birini alınması sırasında (*izlenen*, *UnmonitoredAuto*, veya *UnmonitoredManual*), bunlar yalnızca geri içinde ya da alınması*UnmonitoredAuto* veya *UnmonitoredManual* modu. Çalışırken geri *UnmonitoredAuto* modu, şu özel durum ile ileri sarmadır olarak aynı şekilde çalışır, varsayılan değerini *UpgradeReplicaSetCheckTimeout* farklı - bkz [uygulama Yükseltme parametreleri](service-fabric-application-upgrade-parameters.md). Çalışırken geri *UnmonitoredManual* modu İleri sarmadır olarak aynı şekilde çalışır - geri alma kendisini her UD tamamladıktan sonra askıya alırız ve açıkça kullanarak devam ettirilebilir gerekir [ Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) geri alma işlemine devam etmek için.

Geri alma işlemleri tetiklenen otomatik olarak zaman içinde bir yükseltme sistem durumu ilkeleri *izlenen* modu ile bir *FailureAction* , *geri alma* ihlal ( bkz[Uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md)) veya açıkça kullanarak [başlangıç ServiceFabricApplicationRollback](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback?view=azureservicefabricps).

Değerini geri alma sırasında *UpgradeReplicaSetCheckTimeout* ve modu hala kullanarak istediğiniz zaman değiştirilebilir [güncelleştirme ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade?view=azureservicefabricps).

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamayı kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Nasıl kullanılacağını öğrenerek, uygulama yükseltmeleri uyumlu olma [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).
