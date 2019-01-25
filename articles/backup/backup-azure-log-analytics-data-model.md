---
title: Azure Backup için Log Analytics veri modeli
description: Bu makalede, verileri Azure Backup için Log Analytics veri modeli ayrıntıları hakkında konuşuyor.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 07/24/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09f7d4c5e76d4f74d447f8e8760e1f348462c769
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887028"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Verileri Azure Backup için log Analytics veri modeli
Log Analytics veri modeli, raporlar oluşturmak için kullanın. Veri modeli ile özel sorgular ve panolar oluşturmak veya istediğiniz ancak Azure Backup verileri, özelleştirebilir.

## <a name="using-azure-backup-data-model"></a>Azure Backup veri modelini kullanma
Gereksinimlerinize göre görseller, özel sorgular ve Pano oluşturmak için aşağıdaki alanları veri modeli bir parçası olarak sağlanan kullanabilirsiniz.

### <a name="alert"></a>Uyarı
Bu tabloda uyarı ilgili alanları hakkındaki ayrıntılar verilmektedir.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| AlertUniqueId_s |Metin |Oluşturulan uyarı benzersiz tanıtıcısı |
| AlertType_s |Metin |Uyarı, örneğin, yedekleme türü |
| AlertStatus_s |Metin |Uyarının durumunu, örneğin, etkin |
| AlertOccurrenceDateTime_s |Tarih/Saat |Tarihi ve uyarının oluşturulduğu saat |
| AlertSeverity_s |Metin |Uyarının önem derecesini, örneğin, kritik |
| EventName_s |Metin |Etkinliğin adı. Her zaman AzureBackupCentralReport |
| BackupItemUniqueId_s |Metin |Uyarıyla ilişkili yedekleme öğenin benzersiz tanıtıcısı |
| SchemaVersion_s |Metin |Geçerli örneğin şema sürümü **V1** |
| State_s |Metin |Örneğin, etkin, silinen uyarı nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu uyarı için ait olduğu için yedekleme, örneğin, IaaSVM Dosyaklasörü gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Geçerli işlem, örneğin, uyarı adı |
| Kategori |Metin |Tanılama veri kategorisini Log Analytics'e gönderilir. Her zaman AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Metin |Uyarıyla ilişkili korumalı sunucunun benzersiz tanıtıcısı |
| VaultUniqueId_s |Metin |Uyarıyla ilişkili korumalı kasa benzersiz tanıtıcısı |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanan veriler hakkında kaynağın benzersiz tanımlayıcısı. Örneğin, bir kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="backupitem"></a>BackupItem
Bu tablo, yedekleme öğesi ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Etkinliğin adı. Her zaman AzureBackupCentralReport |  
| BackupItemUniqueId_s |Metin |Yedekleme öğenin benzersiz tanıtıcısı |
| BackupItemId_s |Metin |Yedekleme öğesi tanıtıcısı |
| BackupItemName_s |Metin |Yedekleme öğesinin adı |
| BackupItemFriendlyName_s |Metin |Yedekleme öğesi kolay adı |
| BackupItemType_s |Metin |Yedekleme öğesi, örneğin, VM Dosyaklasörü türü |
| ProtectedServerName_s |Metin |Adı, hangi yedekleme öğesi için bir korumalı sunucunun ait olduğu |
| ProtectionState_s |Metin |Örneğin, korumalı, ProtectionStopped yedekleme öğesi geçerli koruma durumu |
| SchemaVersion_s |Metin |Bu gibi bir durumda şema sürümü **V1** |
| State_s |Metin |Örneğin, etkin, silinen yedekleme öğesi nesnenin durumu |
| BackupManagementType_s |Metin |Sağlayıcı türü için bu yedekleme öğesi ait olduğu için yedekleme, örneğin, IaaSVM Dosyaklasörü gerçekleştirmek için |
| OperationName |Metin |Örneğin, BackupItem işlemin adı |
| Kategori |Metin |Tanılama veri kategorisini Log Analytics'e gönderilir. Her zaman AzureBackupReport |
| Kaynak |Metin |Hangi veri kaynağı toplanır, örneğin, adı kurtarma Hizmetleri kasası |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak kimliği, kaynak kimliği, Kurtarma Hizmetleri kasası |
| SubscriptionId |Metin |Kaynak abonelik tanımlayıcısı (için örnek. Kurtarma Hizmetleri kasası) toplanmakta olan veriler için |
| ResourceGroup |Metin |Kaynak kaynak grubu (için örnek. Kurtarma Hizmetleri kasası) toplanmakta olan veriler için |
| ResourceProvider |Metin |Toplanmakta olan veriler, Microsoft.RecoveryServices için kaynak sağlayıcısı |
| ResourceType |Metin |Örneğin, toplanmakta olan veriler için kaynak türünü kasaları |

