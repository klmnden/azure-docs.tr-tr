---
title: Azure izleme ile ilgili sorunları giderme
description: Bu makalede değişiklik izleme sorunlarını giderme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2a6610b5cb3f01fc70b1737fc4492e09d9a7637b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085345"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Değişiklik izleme ve stok sorunlarını giderme

## <a name="windows"></a>Windows

### <a name="records-not-showing-windows"></a>Senaryo: Windows makineleri için değişim izleme kayıtlarının gösteren değil

#### <a name="issue"></a>Sorun

Herhangi bir envanter veya değişiklik izleme sonuç eklenen değişiklik izleme için Windows makineleri için görmüyorum.

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden kaynaklanabilir:

1. **Microsoft Monitoring Agent** çalışmıyor
2. Otomasyon hesabını geri iletişimi engelleniyor.
3. Değişiklik izleme için yönetim paketlerini karşıdan değildir.
4. Eklenen VM'de Sysprep kullanılarak hazırlanmış Microsoft izleme aracısının yüklü olmadığı bir kopyalanan makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

1. Doğrulama **Microsoft Monitoring Agent** (HealthService.exe), makinede çalışıyor.
1. Denetleme **Olay Görüntüleyicisi'ni** Word'ün meydana gelen olayları arayın ve makine `changetracking` bunlara.
1. Ziyaret [ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning) hakkında adresler ve bağlantı noktaları çalışması değişiklik izleme için izin verilmesi gereken öğrenin.
1. Aşağıdaki değişiklik izleme ve stok yönetim paketlerinin yerel olarak mevcut olduğunu doğrulayın:
    * Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*
    * Microsoft.IntelligencePacks.InventoryChangeTracking.*
    * Microsoft.IntelligencePacks.SingletonInventoryCollection.*
1. Klonlanmış bir görüntü sysprep görüntüsü kullanarak ve olaydan sonra Microsoft Monitoring Agent aracıyı yükleyin.

Desteğe başvurun ve bu çözümleri sorununuzu yoksa, aracıda tanılama toplamak için aşağıdaki komutları çalıştırabilirsiniz.

Aracı makinede gidin `C:\Program Files\Microsoft Monitoring Agent\Agent\Tools` ve aşağıdaki komutları çalıştırın:

```cmd
set stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Önceki örnekte, kullanımı gibi ayrıntılı hata iletileri etkinleştirmek istiyorsanız tarafından varsayılan hata izleme, etkin `VER` parametresi. Bilgi izleme için kullanmak `INF` çağrılırken `StartTracing.cmd`.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
