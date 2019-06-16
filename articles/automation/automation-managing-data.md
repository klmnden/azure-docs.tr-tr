---
title: Azure Otomasyonu verilerini yönetme
description: Bu makale, bir Azure Otomasyonu ortamı yönetmek için birden çok konuları içerir.  Şu anda veri saklama ve Azure Otomasyonu olağanüstü durum kurtarma, Azure automation'da yedekleme içerir.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5f9cd5edfb360da507320306314e67ac61503132
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60738491"
---
# <a name="managing-azure-automation-data"></a>Azure Otomasyonu verilerini yönetme
Bu makale, bir Azure Otomasyonu ortamı yönetmek için birden çok konuları içerir.

## <a name="data-retention"></a>Veri saklama
Azure Otomasyonu'nda kaynak silme, kalıcı olarak kaldırılan önce 90 gün yapılacak denetim için korunur.  Göremez veya bu süre boyunca kaynağı kullanın.  Bu ilke, silinen bir Otomasyon hesabına ait kaynaklar için de geçerlidir.

Azure Otomasyonu, otomatik olarak siler ve 90 günden daha eski işleri kalıcı olarak kaldırır.

Aşağıdaki tabloda, farklı kaynaklar için bekletme ilkesi özetlenmektedir.

| Veriler | İlke |
|:--- |:--- |
| Hesaplar |Hesap bir kullanıcı tarafından silindiğinde ettikten 90 gün sonra kalıcı olarak silinir. |
| Varlıklar |90 gün sonra varlık, bir kullanıcı tarafından silinmiş veya 90 gün sonra varlık, bir kullanıcı tarafından silinmiş tutan hesabı kalıcı olarak silinir. |
| Modüller |Modül bir kullanıcı tarafından silindikten sonra 90 gün veya 90 gün sonra modülü, bir kullanıcı tarafından silinmiş tutan hesabı kalıcı olarak silinir. |
| Runbook'lar |90 gün sonra kaynak, bir kullanıcı tarafından silinmiş veya 90 gün sonra kaynak bir kullanıcı tarafından silindiğinde tutan hesabı kalıcı olarak silinir. |
| İşler |Silinen ve kaldırılan kalıcı olarak 90 gün sonra en son değiştirilen. Sonra iş tamamlandığında, durduruldu veya askıya alınmış olabilir. |
| Düğüm yapılandırmaları/MOF dosyaları |Eski düğüm yapılandırması yeni bir düğüm yapılandırması oluşturulan ettikten 90 gün sonra kalıcı olarak kaldırılır. |
| DSC düğümleri |Azure portalını kullanarak otomasyon hesabından kaydı düğümüdür ettikten 90 gün sonra'kalıcı olarak kaldırılan veya [Unregister-AzureRMAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/unregister-azurermautomationdscnode) Windows PowerShell cmdlet'i. Düğümleri 90 gün sonra bir kullanıcı tarafından düğümü silinir tutan hesabı da kalıcı olarak kaldırılır. |
| Düğüm raporları |Bu düğüm için oluşturulan yeni bir rapor ettikten 90 gün sonra kalıcı olarak kaldırılması |

Bekletme İlkesi, tüm kullanıcılar için geçerlidir ve şu anda özelleştirilemez.

