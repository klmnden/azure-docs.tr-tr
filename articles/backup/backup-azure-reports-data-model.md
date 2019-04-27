---
title: Azure Backup için veri modeli
description: Bu makalede, Azure Backup raporları için Power BI veri modeli ayrıntıları hakkında konuşuyor.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
origin.date: 06/26/2017
ms.date: 08/08/2018
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c6160570644da108ba713e8229b38f9587495c92
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337587"
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
| %ActiveAlertsCreatedInPeriod |Yüzde |Seçilen bir zaman dönemi içindeki etkin uyarılar yüzdesi |
| %CriticalAlertsCreatedInPeriod |Yüzde |Seçilen zaman aralığı içinde kritik uyarılar yüzdesi |
| AlertOccurrenceDate |Tarih |Uyarının oluşturulduğu tarih |
| AlertSeverity |Text |Örneğin, kritik uyarı önem derecesi |
| AlertStatus |Text |Örneğin, etkin uyarı durumu |
| AlertType |Text |Örneğin, yedekleme oluşturulan uyarı türü |
| AlertUniqueId |Text |Oluşturulan uyarı benzersiz kimliği |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Ondalık sayı |Seçilen zaman aralığı için uyarıyı çözümlemek için ortalama süre (dakika cinsinden) |
| EntityState |Text |Örneğin, etkin, silinmiş bir uyarı nesnenin geçerli durumu |

### <a name="backup-item"></a>Yedekleme öğesi
Bu tablo üzerinde çeşitli yedekleme öğesi ile ilgili alanları temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #BackupItems |Tam sayı |Yedekleme öğesi sayısı |
| #UnprotectedBackupItems |Tam sayı |Koruma için durduruldu veya yedekleri ancak başlatılmadı yedeklemeler için yapılandırılan yedekleme öğesi sayısı|
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| BackupItemFriendlyName |Text |Yedekleme öğesi kolay adı |
| BackupItemId |Text |Yedekleme öğesi kimliği |
| BackupItemName |Text |Yedekleme öğesinin adı |
| BackupItemType |Text |Yedekleme öğesi gibi VM Dosyaklasörü türü |
| EntityState |Text |Örneğin, etkin, silinen yedekleme öğesi nesnenin geçerli durumu |
| LastBackupDateTime |Tarih/Saat |Seçili yedekleme öğesi için son yedekleme zamanı |
| LastBackupState |Text |Örneğin, başarılı, başarısız seçili yedekleme öğesi için son yedekleme durumu |
| LastSuccessfulBackupDateTime |Tarih/Saat |Seçili yedekleme öğesi için son başarılı yedekleme saati |
| ProtectionState |Text |Örneğin, korumalı, ProtectionStopped yedekleme öğesi geçerli koruma durumu |

### <a name="calendar"></a>Takvim
Bu tabloda takvimle ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Tarih |Tarih |Verileri filtreleme için seçtiğiniz tarih |
| DateKey |Text |Her bir tarih öğesi için benzersiz anahtar |
| DayDiff |Ondalık sayı |Günlük veri filtreleme için örneğin fark, 0 geçerli günün verileri gösterir, -1, bir önceki günün verileri gösterir, 0 ile -1, geçerli ve önceki gün için veri belirtin  |
| Ay |Text |Aylık veri filtreleme için seçtiğiniz yılın, ayın ilk günü başlar ve 31 günü sona erer |
| MonthDate | Tarih |Tarihi sona erdiğinde ay, ayın veri filtreleme için seçtiğiniz |
| MonthDiff |Ondalık sayı |Örneğin ayın filtreleme veriler için fark, 0 geçerli aya ilişkin verileri gösterir, -1 önceki aya ait verileri gösterir, 0 ile -1 için geçerli ve önceki ayın verilerini belirtmek |
| Hafta |Text |Veri filtreleme için seçtiğiniz hafta hafta Pazar günü başlar ve biter Cumartesi günleri |
| WeekDate |Tarih |Tarih haftanın sona erdiğinde, hafta içinde veri filtreleme için seçtiğiniz |
| WeekDiff |Ondalık sayı |Örneğin haftada filtreleme veriler için fark, 0 geçerli haftanın verilerini gösterir, -1 önceki haftanın verilerini gösterir, 0 ile -1 için geçerli ve önceki haftanın verilerini belirtmek |
| Yıl |Text |Verileri filtreleme için seçtiğiniz takvim yılı |
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
| BackupStorageDestination |Text |Yedekleme depolama alanı gibi bulut Disk hedef  |
| EntityState |Text |Örneğin, etkin, silinen iş nesnenin geçerli durumu |
| JobFailureCode |Text |Hata kodu dizesi nedeniyle iş başarısız oldu |
| JobOperation |Text |İşlem için iş yedekleme, geri yükleme, yapılandırma yedekleme gibi çalıştırılır |
| JobStartDate |Tarih |Tarih çalışan iş başlatıldı |
| JobStartTime |Zaman |Zaman çalıştıran iş başlatıldı |
| JobStatus |Text |Örneğin, tamamlandı, başarısız bir tamamlanmış işinin durumu |
| JobUniqueId |Text |İşi belirlemek için benzersiz kimliği |

