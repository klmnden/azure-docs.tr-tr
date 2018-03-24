---
title: Azure Otomasyonu DSC genel bakış
description: Bir genel bakış, Azure Otomasyonu istenen durum yapılandırması (DSC), koşulları ve bilinen sorunlar
keywords: PowerShell dsc, istenen durum yapılandırması, powershell dsc azure
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 3949b79c3729ecdc2dfdd6297a5f10852e061540
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-automation-dsc-overview"></a>Azure Otomasyonu DSC genel bakış

Azure Otomasyonu DSC, yazma, yönetmek ve PowerShell istenen durum yapılandırması (DSC) derlemek için sağlayan bir Azure hizmetidir [yapılandırmaları](https://msdn.microsoft.com/powershell/dsc/configurations), içeri aktarma [DSC kaynakları](https://msdn.microsoft.com/powershell/dsc/resources)ve tüm bulutta hedef düğümleri yapılandırmaları atayabilirsiniz.

## <a name="why-use-azure-automation-dsc"></a>Azure Otomasyonu DSC neden kullanılır?

Azure Otomasyonu DSC Azure dışında DSC kullanarak birkaç avantajı sağlar.

### <a name="built-in-pull-server"></a>Yerleşik çekme sunucu

Azure Automation'ın sağladığı bir [DSC istek sunucusuyla](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) hedef düğümleri otomatik olarak yapılandırmaları alması için bunları, belirtilen istenen duruma uyumlu ve uyumlulukları hakkında rapor.
Yerleşik çekme sunucunun Azure Automation ayarlama ve kendi çekme sunucunuz gereğini ortadan kaldırır.
Azure Otomasyonu sanal veya fiziksel Windows veya Linux makineler, bulutta veya şirket içi hedefleyebilirsiniz.

### <a name="management-of-all-your-dsc-artifacts"></a>Tüm DSC yapıları Yönetimi

Azure Otomasyonu DSC aynı yönetim katmanı getirir [PowerShell istenen durum Yapılandırması](https://msdn.microsoft.com/powershell/dsc/overview) gibi Azure Otomasyonu PowerShell komut dosyası için sunar.

Azure portal veya PowerShell, tüm, DSC yapılandırmalarını, kaynak ve hedef düğümleri yönetebilirsiniz.

![Azure Otomasyonu dikey penceresi ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Günlük analizi raporlama verilerini alma

Azure Otomasyonu DSC'ye yönetilen düğümler ayrıntılı raporlama Durum verilerini yerleşik çekme sunucusuna gönderir.
Bu veriler için günlük analizi çalışma alanınız göndermek için Azure Otomasyonu DSC yapılandırabilirsiniz.
Günlük analizi çalışma alanına DSC Durum verilerini gönderme hakkında bilgi edinmek için bkz: [İleri Azure otomasyonu için günlük analizi veri raporlama DSC](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Tanıtım videosu

Okumak yerine izlemeyi mi tercih ediyorsunuz? Aşağıdaki video Mayıs 2015, ne zaman Azure Otomasyonu DSC ilk duyurulan göz vardır.

>[!NOTE]
>Kavramlar ve bu videoda ele alınan ömrünü doğru olsa da, bu videonun kaydedilmesinden sonra Azure Otomasyonu DSC değişmiştir.
>Genel kullanıma sunulmuştur, Azure Portal'da daha geniş bir kullanıcı Arabirimine sahiptir ve birçok ek özellikleri desteklemektedir.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl Azure Otomasyonu DSC'ye yönetilecek yerleşik düğümleri için bkz: [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md)
* Azure Automation DSC kullanmaya başlamak için bkz: [Azure Otomasyonu DSC ile çalışmaya başlama](automation-dsc-getting-started.md)
* Böylece hedef düğümleri atayabilirsiniz DSC yapılandırmaları derleme hakkında bilgi edinmek için [Azure Otomasyonu DSC yapılandırmalarında derleme](automation-dsc-compile.md)
* Azure Otomasyonu DSC için PowerShell cmdlet başvuru için bkz: [Azure Otomasyonu DSC cmdlet'leri](/powershell/module/azurerm.automation/#automation)
* Fiyatlandırma bilgileri için bkz: [Azure Otomasyonu DSC fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
* Sürekli dağıtım ardışık düzeninde Azure Automation DSC kullanmaya ilişkin bir örnek görmek için bkz: [Iaas VM'ler kullanarak Azure Otomasyonu DSC ve Chocolatey sürekli dağıtım](automation-dsc-cd-chocolatey.md)