### <a name="backupitemassociation"></a>BackupItemAssociation
Bu tabloda, çeşitli varlıklar ile yedekleme öğesi ilişkilendirmeleri hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |  
| BackupItemUniqueId_s |Metin |Yedekleme öğesinin benzersiz kimliği |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen yedekleme öğesi ilişkilendirme nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - BackupItemAssociation adını temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Metin |Yedekleme öğesi ile ilişkilendirilen ilkesi için benzersiz tanımlayıcı |
| ProtectedServerUniqueId_s |Metin |Yedekleme öğesi ile ilişkilendirilen korumalı sunucu benzersiz tanıtıcısı |
| VaultUniqueId_s |Metin |Yedekleme öğesi içeren kasa benzersiz tanıtıcısı |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |Kaynak abonelik tanımlayıcısı (için örnek. Veri toplanmakta kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak kaynak grubu (için örnek. Veri toplanmakta kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanmakta olan veriler, Microsoft.RecoveryServices için kaynak sağlayıcısı |
| ResourceType |Metin |Örneğin, kasa için veri kaynağının türü toplanır |

### <a name="job"></a>İş
Bu tablo, projeyle ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Etkinliğin adı. Her zaman AzureBackupCentralReport |
| BackupItemUniqueId_s |Metin |Yedekleme öğenin benzersiz tanıtıcısı |
| SchemaVersion_s |Metin |Bu gibi bir durumda şema sürümü **V1** |
| State_s |Metin |Örneğin, etkin, silinen iş nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan adı geçerli işlemin - işi temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Metin |İşi, korumalı sunucunun benzersiz tanımlayıcı ilişkili |
| VaultUniqueId_s |Metin |Korumalı kasa benzersiz tanıtıcısı |
| JobOperation_s |Metin |İşlem için iş yedekleme, geri yükleme, yapılandırma yedekleme gibi çalıştırılır |
| JobStatus_s |Metin |Örneğin, tamamlandı, başarısız tamamlanmış işinin durumu |
| JobFailureCode_s |Metin |Hata kodu dizesi nedeniyle iş başarısız oldu |
| JobStartDateTime_s |Tarih/Saat |Tarih ve saat tarafından başlatılan çalışan iş yapılırken |
| BackupStorageDestination_s |Metin |Yedekleme depolama alanı, örneğin, bulut, diski hedef  |
| JobDurationInSecs_s | Sayı |Saniye cinsinden toplam iş süresi |
| DataTransferredInMB_s | Sayı |Bu proje için MB cinsinden aktarılan veriler|
| JobUniqueId_g |Metin |İşi belirlemek için benzersiz kimliği |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği|
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="policy"></a>İlke
Bu tablo, ilke ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - ilke adını temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Metin |İlke tanımlamak için benzersiz kimlik |
| PolicyName_s |Metin |Tanımlanan ilke adı |
| BackupFrequency_s |Metin |Sıklık ile yedeklemeler çalıştırılır, örneğin, günlük, haftalık |
| BackupTimes_s |Metin |Yedeklemeler, zamanlanan tarih ve saat |
| BackupDaysOfTheWeek_s |Metin |Ne zaman yedeklemeler zamanlandı haftanın günleri |
| RetentionDuration_s |Tam sayı |Yapılandırılan yedekleme bekletme süresi |
| DailyRetentionDuration_s |Tam sayı |Toplam elde tutma süresi yapılandırılan yedekleme için gün |
| DailyRetentionTimes_s |Metin |Tarih ve saat günlük bekletme zaman yapılandırıldı |
| WeeklyRetentionDuration_s |Ondalık sayı |Yapılandırılmış yedeklemeler için hafta cinsinden toplam haftalık tutma süresi |
| WeeklyRetentionTimes_s |Metin |Tarih ve saat haftalık bekletme zaman yapılandırılır |
| WeeklyRetentionDaysOfTheWeek_s |Metin |Haftalık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionDuration_s |Ondalık sayı |Toplam elde tutma süresi yapılandırılan yedeklemeler için bir ay içinde |
| MonthlyRetentionTimes_s |Metin |Tarih ve saat aylık bekletme zaman yapılandırılır |
| MonthlyRetentionFormat_s |Metin |Haftalık hafta için aylık bekletme, örneğin, bağlı olarak, gün için günlük yapılandırmasını türüne göre |
| MonthlyRetentionDaysOfTheWeek_s |Metin |Aylık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionWeeksOfTheMonth_s |Metin |Hafta, aylık bekletme yapılandırıldığında, ayın ilk, son vs. |
| YearlyRetentionDuration_s |Ondalık sayı |Toplam elde tutma süresi yapılandırılan yedeklemeler için yıl içinde |
| YearlyRetentionTimes_s |Metin |Tarih ve saat, yıllık bekletme yapılandırılır |
| YearlyRetentionMonthsOfTheYear_s |Metin |Yıl ay yıllık bekletme için seçili |
| YearlyRetentionFormat_s |Metin |Haftalık hafta yapılandırmasını yıllık bekletme, örneğin, bağlı olarak, gün için günlük türüne göre |
| YearlyRetentionDaysOfTheMonth_s |Metin |Yıllık bekletme için seçili ayın tarihleri |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="policyassociation"></a>PolicyAssociation
Bu tabloda, çeşitli varlıklar ile ilke ilişkilendirmesi hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - PolicyAssociation adını temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Metin |İlke tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Metin |Bu ilkenin ait olduğu kasanın benzersiz kimliği |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="protectedserver"></a>ProtectedServer
Bu tablo, korumalı sunucu ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| ProtectedServerName_s |Metin |Korumalı sunucu adı |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, Deleted, korumalı sunucu nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - ProtectedServer adını temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Metin |Korumalı sunucu benzersiz kimliği |
| RegisteredContainerId_s |Metin |Yedekleme için kayıtlı kapsayıcı kimliği |
| ProtectedServerType_s |Metin |Korumalı sunucu, örneğin, Windows türü |
| ProtectedServerFriendlyName_s |Metin |Korumalı sunucu kolay adı |
| AzureBackupAgentVersion_s |Metin |Aracı yedekleme sürümünün sürüm numarası |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Bu tablo, diğer varlıklarla ilişkilerini korumalı sunucu hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen korumalı sunucu ilişkilendirme nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - ProtectedServerAssociation adını temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Metin |Korumalı sunucu benzersiz kimliği |
| VaultUniqueId_s |Metin |Bu korumalı sunucunun ait olduğu kasanın benzersiz kimliği |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="storage"></a>Depolama
Bu tablo depolama ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| CloudStorageInBytes_s |Ondalık sayı |Hesaplanan yedeklemeler tarafından kullanılan yedekleme depolama en son değeri temel alarak bulut |
| ProtectedInstances_s |Ondalık sayı |Ön uç depolama faturalandırma, hesaplanmış dayalı olarak en son değeri hesaplamak için kullanılan korunan örnek sayısı |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen depolama nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - depolama adını temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Metin |Korumalı sunucunun depolama hesaplandığı benzersiz kimliği |
| VaultUniqueId_s |Metin |Depolama kasasının benzersiz kimliği hesaplanır |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="vault"></a>Kasa
Bu tablo, kasa ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen kasa nesnenin geçerli durumu |
| OperationName |Metin |Bu alan geçerli işlem - kasa adını temsil eder. |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| VaultUniqueId_s |Metin |Kasa benzersiz kimliği |
| VaultName_s |Metin |Kasa adı |
| AzureDataCenter_s |Metin |Kasanın bulunduğu veri merkezi |
| StorageReplicationType_s |Metin |Örneğin, GeoRedundant kasa için depolama çoğaltma türü |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

## <a name="next-steps"></a>Sonraki adımlar
Azure Backup raporları oluşturmak için veri modeli gözden geçirin, sonra başlatabilirsiniz [panosu oluşturma](../azure-monitor/learn/tutorial-logs-dashboards.md) Log analytics'te.