### <a name="policy"></a>İlke
Bu tablo, ilke ile ilgili çeşitli alanlarını temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Policies |Tam sayı |Sistemde mevcut yedekleme ilkelerini sayısı |
| #PoliciesInUse |Tam sayı |Şu anda yedeklemeleri yapılandırmak için kullanılan ilkeleri sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| BackupDaysOfTheWeek |Text |Ne zaman yedeklemeler zamanlandı haftanın günleri |
| BackupFrequency |Text |Sıklık ile yedeklemeleri çalıştırma Örneğin, günlük, haftalık |
| BackupTimes |Text |Yedeklemeler, zamanlanan tarih ve saat |
| DailyRetentionDuration |Tam sayı |Toplam elde tutma süresi yapılandırılan yedekleme için gün |
| DailyRetentionTimes |Text |Tarih ve saat günlük bekletme zaman yapılandırıldı |
| EntityState |Text |Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| MonthlyRetentionDaysOfTheMonth |Text |Aylık bekletme için seçili ayın tarihleri |
| MonthlyRetentionDaysOfTheWeek |Text |Aylık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionDuration |Ondalık sayı |Toplam elde tutma süresi yapılandırılan yedeklemeler için bir ay içinde |
| MonthlyRetentionFormat |Text |Aylık bekletme için yapılandırma günlük tabanlı, haftalık için hafta tabanlı gün için örneğin |
| MonthlyRetentionTimes |Text |Tarih ve saat aylık bekletme zaman yapılandırılır |
| MonthlyRetentionWeeksOfTheMonth |Text |Aylık bekletme olduğunda bir ayın hafta, örneğin, ilk, son VS yapılandırılmış. |
| PolicyName |Text |Tanımlanan ilke adı |
| PolicyUniqueId |Text |İlke tanımlamak için benzersiz kimlik |
| RetentionType |Text |Örneğin, günlük, haftalık, aylık, yıllık bekletme ilkesi, yazın |
| WeeklyRetentionDaysOfTheWeek |Text |Haftalık bekletme için haftanın günü seçilmedi |
| WeeklyRetentionDuration |Ondalık sayı |Yapılandırılmış yedeklemeler için hafta cinsinden toplam haftalık tutma süresi |
| WeeklyRetentionTimes |Text |Tarih ve saat haftalık bekletme zaman yapılandırılır |
| YearlyRetentionDaysOfTheMonth |Text |Yıllık bekletme için seçili ayın tarihleri |
| YearlyRetentionDaysOfTheWeek |Text |Yıllık bekletme için haftanın günü seçilmedi |
| YearlyRetentionDuration |Ondalık sayı |Toplam elde tutma süresi yapılandırılan yedeklemeler için yıl içinde |
| YearlyRetentionFormat |Text |Yıllık bekletme için yapılandırma günlük tabanlı, haftalık için hafta tabanlı gün için örneğin |
| YearlyRetentionMonthsOfTheYear |Text |Yıl ay yıllık bekletme için seçili |
| YearlyRetentionTimes |Text |Tarih ve saat, yıllık bekletme yapılandırılır |
| YearlyRetentionWeeksOfTheMonth |Text |Yıllık bekletme olduğunda bir ayın hafta, örneğin, ilk, son VS yapılandırılmış. |

