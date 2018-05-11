---
title: Azure Otomasyonu verilerini yönetme
description: Bu makale bir Azure Otomasyonu ortamının yönetilmesi için birden çok konuları içerir.  Şu anda veri saklama ve Azure Automation olağanüstü durum kurtarma Azure Automation yedekleme içerir.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: b9245a0a81958f1044ad5be6f5448e086ba54636
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="managing-azure-automation-data"></a>Azure Otomasyonu verilerini yönetme
Bu makale bir Azure Otomasyonu ortamının yönetilmesi için birden çok konuları içerir.

## <a name="data-retention"></a>Veri saklama
Azure Otomasyonu'nda kaynak sildiğinizde, kalıcı olarak kaldırılmasını önce 90 gün denetim amacıyla korunur.  Bakın veya bu süre boyunca kaynağı kullanın.  Bu ilke, silinen bir automation hesabına ait kaynaklara geçerlidir.

Azure Otomasyonu otomatik olarak siler ve işleri 90 günden daha eski kalıcı olarak kaldırır.

Aşağıdaki tabloda farklı kaynaklar için bekletme ilkesi özetler.

| Veriler | İlke |
|:--- |:--- |
| Hesaplar |Hesap bir kullanıcı tarafından silindi ettikten 90 gün sonra kalıcı olarak silinir. |
| Varlıklar |Varlık bir kullanıcı tarafından silinmiş ettikten 90 gün veya 90 gün sonra varlık bir kullanıcı tarafından silinmiş tutan hesabı kalıcı olarak silinir. |
| Modüller |Modül bir kullanıcı tarafından silinmiş ettikten 90 gün veya 90 gün sonra modülü bir kullanıcı tarafından silinmiş tutan hesabı kalıcı olarak silinir. |
| Runbook'lar |Kaynak bir kullanıcı tarafından silinmiş ettikten 90 gün veya 90 gün sonra kaynak kullanıcı tarafından silindi tutan hesabı kalıcı olarak silinir. |
| İşler |Silinen ve kalıcı olarak kaldırılan 90 gün sonra değiştirilen son. İş tamamlandığında, durduruldu veya askıya alınmış sonra bu olabilir. |
| Düğüm yapılandırmaları/MOF dosyaları |Eski düğüm yapılandırması, yeni bir düğüm yapılandırması oluşturulan ettikten 90 gün sonra kalıcı olarak kaldırılır. |
| DSC düğümleri |Azure portal kullanarak Automation hesabını kaydı bir düğümdür ettikten 90 gün sonra'kalıcı olarak kaldırılır veya [Unregister-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) Windows PowerShell cmdlet'i. Düğümleri 90 gün sonra düğüm bir kullanıcı tarafından silinmiş tutan hesabı da kalıcı olarak kaldırılır. |
| Düğüm raporları |Bu düğüm için oluşturulan yeni bir rapor ettikten 90 gün sonra kalıcı olarak silinir |

Bekletme İlkesi, tüm kullanıcılar için geçerlidir ve şu anda özelleştirilemez.

Ancak, uzun bir süre için verileri korumak gerekiyorsa, runbook günlük analizi için iş günlüklerini gönderebilir.  Daha fazla bilgi için gözden [Azure Otomasyonu işi veri iletmek için günlük analizi](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Azure Otomasyonunu Yedekleme
Microsoft Azure automation hesabında sildiğinizde, hesaptaki tüm nesnelere runbook'lar, modüller, yapılandırmaları, ayarları, işleri ve varlıkları dahil olmak üzere silinir. Hesap silindikten sonra nesneleri kurtarılamıyor.  Otomasyon hesabınızın içeriğini silmeden önce yedeklemek için aşağıdaki bilgileri kullanın. 

### <a name="runbooks"></a>Runbook'lar
Azure portalını kullanarak komut dosyaları için runbook'larınızın dışa aktarabilirsiniz veya [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) Windows PowerShell cmdlet'i.  Bu komut dosyaları başka bir Otomasyon hesabı içinde anlatıldığı gibi alınabilir [oluşturma veya bir Runbook'u içeri aktarma](https://msdn.microsoft.com/library/dn643637.aspx).

### <a name="integration-modules"></a>Tümleştirme modülleri
Azure Otomasyon tümleştirme modülleri dışarı aktaramazsınız.  Otomasyon hesabı dışında kullanılabilir emin olmalısınız.

### <a name="assets"></a>Varlıklar
Dışarı aktarılamıyor [varlıklar](https://msdn.microsoft.com/library/dn939988.aspx) Azure Otomasyonu gelen.  Azure Portalı'nı kullanarak, değişkenleri, kimlik, sertifikalar, bağlantıları ve zamanlamaları ayrıntılarını not almanız gerekir.  Başka bir automation'a içeri aktardığınız runbook'lar tarafından kullanılan tüm varlıkları el ile oluşturmanız gerekir.

Kullanabileceğiniz [Azure cmdlet'lerini](https://msdn.microsoft.com/library/dn690262.aspx) şifrelenmemiş varlıklar ve bunları kaydetmek ya da ayrıntılarını ileride kullanılmak üzere alınamıyor veya başka bir Otomasyon hesabı eşdeğer varlıklar oluşturun.

Şifrelenmiş değişkenler ya da cmdlet'leri kullanarak kimlik bilgilerini parola alan için değer alınamıyor.  Bu değerleri tanımadığınız sonra bunları kullanarak bir runbook'tan almak [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) ve [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) etkinlikler.

Azure Otomasyon sertifikaları dışarı aktaramazsınız.  Herhangi bir sertifika Azure dışında kullanılabilir olduğundan emin olmalısınız.

### <a name="dsc-configurations"></a>DSC yapılandırmaları
Azure portalını kullanarak komut dosyalarının yapılandırmalarınızı dışa aktarabilirsiniz veya [verme AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) Windows PowerShell cmdlet'i. Bu yapılandırmalar aktarılır ve başka bir Otomasyon hesabı kullanılır.

## <a name="geo-replication-in-azure-automation"></a>Azure automation'da coğrafi çoğaltma
Coğrafi çoğaltma, standart Azure Automation hesaplarında artıklık için farklı bir coğrafi bölge için hesap verileri yedekler. Bir birincil bölge hesabınızı ayarlarken seçebilir ve ardından bir ikincil bölge için otomatik olarak atanır. Birincil bölgesinden kopyaladığınız ikincil veri veri kaybı durumunda sürekli olarak güncelleştirilir.  

Aşağıdaki tabloda kullanılabilir birincil ve ikincil bölge eşleştirmeleri gösterilir.

| Birincil | İkincil |
| --- | --- |
| Orta Güney ABD |Orta Kuzey ABD |
| ABD Doğu 2 |Orta ABD |
| Batı Avrupa |Kuzey Avrupa |
| Güneydoğu Asya |Doğu Asya |
| Japonya Doğu |Japonya Batı |

Bir birincil bölge veriler kaybolur olası olayda Microsoft Kurtarma dener. Birincil veri kurtarılamazsa, etkilenen müşteriler bu konuda aboneliğini bildirilir ve yük devretme coğrafi sonra gerçekleştirilir.

