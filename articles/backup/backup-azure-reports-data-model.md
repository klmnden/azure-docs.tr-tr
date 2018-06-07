---
title: Azure yedekleme için veri modeli
description: Bu makalede, Azure Backup raporları için Power BI veri modeli ayrıntılarına hakkında alınmaktadır.
services: backup
author: JPallavi
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a17e011452f9b87c1201cea12f394a9cdd18e54b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606231"
---
# <a name="data-model-for-azure-backup-reports"></a>Azure Backup raporları için veri modeli
Bu makalede, Azure Backup raporlar oluşturmak için kullanılan Power BI veri modeli açıklanır. Bu veri modelini kullanarak, varolan raporlarla ilgili alanlara göre filtre uygulayabilirsiniz ve daha fazla tabloları ve alanları modeli kullanarak da önemlisi, kendi raporlarınızı oluşturun. 

## <a name="creating-new-reports-in-power-bi"></a>Power BI'da yeni raporları oluşturma
Power BI yapabilecekleriniz kullandığı özelleştirme özellikleri sağlar [veri modelini kullanarak raporları oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Azure Backup veri modelini kullanma
Raporları oluşturma ve mevcut raporları özelleştirme ve veri modelinin bir parçası olarak sunulan aşağıdaki alanları kullanın.

### <a name="alert"></a>Uyarı
Bu tablo üzerinde çeşitli uyarı ilgili alanları temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Tam sayı |Seçilen bir zaman diliminde oluşturulan uyarıların sayısı |
| % ActiveAlertsCreatedInPeriod |Yüzde Oranı |Seçilen zaman aralığı içindeki etkin uyarılar yüzdesi |
| % CriticalAlertsCreatedInPeriod |Yüzde Oranı |Seçilen zaman aralığı içinde kritik uyarılar yüzdesi |
| AlertOccurenceDate |Tarih |Uyarının oluşturulduğu tarih |
| AlertSeverity |Metin |Örneğin, kritik uyarı önem derecesi |
| AlertStatus |Metin |Örneğin, etkin uyarı durumu |
| AlertType |Metin |Örneğin, yedekleme oluşturulan uyarı türü |
| AlertUniqueId |Metin |Oluşturulan uyarının benzersiz kimliği |
| AsOnDateTime |Tarih/Saat |Seçili olan satır için son yenileme süresi |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Ondalık sayı |Seçilen zaman aralığı için uyarıyı çözümlemek için ortalama süre (dakika cinsinden) |
| EntityState |Metin |Örneğin, etkin, silinen uyarı nesnenin geçerli durumu |

### <a name="backup-item"></a>Yedekleme öğesi
Bu tablo üzerinde çeşitli yedekleme öğesi ilişkili alanlar temel alanlar ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #BackupItems |Tam sayı |Yedekleme öğelerin sayısı |
| #UnprotectedBackupItems |Tam sayı |Koruma için durduruldu veya yedeklemeleri ancak başlatılmamış yedeklemeler için yapılandırılan yedekleme öğelerinin sayısı|
| AsOnDateTime |Tarih/Saat |Seçili olan satır için son yenileme süresi |
| BackupItemFriendlyName |Metin |Yedekleme öğesinin kolay adı |
| BackupItemId |Metin |Yedekleme öğesinin kimliği |
| BackupItemName |Metin |Yedekleme öğesinin adı |
| BackupItemType |Metin |Yedekleme öğesi Örneğin, VM, FileFolder türü |
| EntityState |Metin |Örneğin, etkin, silinen yedekleme öğesi nesnenin geçerli durumu |
| LastBackupDateTime |Tarih/Saat |Yedekleme seçili öğe için son yedekleme zamanı |
| LastBackupState |Metin |Örneğin, başarılı, başarısız yedekleme seçili öğe için son yedekleme durumu |
| LastSuccessfulBackupDateTime |Tarih/Saat |Seçili yedekleme öğesi için son başarılı yedekleme saati |
| ProtectionState |Metin |Örneğin, korumalı, ProtectionStopped yedekleme öğesinin geçerli koruma durumu |

### <a name="calendar"></a>Takvim
Bu tablo takvim ilişkili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Tarih |Tarih |Verileri filtreleme için seçilen tarihi |
| DateKey |Metin |Her bir tarih öğesi için benzersiz anahtar |
| DayDiff |Ondalık sayı |Filtreleme veriler için gün içinde örneğin fark, 0, geçerli günün verileri gösterir, bir önceki günün veri -1 gösterir, 0 ile -1 geçerli ve önceki gün için veri gösterir  |
| Ay |Metin |Ay verileri filtreleme için seçilen yılın ay ilk günü başlayan ve biten 31 gün |
| MonthDate | Tarih |Tarih ay sona erdiğinde, ay verileri filtreleme için seçili |
| MonthDiff |Ondalık sayı |Filtreleme veriler için aydaki örneğin fark, 0, geçerli ayın verilerini gösterir, önceki ayın veri -1 gösterir, 0 ile -1, geçerli ve önceki ay için verileri belirtmek |
| Hafta |Metin |Verileri filtreleme için seçilen hafta hafta Pazar günü başlar ve biter Cumartesi günleri |
| WeekDate |Tarih |Hafta sona erdiğinde, haftanın tarihi verileri filtreleme için seçili |
| WeekDiff |Ondalık sayı |Hafta filtreleme veriler için örneğin fark, 0, geçerli haftanın verilerini gösterir, önceki haftanın veri -1 gösterir, 0 ile -1 için geçerli ve önceki hafta verileri belirtmek |
| Yıl |Metin |Verileri filtreleme için seçili takvim yılı |
| YearDate |Tarih |Tarih yıl sona erdiğinde, yıl verileri filtreleme için seçili |

### <a name="job"></a>İş
Bu tablo üzerinde çeşitli iş ile ilgili alanları temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #JobsCreatedInPeriod |Tam sayı |Seçilen zaman diliminde oluşturulmuş işlerin sayısı |
| % FailuresForJobsCreatedInPeriod |Yüzde Oranı |Yüzdesi seçilen zaman aralığı içinde iş hataları genel |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Ondalık sayı |Veri 80. yüzde birlik değerini aktarılan MB için **yedekleme** seçilen zaman diliminde oluşturulan işler |
| AsOnDateTime |Tarih/Saat |Seçili olan satır için son yenileme süresi |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Ondalık sayı |Ortalama zaman dakika cinsinden **tamamlanmış yedeği** seçilen bir zaman diliminde oluşturulan işler |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Ondalık sayı |Ortalama zaman dakika cinsinden **geri yükleme tamamlandı** seçilen bir zaman diliminde oluşturulan işler |
| BackupStorageDestination |Metin |Yedekleme depolama Örneğin, bulut, Disk hedefi  |
| EntityState |Metin |Örneğin, etkin, silinen iş nesnenin geçerli durumu |
| JobFailureCode |Metin |Hata kodu dize hangi nedeniyle iş başarısız oldu |
| JobOperation |Metin |İçin proje Örneğin, yedekleme, geri yükleme, yapılandırma yedekleme çalıştığı işlemi |
| JobStartDate |Tarih |İş çalışmaya başladığı tarih |
| JobStartTime |Zaman |Çalışan işin başladığı zaman zaman |
| JobStatus |Metin |Örneğin, tamamlandı, başarısız tamamlanmış işinin durumu |
| JobUniqueId |Metin |İşi belirlemek için benzersiz kimliği |

### <a name="policy"></a>İlke
Bu tablo üzerinde çeşitli ilkesiyle ilgili alanları temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Policies |Tam sayı |Sistemde mevcut yedekleme ilkeleri sayısı |
| #PoliciesInUse |Tam sayı |Şu anda yedeklemeleri yapılandırma için kullanılan ilkeleri sayısı |
| AsOnDateTime |Tarih/Saat |Seçili olan satır için son yenileme süresi |
| BackupDaysOfTheWeek |Metin |Yedeklemelerin ne zaman zamanlandı haftanın günleri |
| BackupFrequency |Metin |Yedeklemeler, çalıştığı Örneğin, günlük, haftalık sıklığı |
| BackupTimes |Metin |Yedeklemelerin ne zaman zamanlanmış tarih ve saat |
| DailyRetentionDuration |Tam sayı |Yapılandırılan yedekleme için gün cinsinden toplam tutma süresi |
| DailyRetentionTimes |Metin |Tarih ve saat günlük bekletme zaman yapılandırıldı |
| EntityState |Metin |Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| MonthlyRetentionDaysOfTheMonth |Metin |Aylık bekletme için seçili ayın tarihleri |
| MonthlyRetentionDaysOfTheWeek |Metin |Aylık bekletme için haftanın günlerini seçili |
| MonthlyRetentionDuration |Ondalık sayı |Yapılandırılmış yedeklemeler için ay cinsinden toplam tutma süresi |
| MonthlyRetentionFormat |Metin |Aylık bekletme yapılandırmasının Örneğin, günlük tabanlı, haftalık dayalı haftanın günü için yazın |
| MonthlyRetentionTimes |Metin |Tarih ve saat aylık bekletme zaman yapılandırılır |
| MonthlyRetentionWeeksOfTheMonth |Metin |Aylık bekletme olduğunda ayın haftasını, örneğin, ilk, son vb. yapılandırılmış. |
| PolicyName |Metin |Tanımlanan ilke adı |
| PolicyUniqueId |Metin |İlke tanımlamak için benzersiz kimlik |
| RetentionType |Metin |Örneğin, günlük, haftalık, aylık, yıllık bekletme ilkesi, yazın |
| WeeklyRetentionDaysOfTheWeek |Metin |Haftalık bekletme için haftanın günlerini seçili |
| WeeklyRetentionDuration |Ondalık sayı |Yapılandırılmış yedeklemeler için hafta cinsinden toplam haftalık tutma süresi |
| WeeklyRetentionTimes |Metin |Tarih ve saat haftalık bekletme zaman yapılandırılır |
| YearlyRetentionDaysOfTheMonth |Metin |Yıllık bekletme için seçili ayın tarihleri |
| YearlyRetentionDaysOfTheWeek |Metin |Yıllık bekletme için haftanın günlerini seçili |
| YearlyRetentionDuration |Ondalık sayı |Yapılandırılmış yedeklemeleri yıldır cinsinden toplam tutma süresi |
| YearlyRetentionFormat |Metin |Yıllık bekletme yapılandırmasının Örneğin, günlük tabanlı, haftalık dayalı haftanın günü için yazın |
| YearlyRetentionMonthsOfTheYear |Metin |Yıllık bekletme için yılın ay seçili |
| YearlyRetentionTimes |Metin |Tarih ve saat yıllık bekletme zaman yapılandırılır |
| YearlyRetentionWeeksOfTheMonth |Metin |Yıllık bekletme olduğunda ayın haftasını, örneğin, ilk, son vb. yapılandırılmış. |

### <a name="protected-server"></a>Korumalı sunucu
Bu tablo üzerinde çeşitli korumalı sunucu ilgili alanlar temel alanlar ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedServers |Tam sayı |Korumalı sunucu sayısı |
| AsOnDateTime |Tarih/Saat |Seçili olan satır için son yenileme süresi |
| AzureBackupAgentOSType |Metin |Azure yedekleme Aracısı'nın işletim sistemi türü |
| AzureBackupAgentOSVersion |Metin |Azure yedekleme Aracısı'nın işletim sistemi sürümü |
| AzureBackupAgentUpdateDate |Metin |Aracı Backup Aracısı güncelleştirildiği tarihi |
| AzureBackupAgentVersion |Metin |Aracı yedekleme sürümünün sürüm numarası |
| BackupManagementType |Metin |Örneğin, IaaSVM FileFolder yedeklemenin için sağlayıcı türü |
| EntityState |Metin |Örneğin, etkin, silinen korumalı sunucu nesnenin geçerli durumu |
| ProtectedServerFriendlyName |Metin |Korumalı sunucusunun kolay adı |
| ProtectedServerName |Metin |Korumalı sunucunun adı |
| ProtectedServerType |Metin |Örneğin, IaaSVMContainer korumalı sunucu türünü yedeklenen |
| ProtectedServerName |Metin |Hangi yedekleme öğesi için korumalı sunucunun adını ait |
| RegisteredContainerId |Metin |Yedekleme için kayıtlı kapsayıcı kimliği |

### <a name="storage"></a>Depolama
Bu tablo üzerinde çeşitli depolama ilişkili alanlar temel alanlar ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedInstances |Ondalık sayı |Seçilen zaman içinde ön uç depolama faturalama, hesaplanan dayalı olarak en son değeri hesaplamak için kullanılan korumalı örneği sayısı |
| AsOnDateTime |Tarih/Saat |Seçili olan satır için son yenileme süresi |
| CloudStorageInMB |Ondalık sayı |Hesaplanan yedekleri tarafından kullanılan bulut yedekleme depolama alanı en son değeri seçili zaman dayanır. |
| EntityState |Metin |Örneğin, etkin, silinen nesnenin geçerli durumu |
| LastUpdatedDate |Tarih |Seçilen satırın en son güncelleştirildiği tarihi |

### <a name="time"></a>Zaman
Bu tablo saati ile ilgili alanları hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Saat |Zaman |Örneğin, 1:00:00 PM günün saati |
| HourNumber |Ondalık sayı |Örneğin, 13,00 gün saat sayısı |
| Dakika |Ondalık sayı |Saatin dakikasını |
| PeriodOfTheDay |Metin |Örneğin, 12-3 AM günlük dönem yuvada zaman |
| Zaman |Zaman |Örneğin, 00:00:01: 00 günün saati |
| TimeKey |Metin |Saati temsil eden anahtar değer |

### <a name="vault"></a>Kasa
Bu tablo üzerinde çeşitli kasası ilişkili alanlar temel alanlar ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Vaults |Tam sayı |Kasa sayısı |
| AsOnDateTime |Tarih/Saat |Seçili olan satır için son yenileme süresi |
| AzureDataCenter |Metin |Kasa bulunduğu veri merkezi |
| EntityState |Metin |Örneğin, etkin, silinen kasası nesnenin geçerli durumu |
| StorageReplicationType |Metin |Örneğin, GeoRedundant kasa için depolama çoğaltma türü |
| SubscriptionId |Metin |Rapor oluşturmak için seçilen müşterinin abonelik kimliği |
| VaultName |Metin |Kasa adı |
| VaultTags |Metin |Kasaya ilişkili etiketleri |

## <a name="next-steps"></a>Sonraki adımlar
Azure Backup raporları oluşturmak için veri modeli gözden sonra oluşturma ve Power BI raporları görüntüleme hakkında daha fazla ayrıntı için aşağıdaki makalelere bakın.

* [Power BI raporları oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Power BI raporları filtreleme](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
