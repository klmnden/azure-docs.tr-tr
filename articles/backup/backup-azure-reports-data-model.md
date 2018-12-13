---
title: Azure Backup için veri modeli
description: Bu makalede, Azure Backup raporları için Power BI veri modeli ayrıntıları hakkında konuşuyor.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 573b7e9c5c44c7162b4020f1ef54b8986003c0b5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877142"
---
# <a name="data-model-for-azure-backup-reports"></a>Azure Backup raporları için veri modeli
Bu makalede, Azure Backup raporları oluşturmak için kullanılan Power BI veri modeli açıklanmaktadır. Bu veri modelini kullanarak mevcut raporları ilgili alanlara göre filtreleyebilirsiniz ve daha fazla tabloları ve alanları modeli kullanarak da önemlisi, kendi raporlarınızı oluşturun. 

## <a name="creating-new-reports-in-power-bi"></a>Power BI'da yeni raporlar oluşturma
Power BI sağlar özelleştirme özelliklerini kullanarak yapabileceğiniz [veri modeli kullanarak raporlar oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Azure Backup veri modelini kullanma
Rapor oluşturabilir ve mevcut raporları özelleştirme ve veri modelinin bir parçası olarak sunulan aşağıdaki alanları kullanabilirsiniz.

### <a name="alert"></a>Uyarı
Bu tabloda, çeşitli uyarı ilgili alanları üzerinde temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Tam sayı |Seçili zaman aralığında oluşturulan uyarıların sayısı |
| % ActiveAlertsCreatedInPeriod |Yüzde |Seçilen bir zaman dönemi içindeki etkin uyarılar yüzdesi |
| % CriticalAlertsCreatedInPeriod |Yüzde |Seçilen zaman aralığı içinde kritik uyarılar yüzdesi |
| AlertOccurrenceDate |Tarih |Uyarının oluşturulduğu tarih |
| AlertSeverity |Metin |Örneğin, kritik uyarı önem derecesi |
| AlertStatus |Metin |Örneğin, etkin uyarı durumu |
| AlertType |Metin |Örneğin, yedekleme oluşturulan uyarı türü |
| AlertUniqueId |Metin |Oluşturulan uyarı benzersiz kimliği |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Ondalık sayı |Seçilen zaman aralığı için uyarıyı çözümlemek için ortalama süre (dakika cinsinden) |
| EntityState |Metin |Örneğin, etkin, silinmiş bir uyarı nesnenin geçerli durumu |

### <a name="backup-item"></a>Yedekleme öğesi
Bu tablo üzerinde çeşitli yedekleme öğesi ile ilgili alanları temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #BackupItems |Tam sayı |Yedekleme öğesi sayısı |
| #UnprotectedBackupItems |Tam sayı |Koruma için durduruldu veya yedekleri ancak başlatılmadı yedeklemeler için yapılandırılan yedekleme öğesi sayısı|
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| BackupItemFriendlyName |Metin |Yedekleme öğesi kolay adı |
| BackupItemId |Metin |Yedekleme öğesi kimliği |
| BackupItemName |Metin |Yedekleme öğesinin adı |
| BackupItemType |Metin |Yedekleme öğesi gibi VM Dosyaklasörü türü |
| EntityState |Metin |Örneğin, etkin, silinen yedekleme öğesi nesnenin geçerli durumu |
| LastBackupDateTime |Tarih/Saat |Seçili yedekleme öğesi için son yedekleme zamanı |
| LastBackupState |Metin |Örneğin, başarılı, başarısız seçili yedekleme öğesi için son yedekleme durumu |
| LastSuccessfulBackupDateTime |Tarih/Saat |Seçili yedekleme öğesi için son başarılı yedekleme saati |
| ProtectionState |Metin |Örneğin, korumalı, ProtectionStopped yedekleme öğesi geçerli koruma durumu |

### <a name="calendar"></a>Takvim
Bu tabloda takvimle ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Tarih |Tarih |Verileri filtreleme için seçtiğiniz tarih |
| DateKey |Metin |Her bir tarih öğesi için benzersiz anahtar |
| DayDiff |Ondalık sayı |Günlük veri filtreleme için örneğin fark, 0 geçerli günün verileri gösterir, -1, bir önceki günün verileri gösterir, 0 ile -1, geçerli ve önceki gün için veri belirtin  |
| Ay |Metin |Aylık veri filtreleme için seçtiğiniz yılın, ayın ilk günü başlar ve 31 günü sona erer |
| MonthDate | Tarih |Tarihi sona erdiğinde ay, ayın veri filtreleme için seçtiğiniz |
| MonthDiff |Ondalık sayı |Örneğin ayın filtreleme veriler için fark, 0 geçerli aya ilişkin verileri gösterir, -1 önceki aya ait verileri gösterir, 0 ile -1 için geçerli ve önceki ayın verilerini belirtmek |
| Hafta |Metin |Veri filtreleme için seçtiğiniz hafta hafta Pazar günü başlar ve biter Cumartesi günleri |
| WeekDate |Tarih |Tarih haftanın sona erdiğinde, hafta içinde veri filtreleme için seçtiğiniz |
| WeekDiff |Ondalık sayı |Örneğin haftada filtreleme veriler için fark, 0 geçerli haftanın verilerini gösterir, -1 önceki haftanın verilerini gösterir, 0 ile -1 için geçerli ve önceki haftanın verilerini belirtmek |
| Yıl |Metin |Verileri filtreleme için seçtiğiniz takvim yılı |
| YearDate |Tarih |Tarihi sona erdiğinde yıl, yılın veri filtreleme için seçtiğiniz |

### <a name="job"></a>İş
Bu tablo, iş ile ilgili çeşitli alanlarını temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #JobsCreatedInPeriod |Tam sayı |Seçili zaman aralığında oluşturulan iş sayısı |
| % FailuresForJobsCreatedInPeriod |Yüzde |Yüzde seçili zaman aralığında iş hataları genel |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Ondalık sayı |aktarılan veri 80. yüzdelik dilim değeri için MB cinsinden **yedekleme** seçili zaman aralığında oluşturulan işler |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Ondalık sayı |Ortalama süresi için dakika cinsinden **tamamlanmış yedekleme** seçili zaman aralığında oluşturulan işler |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Ondalık sayı |Ortalama süresi için dakika cinsinden **geri yükleme tamamlandı** seçili zaman aralığında oluşturulan işler |
| BackupStorageDestination |Metin |Yedekleme depolama alanı gibi bulut Disk hedef  |
| EntityState |Metin |Örneğin, etkin, silinen iş nesnenin geçerli durumu |
| JobFailureCode |Metin |Hata kodu dizesi nedeniyle iş başarısız oldu |
| JobOperation |Metin |İşlem için iş yedekleme, geri yükleme, yapılandırma yedekleme gibi çalıştırılır |
| JobStartDate |Tarih |Tarih çalışan iş başlatıldı |
| JobStartTime |Zaman |Zaman çalıştıran iş başlatıldı |
| JobStatus |Metin |Örneğin, tamamlandı, başarısız bir tamamlanmış işinin durumu |
| JobUniqueId |Metin |İşi belirlemek için benzersiz kimliği |

### <a name="policy"></a>İlke
Bu tablo, ilke ile ilgili çeşitli alanlarını temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Policies |Tam sayı |Sistemde mevcut yedekleme ilkelerini sayısı |
| #PoliciesInUse |Tam sayı |Şu anda yedeklemeleri yapılandırmak için kullanılan ilkeleri sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| BackupDaysOfTheWeek |Metin |Ne zaman yedeklemeler zamanlandı haftanın günleri |
| BackupFrequency |Metin |Sıklık ile yedeklemeleri çalıştırma Örneğin, günlük, haftalık |
| BackupTimes |Metin |Yedeklemeler, zamanlanan tarih ve saat |
| DailyRetentionDuration |Tam sayı |Toplam elde tutma süresi yapılandırılan yedekleme için gün |
| DailyRetentionTimes |Metin |Tarih ve saat günlük bekletme zaman yapılandırıldı |
| EntityState |Metin |Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| MonthlyRetentionDaysOfTheMonth |Metin |Aylık bekletme için seçili ayın tarihleri |
| MonthlyRetentionDaysOfTheWeek |Metin |Aylık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionDuration |Ondalık sayı |Toplam elde tutma süresi yapılandırılan yedeklemeler için bir ay içinde |
| MonthlyRetentionFormat |Metin |Aylık bekletme için yapılandırma günlük tabanlı, haftalık için hafta tabanlı gün için örneğin |
| MonthlyRetentionTimes |Metin |Tarih ve saat aylık bekletme zaman yapılandırılır |
| MonthlyRetentionWeeksOfTheMonth |Metin |Aylık bekletme olduğunda bir ayın hafta, örneğin, ilk, son VS yapılandırılmış. |
| PolicyName |Metin |Tanımlanan ilke adı |
| PolicyUniqueId |Metin |İlke tanımlamak için benzersiz kimlik |
| RetentionType |Metin |Örneğin, günlük, haftalık, aylık, yıllık bekletme ilkesi, yazın |
| WeeklyRetentionDaysOfTheWeek |Metin |Haftalık bekletme için haftanın günü seçilmedi |
| WeeklyRetentionDuration |Ondalık sayı |Yapılandırılmış yedeklemeler için hafta cinsinden toplam haftalık tutma süresi |
| WeeklyRetentionTimes |Metin |Tarih ve saat haftalık bekletme zaman yapılandırılır |
| YearlyRetentionDaysOfTheMonth |Metin |Yıllık bekletme için seçili ayın tarihleri |
| YearlyRetentionDaysOfTheWeek |Metin |Yıllık bekletme için haftanın günü seçilmedi |
| YearlyRetentionDuration |Ondalık sayı |Toplam elde tutma süresi yapılandırılan yedeklemeler için yıl içinde |
| YearlyRetentionFormat |Metin |Yıllık bekletme için yapılandırma günlük tabanlı, haftalık için hafta tabanlı gün için örneğin |
| YearlyRetentionMonthsOfTheYear |Metin |Yıl ay yıllık bekletme için seçili |
| YearlyRetentionTimes |Metin |Tarih ve saat, yıllık bekletme yapılandırılır |
| YearlyRetentionWeeksOfTheMonth |Metin |Yıllık bekletme olduğunda bir ayın hafta, örneğin, ilk, son VS yapılandırılmış. |

### <a name="protected-server"></a>Korumalı sunucu
Bu tablo üzerinde çeşitli korumalı sunucu ilgili alanları temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedServers |Tam sayı |Korumalı sunucu sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| AzureBackupAgentOSType |Metin |Azure yedekleme Aracısı'nın işletim sistemi türü |
| AzureBackupAgentOSVersion |Metin |Azure yedekleme Aracısı'nın işletim sistemi sürümü |
| AzureBackupAgentUpdateDate |Metin |Aracısı Yedekleme aracısı ne zaman güncelleştirildiği tarih |
| AzureBackupAgentVersion |Metin |Aracı yedekleme sürümünün sürüm numarası |
| BackupManagementType |Metin |Yedekleme gibi IaaSVM Dosyaklasörü gerçekleştirmek için sağlayıcı türü |
| EntityState |Metin |Örneğin, etkin, silinen korumalı sunucu nesnenin geçerli durumu |
| ProtectedServerFriendlyName |Metin |Korumalı sunucu kolay adı |
| ProtectedServerName |Metin |Korumalı sunucu adı |
| ProtectedServerType |Metin |Örneğin, IaaSVMContainer korumalı sunucu türünü desteklenen |
| ProtectedServerName |Metin |Adı, hangi yedekleme öğesi için bir korumalı sunucunun ait olduğu |
| RegisteredContainerId |Metin |Yedekleme için kayıtlı kapsayıcı kimliği |

### <a name="storage"></a>Depolama
Bu tablo depolama ile ilgili çeşitli alanlarını temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedInstances |Ondalık sayı |Seçilen sürede ön uç depolama faturalandırma, hesaplanmış dayalı olarak en son değeri hesaplamak için kullanılan korunan örnek sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| CloudStorageInMB |Ondalık sayı |Hesaplanan yedeklemeler tarafından kullanılan yedekleme depolama bulut en son değeri seçili zaman dayanır. |
| EntityState |Metin |Örneğin, etkin, silinen nesnenin geçerli durumu |
| LastUpdatedDate |Tarih |Seçili satır son güncelleştirildiği tarih |

### <a name="time"></a>Zaman
Bu tabloda zamanla ilişkili alanları hakkındaki ayrıntılar verilmektedir.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Saat |Zaman |Örneğin, 1:00:00 PM günün saati |
| HourNumber |Ondalık sayı |Örneğin, 13,00 günün saat sayı |
| Dakika |Ondalık sayı |Saatin dakikasını |
| PeriodOfTheDay |Metin |Örneğin, 12-3'te günlük dönem yuvasında zaman |
| Zaman |Zaman |Örneğin, 12:00:01: 00 ve günün saati |
| TimeKey |Metin |Saati temsil eden anahtar değer |

### <a name="vault"></a>Kasa
Bu tablo, kasa ile ilgili çeşitli alanlarını temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Vaults |Tam sayı |Kasaları sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| AzureDataCenter |Metin |Kasanın bulunduğu veri merkezi |
| EntityState |Metin |Örneğin, etkin, silinen kasa nesnenin geçerli durumu |
| StorageReplicationType |Metin |Örneğin, GeoRedundant kasa için depolama çoğaltma türü |
| SubscriptionId |Metin |Raporlar oluşturmak için seçili müşteri abonelik kimliği |
| VaultName |Metin |Kasa adı |
| VaultTags |Metin |Kasaya ilişkili etiketleri |

## <a name="next-steps"></a>Sonraki adımlar
Azure Backup raporları oluşturmak için veri modeli gözden geçirin, sonra oluşturma ve Power BI'da raporları görüntüleme hakkında daha fazla ayrıntı için aşağıdaki makalelere bakın.

* [Power BI'da raporlar oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Power BI raporlarını filtreleme](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
