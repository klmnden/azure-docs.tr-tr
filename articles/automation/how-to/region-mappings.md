---
title: Azure otomasyon ve Log Analytics çalışma alanı eşlemeleri
description: Bu makalede bir Otomasyon hesabı ile Log Analytics çalışma alanı arasında çözümünü desteklemek için izin verilen eşlemeleri
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8afe8fed8912dd365071f1c4c5560c9f5578dcd8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66133111"
---
# <a name="workspace-mappings"></a>Çalışma alanı eşlemeleri

Güncelleştirme yönetimi, değişiklik izleme ve stok veya Vm'leri başlatma/durdurma gibi çözümlerle sırasında yoğun olmayan saatlerde çözümü etkinleştirirken, bir Log Analytics çalışma alanı ve Otomasyon hesabı bağlamak için yalnızca belirli bölgelerde desteklenir. Bu eşleme yalnızca Otomasyon hesabının ve Log Analytics çalışma alanı için de geçerlidir. Otomasyon hesabı veya Log Analytics çalışma alanına raporlama kaynakları başka bölgelerde bulunabilir.

## <a name="supported-mappings"></a>Desteklenen eşlemeleri

Aşağıdaki tabloda, desteklenen eşlemeleri gösterir:

|**Log Analytics çalışma alanı bölgesi**|**Azure Otomasyonu bölge**|
|---|---|
|**ABD**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Kanada**||
|CanadaCentral|CanadaCentral|
|**Asya Pasifik**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|JapanEast|JapanEast|
|**Avrupa**||
|UKSouth|UKSouth|
|WestEurope|WestEurope|
|**ABD Devleti**||
|USGovVirginia|USGovVirginia|

<sup>1</sup> EastUS eşleme Otomasyon hesapları için Log Analytics çalışma alanları için tam bir bölgeden bölgeye eşleme değil, ancak doğru eşleme.

<sup>2</sup> kapasitesi kısıtlamaları nedeniyle bölgeyi yeni kaynakları oluşturulurken kullanılamaz. Bu, Otomasyon hesaplarını ve Log Analytics çalışma alanlarını içerir. Ancak, önceden var olan bağlı kaynaklar bölgede çalışmaya devam.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını Kaldır

Bir Log Analytics çalışma alanı ile Otomasyon hesabınızı tümleştirmek istediğiniz karar verirseniz, doğrudan Azure portalından hesabınızı kesebilir. Devam etmeden önce önce bunları kullanıyorsanız, güncelleştirme yönetimi, değişiklik izleme ve stok veya Vm'leri başlatma/durdurma sırasında yoğun olmayan saatlerde çözümleri kaldırmanız gerekir. Bu işlem, bunları kaldırmazsanız gelen devam etmeden engellenir. Kaldırmak için gerekli adımları anlamak için alınan belirli çözüm makalesini gözden geçirin.

Bu çözümleri kaldırdıktan sonra Otomasyon hesabının bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri dahil olmak üzere bazı çözümler Otomasyon varlıklarından oluşturmuş olabilir ve ayrıca çalışma alanının bağlantısı kaldırılıyor önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve sayfa seçin üzerinde Otomasyon hesabı **bağlantılı çalışma** bölümünde **ilgili kaynakları** soldaki.

2. Bağlantıyı kaldır çalışma sayfasında tıklayın **çalışma alanının bağlantısını Kaldır**. Devam etmek istediğinizi doğrulayan bir ileti alırsınız.

3. Azure Otomasyonu hesabı Log Analytics çalışma alanınızın bağlantısını dener, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.

İsteğe bağlı olarak, güncelleştirme yönetimi çözümünü kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Güncelleştirme zamanlamaları - her oluşturduğunuz güncelleştirme dağıtımlarının eşleşen adlara sahip)

* Çözüm için - oluşturulan karma çalışan grupları her benzer şekilde machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8 için olarak adlandırılır).

İsteğe bağlı olarak, yoğun olmayan saatlerde çözüm sırasında Vm'leri başlatma/durdurma kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Başlatma ve durdurma VM runbook zamanlama
* VM runbook'ları durdurun ve başlatın
* Değişkenler

Alternatif olarak, ayrıca çalışma alanınızı Otomasyon hesabınızdan Log Analytics çalışma alanınızdan kesebilir. Çalışma alanınızda seçin **Otomasyon hesabı** altında **ilgili kaynakları**. Otomasyon hesabı sayfasında **hesabı bağlantısını**.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl eklemek için aşağıdaki çözümleri:

Güncelleştirme yönetimi ve değişiklik izleme ve stok:

* Gelen bir [sanal makine](../automation-onboard-solutions-from-vm.md)
* Öğesinden, [Otomasyon hesabı](../automation-onboard-solutions-from-automation-account.md)
* Zaman [birden çok makine gözatma](../automation-onboard-solutions-from-browse.md)
* Gelen bir [runbook](../automation-onboard-solutions.md)

Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma

* [Kapalı olduğu saatlerde Vm'leri başlatma/durdurma dağıtma](../automation-solution-vm-management.md)