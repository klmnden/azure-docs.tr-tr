---
title: "Uygulama yükseltme konuları Gelişmiş | Microsoft Docs"
description: "Bu makalede, Service Fabric uygulaması yükseltmeyle ilgili bazı gelişmiş konular yer almaktadır."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: 8d3b922f3d50b645ac9db2cc879a319df1262e0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Service Fabric uygulama yükseltme: Gelişmiş konular
## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Ekleme veya uygulama yükseltme sırasında hizmetleri kaldırılıyor
Yeni bir hizmet zaten dağıtılmış ve bir yükseltme olarak yayımlanan uygulamaya eklenirse, dağıtılan bir uygulama için yeni hizmet eklenir.  Bu tür yükseltme zaten uygulamanın parçası olan hizmetleri etkilemez. Ancak, yeni hizmetinin etkin olması için eklendi Hizmeti'nin bir örneğini başlatılmalıdır (kullanarak `New-ServiceFabricService` cmdlet'i).

Hizmetleri, yükseltme işleminin bir parçası olarak uygulamadan de kaldırılabilir. Ancak, geçerli hizmetin tüm örneklerine ait to-edilecek silinmiş yükseltmeye devam etmeden önce durdurulması gerekir (kullanarak `Remove-ServiceFabricService` cmdlet'i).

## <a name="manual-upgrade-mode"></a>El ile yükseltme moduna
> [!NOTE]
> İzlenmeyen el ile modu yalnızca başarısız veya askıya alınmış yükseltme için dikkate alınmalıdır. Service Fabric uygulamaları için önerilen yükseltme modu izlenen moddur.
>
>

Azure Service Fabric geliştirme ve üretim kümeleri desteklemek için birden fazla yükseltme modu sağlar. Seçilen dağıtım seçenekleri farklı ortamlar için farklı olabilir.

İzlenen uygulama yükseltme, üretim ortamında kullanmak için en tipik bir yükseltmedir. Yükseltme İlkesi belirtildiğinde, Service Fabric yükseltme devam etmeden önce uygulama sağlıklı olmasını sağlar.

 Uygulama Yöneticisi tarafından el ile çalışırken uygulama yükseltme modu çeşitli yükseltme etki alanları arasında yükseltme işlemi ilerleme durumu üzerinde toplam denetime sahip olmasını kullanabilirsiniz. Bu mod özelleştirilmiş veya karmaşık sistem durumu değerlendirme İlkesi gereklidir veya Geleneksel olmayan yükseltme gerçekleştiği yararlıdır (örneğin, uygulama veri kaybına zaten).

Son olarak, geliştirme veya hızlı yineleme döngüsü hizmeti geliştirme sırasında sağlamak için sınama ortamları için otomatik uygulama yükseltme kullanışlıdır.

## <a name="change-to-manual-upgrade-mode"></a>El ile yükseltme moduna geçin
**El ile**--geçerli UD uygulama yükseltmeyi durdurun ve yükseltme modu izlenmeyen el ile olarak değiştirin. Yönetici el ile çağırmayı gerektiren **MoveNextApplicationUpgradeDomainAsync** yükseltme işlemine devam veya yeni bir yükseltme başlatarak bir geri alma tetiklemek için. El ile moduna yükseltme girdikten sonra yeni bir yükseltme başlatılana kadar el ile modunda kalır. **GetApplicationUpgradeProgressAsync** komut döndürür DOKU\_uygulama\_yükseltme\_durumu\_çalışırken\_İleri\_BEKLEMEDE.

## <a name="upgrade-with-a-diff-package"></a>Diff paketi ile yükseltme
Service Fabric uygulaması, tam ve müstakil uygulama paketiyle sağlama tarafından yükseltilebilir. Yalnızca güncelleştirilmiş uygulama dosyaları, güncelleştirilmiş uygulama bildirimi ve hizmet bildirim dosyalarını içeren bir fark paketi kullanarak bir uygulama da yükseltilebilir.

Bir tam uygulama paketi başlatmak ve Service Fabric uygulaması çalıştırmak gereken tüm dosyaları içerir. Bir fark paketi son sağlamak ve geçerli yükseltme arasında değişen dosyaları içerir ve ayrıca dosyaları tam uygulama bildirimi ve hizmet bildirimi. Uygulama bildirimini veya yapı düzende bulunamıyor hizmet bildirimi herhangi başvurusu için görüntü deposunda aranır.

Tam uygulama paketleri ilk kümeye bir uygulamanın yüklenmesi için gereklidir. Sonraki güncelleştirmeler tam uygulama paketi veya bir fark paket olabilir.

Durumlar fark paket iyi bir seçimdir kullanırken:

* Birkaç hizmet bildirim dosyaları ve/veya birkaç kod paketler, yapılandırma paketleri veya veri paketleri başvuruda bulunan bir geniş Uygulama paketiniz varsa, bir fark paket tercih edilir.
* Yapı düzeni doğrudan, uygulama derleme işlemini oluşturan bir dağıtım sistemi olduğunda bir fark paket tercih edilir. Bu durumda, kod değişmediğinden olsa bile, yeni oluşturulan derlemeleri farklı bir sağlama toplamı alın. Bir tam uygulama paketini kullanarak tüm kod paketler sürümüne güncelleştirmenizi gerektirir. Fark paketini kullanarak, yalnızca değiştirilen dosyaların ve sürüm değiştiği bildirim dosyaları sağlar.

Visual Studio kullanarak bir uygulama yükseltildiğinde, fark paketini otomatik olarak yayımlanır. Bir fark paketi el ile oluşturmak için uygulama bildirimi ve hizmet bildirimlerini güncelleştirilmesi gerekir, ancak yalnızca değiştirilen paket son uygulama paketinde eklenmelidir.

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

Şimdi, PowerShell kullanarak bir fark paketini kullanarak yalnızca kod paketi service1 güncelleştirmek istediğiniz varsayalım. Şimdi, güncelleştirilmiş uygulamanızı aşağıdaki klasör yapısını sahiptir:

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

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Uygulama yükseltme nasıl kullanılacağını öğrenerek uyumlu hale getirmek [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [sorun giderme uygulama yükseltmelerini](service-fabric-application-upgrade-troubleshooting.md).