### <a name="protected-server"></a>Korumalı sunucu
Bu tablo üzerinde çeşitli korumalı sunucu ilgili alanları temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedServers |Tam sayı |Korumalı sunucu sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| AzureBackupAgentOSType |Text |Azure yedekleme Aracısı'nın işletim sistemi türü |
| AzureBackupAgentOSVersion |Text |Azure yedekleme Aracısı'nın işletim sistemi sürümü |
| AzureBackupAgentUpdateDate |Text |Aracısı Yedekleme aracısı ne zaman güncelleştirildiği tarih |
| AzureBackupAgentVersion |Text |Aracı yedekleme sürümünün sürüm numarası |
| BackupManagementType |Text |Yedekleme gibi IaaSVM Dosyaklasörü gerçekleştirmek için sağlayıcı türü |
| EntityState |Text |Örneğin, etkin, silinen korumalı sunucu nesnenin geçerli durumu |
| ProtectedServerFriendlyName |Text |Korumalı sunucu kolay adı |
| ProtectedServerName |Text |Korumalı sunucu adı |
| ProtectedServerType |Text |Örneğin, IaaSVMContainer korumalı sunucu türünü desteklenen |
| ProtectedServerName |Text |Adı, hangi yedekleme öğesi için bir korumalı sunucunun ait olduğu |
| RegisteredContainerId |Text |Yedekleme için kayıtlı kapsayıcı kimliği |

### <a name="storage"></a>Depolama
Bu tablo depolama ile ilgili çeşitli alanlarını temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedInstances |Ondalık sayı |Seçilen sürede ön uç depolama faturalandırma, hesaplanmış dayalı olarak en son değeri hesaplamak için kullanılan korunan örnek sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| CloudStorageInMB |Ondalık sayı |Hesaplanan yedeklemeler tarafından kullanılan yedekleme depolama bulut en son değeri seçili zaman dayanır. |
| EntityState |Text |Örneğin, etkin, silinen nesnenin geçerli durumu |
| LastUpdatedDate |Tarih |Seçili satır son güncelleştirildiği tarih |

### <a name="time"></a>Zaman
Bu tabloda zamanla ilişkili alanları hakkındaki ayrıntılar verilmektedir.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Saat |Zaman |Örneğin, 1:00:00 PM günün saati |
| HourNumber |Ondalık sayı |Örneğin, 13,00 günün saat sayı |
| Dakika |Ondalık sayı |Saatin dakikasını |
| PeriodOfTheDay |Text |Örneğin, 12-3'te günlük dönem yuvasında zaman |
| Zaman |Zaman |Örneğin, 12:00:01: 00 ve günün saati |
| TimeKey |Text |Saati temsil eden anahtar değer |

### <a name="vault"></a>Kasa
Bu tablo, kasa ile ilgili çeşitli alanlarını temel alan ve toplamalar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Vaults |Tam sayı |Kasaları sayısı |
| AsOnDateTime |Tarih/Saat |Seçili satır için son yenileme zamanı |
| AzureDataCenter |Text |Kasanın bulunduğu veri merkezi |
| EntityState |Text |Örneğin, etkin, silinen kasa nesnenin geçerli durumu |
| StorageReplicationType |Text |Örneğin, GeoRedundant kasa için depolama çoğaltma türü |
| SubscriptionId |Text |Raporlar oluşturmak için seçili müşteri abonelik kimliği |
| VaultName |Text |Kasa adı |
| VaultTags |Text |Kasaya ilişkili etiketleri |

## <a name="next-steps"></a>Sonraki adımlar
Azure Backup raporları oluşturmak için veri modeli gözden geçirin, sonra oluşturma ve Power BI'da raporları görüntüleme hakkında daha fazla ayrıntı için aşağıdaki makalelere bakın.

- [Power BI'da raporlar oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
- [Power BI raporlarını filtreleme](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)


<!-- Update_Description: update metedata properties -->