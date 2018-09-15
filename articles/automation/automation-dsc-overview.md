---
title: Azure Otomasyonu durum yapılandırmasına genel bakış
description: Bir genel bakış, Azure Otomasyonu durum yapılandırması (DSC), terimleri ve bilinen sorunlar
keywords: PowerShell dsc, istenen durum yapılandırması, powershell dsc azure
services: automation
ms.service: automation
ms.component: dsc
author: bobbytreed
ms.author: robreed
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ef55e6ca6fc913710bae68a7423369b33f26c009
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629007"
---
# <a name="azure-automation-state-configuration-overview"></a>Azure Otomasyonu durum yapılandırmasına genel bakış

Azure Otomasyonu durumu yapılandırmadır yazmak, yönetmek ve PowerShell Desired State Configuration (DSC) derleme olanak tanıyan bir Azure hizmeti [yapılandırmaları](/powershell/dsc/configurations), içeri aktarma [DSC kaynakları](/powershell/dsc/resources), ve Hedef düğümler, tüm bulut yapılandırmaları atayın.

## <a name="why-use-azure-automation-state-configuration"></a>Neden Azure Otomasyon durum yapılandırması kullanın

Azure Otomasyonu durum yapılandırması üzerinden Azure dışındaki DSC kullanarak çeşitli avantajlar sağlar.

### <a name="built-in-pull-server"></a>Yerleşik çekme sunucusu

Azure Otomasyonu durumu yapılandırma, benzer bir DSC çekme sunucusu sağlar [Windows özelliği DSC hizmet](/powershell/dsc/pullserver) hedef düğümler otomatik olarak yapılandırmaları alabilmesi adına, istenen duruma uyumlu hale gelir ve geri raporlama kendi Uyumluluk. Azure automation'da yerleşik çekme sunucusu ayarlama ve kendi çekme sunucunuz ihtiyacını ortadan kaldırır. Azure Otomasyonu, sanal veya fiziksel Windows veya Linux makineleri, bulutta veya şirket içi hedefleyebilirsiniz.

### <a name="management-of-all-your-dsc-artifacts"></a>Tüm DSC yapıların Yönetimi

Azure Otomasyonu durum Yapılandırması Yönetim katmanının aynısını getirir [PowerShell Desired State Configuration](/powershell/dsc/overview) Azure Otomasyonu, PowerShell betikleri için sunduğu.

Azure portaldan veya powershell'den, tüm, DSC yapılandırmaları, kaynak ve hedef düğümler olarak yönetebilirsiniz.

![Azure Otomasyonu dikey penceresinin ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Log Analytics'e raporlama verilerini içeri aktarma

Azure Otomasyonu durum yapılandırması ile yönetilen düğümler ayrıntılı raporlama Durum verilerini yerleşik çekme sunucusuna gönderir. Log Analytics çalışma alanınıza bu veri göndermek için Azure Otomasyon durum yapılandırması yapılandırabilirsiniz. Log Analytics çalışma alanınıza durum yapılandırması Durum verilerini gönderme hakkında bilgi edinmek için bkz: [İleri Azure Otomasyon durum için günlük nalytics veri raporlama Yapılandırması](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Tanıtım videosu

Okumak yerine izlemeyi mi tercih ediyorsunuz? Azure Otomasyonu durum yapılandırması ilk zaman Duyuruldu Mayıs 2015 aşağıdaki videoya göz vardır.

> [!NOTE]
> Bu videoda ele alınan yaşam döngüsü ve kavramları doğru olsa da, bu videonun kayda alındığı beri Azure Otomasyonu durumu yapılandırma çok ilerledikten. Genel kullanıma sunulmuştur, Azure portalında bir çok daha geniş bir kullanıcı Arabirimine sahiptir ve çok sayıda ek özellikleri destekler.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz: [Azure Otomasyon durum yapılandırması ile çalışmaya başlama](automation-dsc-getting-started.md)
- Bilgi edinmek için nasıl yerleşik düğümlerine bkz [makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)
- Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [yapılandırmaları Azure Automation durumu yapılandırma derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz: [Azure Otomasyon durum yapılandırması için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
- Bir sürekli dağıtım işlem hattı, Azure Otomasyonu durum yapılandırmasını kullanarak bir örnek görmek için bkz: [sürekli dağıtımı kullanarak Azure Otomasyon durum yapılandırması ve Chocolatey](automation-dsc-cd-chocolatey.md)