Ancak, uzun bir süre saklamak istiyorsanız iş günlüklerini Azure İzleyici günlüklerine runbook iletebilir.  Daha fazla bilgi için gözden [iletmek Azure Otomasyonu iş verilerini Azure İzleyici günlüklerine](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Azure Otomasyonunu Yedekleme
Microsoft azure'da bir Otomasyon hesabı sildiğinizde, hesaptaki tüm nesnelere runbook'ları, modüller, yapılandırmaları, ayarları, işleri ve varlıkları dahil olmak üzere silinir. Nesneleri, hesap silindikten sonra kurtarılamaz.  Otomasyon hesabınızın içeriğini silmeden önce yedeklemek için aşağıdaki bilgileri kullanabilirsiniz. 

### <a name="runbooks"></a>Runbook'lar
Azure portalını kullanarak komut dosyaları için runbook'larınızı dışa aktarabilirsiniz veya [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) Windows PowerShell cmdlet'i.  Bu komut dosyaları başka bir Otomasyon hesabına bölümünde açıklandığı gibi içeri aktarılabilir [oluşturma veya bir Runbook'u içeri aktarma](/previous-versions/azure/dn643637(v=azure.100)).

### <a name="integration-modules"></a>Tümleştirme modülleri
Azure Otomasyonu tümleştirme modülleri dışarı aktaramazsınız.  Otomasyon hesabı dışında kullanılabilir emin olmanız gerekir.

### <a name="assets"></a>Varlıklar
Dışarı aktaramadığınız [varlıklar](/previous-versions/azure/dn939988(v=azure.100)) Azure Otomasyonu öğesinden.  Azure portalını kullanarak, değişkenleri, kimlik bilgileri, sertifikalar, bağlantılar ve zamanlamaları ayrıntılarını not almanız gerekir.  Başka bir automation'a içeri aktardığınız runbook'lar tarafından kullanılan tüm varlıkları el ile oluşturmanız gerekir.

Kullanabileceğiniz [Azure cmdlet'lerini](https://docs.microsoft.com/powershell/module/azurerm.automation#automation) ayrıntılarını şifrelenmemiş varlıkları ve kaydetmek ya da ileride kullanılmak üzere alın veya eşdeğer varlıklar başka bir Otomasyon hesabı oluşturun.

Şifrelenmiş değişkenler veya cmdlet'lerini kullanarak kimlik bilgilerinin parola alanı değeri alınamıyor.  Bu değerleri tanımadığınız sonra bir runbook kullanarak alabilirsiniz [Get-AutomationVariable](/previous-versions/azure/dn940012(v=azure.100)) ve [Get-AutomationPSCredential](/previous-versions/azure/dn940015(v=azure.100)) etkinlikler.

Azure Otomasyonu sertifika dışarı aktaramazsınız.  Sertifikalarını Azure dışında kullanılabilir olduğundan emin olmanız gerekir.

### <a name="dsc-configurations"></a>DSC yapılandırmaları
Azure portalını kullanarak komut dosyalarının yapılandırmanızı dışarı aktarabilirsiniz veya [dışarı aktarma AzureRmAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/azurerm.automation/export-azurermautomationdscconfiguration) Windows PowerShell cmdlet'i. Bu yapılandırmalar, aktarılır ve başka bir Otomasyon hesabı kullanılır.

## <a name="geo-replication-in-azure-automation"></a>Azure automation'da coğrafi çoğaltma
Coğrafi çoğaltma, standart Azure Automation hesaplarında yedeklilik için farklı bir coğrafi bölgeye hesap verileri yedekler. Hesabınızı ayarlarken bir birincil bölge seçin ve ardından ikincil bir bölgeye otomatik olarak atanır. Kopyalanan birincil bölgeden ikincil verileri, veri kaybı durumunda sürekli olarak güncelleştirilir.  

Aşağıdaki tabloda kullanılabilir birincil ve ikincil bölge eşleştirmeleri gösterilir.

| Birincil | İkincil |
| --- | --- |
| Orta Güney ABD |Orta Kuzey ABD |
| ABD Doğu 2 |Orta ABD |
| Batı Avrupa |Kuzey Avrupa |
| Güneydoğu Asya |Doğu Asya |
| Japonya Doğu |Japonya Batı |

Bir birincil bölge verileri kaybedilirse olası durumunda Microsoft, kurtarılır çalışır. Birincil verilerin kurtarılamaması durumunda coğrafi olarak yük devretme ardından gerçekleştirilir ve etkilenen müşteriler bu konuda aboneliğini bildirilir.


