---
title: Azure Service Fabric kod paketi hataları tanılamak | Microsoft Docs
description: Azure Service Fabric ile ortak kod paketi hatalarında sorun giderme hakkında bilgi edinin
services: service-fabric
documentationcenter: .net
author: grzuber
manager: gkhanna
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/09/2019
ms.author: grzuber
ms.openlocfilehash: 235952388d2c044cc141b3020c67944c4250ea3d
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276957"
---
# <a name="diagnose-common-code-package-errors-with-service-fabric"></a>Service Fabric ile ortak kod paketi hataları tanılama

Bu makalede, bir kod paketi beklenmedik şekilde sonlandırılması ne anlama geldiğini gösterir ve olası nedeni büyük olasılıkla sorunu hafifletmeye sorun giderme adımlarını yanı sıra sık karşılaşılan hata kodları hakkında Öngörüler sağlar.

## <a name="when-does-a-process-or-container-terminate-unexpectedly"></a>Ne zaman bir işleme veya kapsayıcıya beklenmedik şekilde sonlandırılması?

Service Fabric kod paketi başlatmak için bir istek aldığında, uygulama ve hizmet bildirimleri yapılandırma seçeneklerini temel yerel sistemde ortamını hazırlama başlar. Bu hazırlıkları ağ uç noktaları veya kaynak ayırma, güvenlik duvarı kuralları yapılandırma veya kaynak İdaresi kısıtlamaları ayarı içerebilir. Ortam düzgün şekilde yapılandırıldıktan sonra Service Fabric kod paketi getirmek çalışır. Bu adım işletim sistemi ya da kapsayıcı çalışma zamanı işleme veya kapsayıcıya başarıyla etkinleştirilmiş olduğunu bildirirse başarılı olarak kabul edilir. Etkinleştirme başarısız olursa, bir sistem durumu ileti yazan SFX görmeniz gerekir

```
There was an error during CodePackage activation. Service host failed to activate. Error: 0xXXXXXXXX
```

Kod paketi başarıyla olana sonra Service Fabric ömrü izlemeye başlar. Bu noktada, bir işlem veya kapsayıcı için bir dizi nedenden herhangi bir zamanda sonlandırabilir. Bu bir DLL başlatmak başarısız bir işletim sistemi çalıştıran vb. Masaüstü yığın alanı yetersiz. Kod paketinizi sonlandırıldıysa bildiren SFX durum iletisinde görmeniz gerekir

```
The process/container terminated with exit code: XXXXXXXX. Please look at your application logs/dump or debug your code package for more details. For information about common termination errors, please visit https://aka.ms/service-fabric-termination-errors
```

Çıkış kodu, ileti, neden sonlandırıldı için farklı işlem/kapsayıcı tarafından sağlanan tek ipucudur ve herhangi bir yığın düzeyi tarafından oluşturulmuş bu sistem durumu sağlanır. Bu çıkış kodu, örneğin, bir .NET sorun, bir işletim sistemi hatasıyla ilgili veya kodunuz tarafından tetiklendiği anlamak zordur. Sonuç olarak, bu makalede sonlandırma çıkış kodları kaynağını tanımak için bir başlangıç noktası olarak kullanılabilir ve olası çözümleri, bu yaygın senaryolar için genel çözümler ve hata, geçerli olmayabilir aklınızda görüyorsunuz.

## <a name="how-can-i-tell-if-service-fabric-terminated-my-code-package"></a>Service Fabric kodu paketimle sonlandırıldı olmadığını nasıl anlayabilirim?

Service Fabric, kod paketi için çeşitli nedenlerle sonlandırılması için sorumlu olabilir. Örneğin, kod paketi başka bir düğüme Yük Dengeleme için yerleştirmek karar verebilir amaçlar. Aşağıdaki tabloda çıkış kodlarından birini görürseniz, kod paketi Service Fabric tarafından sona erdirildi olmadığını söyleyebilir:

>[!NOTE]
> Aşağıdaki tabloda olanlar dışındaki bir çıkış kodu ile işlem/kapsayıcı sona ererse, Service Fabric sonlandırılması için sorumlu değildir.

Çıkış kodu | Açıklama
--------- | -----------
7147 | Bu hata kodları, Service Fabric düzgün bir şekilde işlem/kapsayıcı Ctrl + C sinyali göndererek kapatıldığını gösterir.
7148 | Bu hata kodları, Service Fabric işlem/kapsayıcı sonlandırıldı gösterir. Bazı durumlarda, bu hata kodu sonlandırılan gerekiyordu, dolayısıyla bir Ctrl + C sinyali gönderdikten sonra zamanında işlem/kapsayıcı yanıt vermesini yaramadı belirtebilirsiniz.


## <a name="other-common-error-codes-and-their-potential-fixes"></a>Ortak diğer hata kodları ve bunların olası düzeltmeleri

Çıkış kodu | Onaltılık değer | Kısa açıklama | Kök Neden | Olası düzeltme
--------- | --------- | ----------------- | ---------- | -------------
3221225794 | 0xc0000142 | STATUS_DLL_INIT_FAILED | Bu hata büyük olasılıkla makinenin Masaüstü yığın alanı kalmadı çalıştırıldı gelebilir. Bu neden çok sayıda düğüm üzerinde çalışan uygulamanıza ait işlemler varsa özellikle yüksektir. | Programınız için Ctrl + C sinyali yanıt oluşturulmadıysa, küme bildirimi "EnableActivateNoWindow" ayarını etkinleştirebilirsiniz. Bu ayarın etkinleştirilmesi, kod paketi bir GUI penceresi çalışır ve Ctrl + C sinyali almaz, ancak her işlemin kullandığı Masaüstü yığın alanı miktarını azaltır anlamına gelir. Ardından Ctrl + C sinyali almak, kod paketi erişmesi gerekiyorsa, düğümün Masaüstü yığın boyutunu artırabilir.
3762504530 | 0xe0434352 | Yok | Bu değer, yönetilen koddan (.NET) işlenmeyen bir özel durum için hata kodudur. | Bu çıkış kodu görüyorsanız, uygulamanızı işlenmemiş kalan ve işlem sonlandırıldı, bir özel durum harekete geçirilen anlamına gelir. Uygulamanızın günlükleri ve dökümlerinde hata ayıklama, hatanın nedenini belirlemek için ilk adım olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [diğer yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md)
* Azure İzleyici günlüklerine ve hangi okuyarak sunduğu daha ayrıntılı bir bakış elde [Azure İzleyici günlüklerine nedir?](../operations-management-suite/operations-management-suite-overview.md)
* Azure İzleyici günlüklerine hakkında daha fazla bilgi [uyarı](../log-analytics/log-analytics-alerts.md) algılama ve tanılama konusunda yardımcı olacak.
* Analytics'in [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri, Azure İzleyici günlüklerine bir parçası olarak sunulan
