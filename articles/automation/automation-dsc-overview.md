---
title: Azure Automation DSC genel bakış
description: Bir genel bakış, Azure Otomasyonu Desired State Configuration (DSC), terimleri ve bilinen sorunlar
keywords: PowerShell dsc, istenen durum yapılandırması, powershell dsc azure
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f6d49ffa59ed53c0a1966a4132fd5fe1689a13ce
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247358"
---
# <a name="azure-automation-dsc-overview"></a>Azure Automation DSC genel bakış

Azure Automation DSC, yazmak, yönetmek ve PowerShell Desired State Configuration (DSC) derleme olanak tanıyan bir Azure hizmetidir [yapılandırmaları](https://msdn.microsoft.com/powershell/dsc/configurations), içeri aktarma [DSC kaynakları](https://msdn.microsoft.com/powershell/dsc/resources)ve yapılandırmaları Ata Hedef düğümler için tümü buluttaki.

## <a name="why-use-azure-automation-dsc"></a>Neden Azure Automation DSC kullanma

Azure Otomasyonu DSC üzerinden Azure dışındaki DSC kullanarak çeşitli avantajlar sağlar.

### <a name="built-in-pull-server"></a>Yerleşik çekme sunucusu

Azure Otomasyonu, benzer bir DSC çekme sunucusu sağlar [Windows özelliği DSC hizmet](/powershell/dsc/pullserver) hedef düğümler otomatik olarak yapılandırmaları alabilmesi adına, istenen duruma uyumlu hale gelir ve uyumlulukları hakkında geri rapor.
Azure automation'da yerleşik çekme sunucusu ayarlama ve kendi çekme sunucunuz ihtiyacını ortadan kaldırır.
Azure Otomasyonu, sanal veya fiziksel Windows veya Linux makineleri, bulutta veya şirket içi hedefleyebilirsiniz.

### <a name="management-of-all-your-dsc-artifacts"></a>Tüm DSC yapıların Yönetimi

Azure Otomasyonu DSC getirir yönetim katmanının aynısını [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) Azure Otomasyonu, PowerShell betikleri için sunduğu.

Azure portaldan veya powershell'den, tüm, DSC yapılandırmaları, kaynak ve hedef düğümler olarak yönetebilirsiniz.

![Azure Otomasyonu dikey penceresinin ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Log Analytics'e raporlama verilerini içeri aktarma

Azure Otomasyonu DSC ile yönetilen düğümler ayrıntılı raporlama Durum verilerini yerleşik çekme sunucusuna gönderir.
Log Analytics çalışma alanınıza bu veri göndermek için Azure Automation DSC yapılandırabilirsiniz.
Log Analytics çalışma alanınıza DSC Durum verilerini gönderme hakkında bilgi edinmek için bkz: [İleri Azure Otomasyon raporlama verilerini Log Analytics'e DSC](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Tanıtım videosu

Okumak yerine izlemeyi mi tercih ediyorsunuz? Azure Otomasyonu DSC ilk zaman Duyuruldu Mayıs 2015 aşağıdaki videoya göz vardır.

>[!NOTE]
>Bu videoda ele alınan yaşam döngüsü ve kavramları doğru olsa da, bu videonun kayda alındığı beri Azure Automation DSC çok ilerledikten.
>Genel kullanıma sunulmuştur, Azure portalında bir çok daha geniş bir kullanıcı Arabirimine sahiptir ve çok sayıda ek özellikleri destekler.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl Azure Automation DSC ile yönetilmek üzere ekleme düğümleri için bkz: [makineleri Azure Automation DSC tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)
* Azure Automation DSC kullanmaya başlamak için bkz: [Azure Automation DSC ile çalışmaya başlama](automation-dsc-getting-started.md)
* Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [Azure Automation DSC yapılandırmaları derleme](automation-dsc-compile.md)
* Azure Automation DSC için PowerShell cmdlet başvurusu için bkz. [Azure Automation DSC cmdlet'leri](/powershell/module/azurerm.automation/#automation)
* Fiyatlandırma bilgileri için bkz: [Azure Automation DSC fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
* Bir sürekli dağıtım işlem hattı, Azure Automation DSC kullanma örneği için bkz [Iaas Vm'leri kullanarak Azure Automation DSC ve Chocolatey sürekli dağıtım](automation-dsc-cd-chocolatey.md)
