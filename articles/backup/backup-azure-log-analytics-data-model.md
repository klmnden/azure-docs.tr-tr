---
title: Azure İzleyici, veri modeli için Azure Backup kaydeder.
description: Bu makalede Bahsediyor Azure İzleyici, Azure yedekleme verileri için veri modeli ayrıntıları günlüğe kaydeder.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: adigan
ms.openlocfilehash: dd4dad2cc3e541d3b6866c02341161dc1d9e1e6c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58075279"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Verileri Azure Backup için log Analytics veri modeli

Log Analytics veri modeli, özel uyarıları Log Analytics'ten oluşturmak için kullanın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

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
|AlertTimeToResolveInMinutes_s    | Sayı        |Bir uyarıyı çözümlemek için geçen süre. Etkin uyarılar için boş.         |
|AlertConsolidationStatus_s   |Metin         |Uyarı birleştirilmiş bir uyarı olup olmadığını belirleyin         |
|CountOfAlertsConsolidated_s     |Sayı         |Birleştirilmiş bir uyarı olup olmadığını birleştirilmiş uyarı sayısı          |
|AlertRaisedOn_s     |Metin         |Varlık uyarı veriliş türü         |
|AlertCode_s     |Metin         |Bir uyarı türü benzersiz olarak tanımlanabilmesi için kod         |
|RecommendedAction_s   |Metin         |Uyarıyı çözmek için önerilen eylem         |
| EventName_s |Metin |Etkinliğin adı. Her zaman AzureBackupCentralReport |
| BackupItemUniqueId_s |Metin |Uyarıyla ilişkili yedekleme öğenin benzersiz tanıtıcısı |
| SchemaVersion_s |Metin |Geçerli örneğin şema sürümü **V2** |
| State_s |Metin |Örneğin, etkin, silinen uyarı nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu uyarı için ait olduğu için yedekleme, örneğin, IaaSVM Dosyaklasörü gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Geçerli işlem, örneğin, uyarı adı |
| Kategori |Metin |Tanılama veri kategorisini Azure İzleyici günlüklerine gönderildi. Her zaman AzureBackupReport |
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
| ProtectionState_s |Metin |Örneğin, korumalı, ProtectionStopped yedekleme öğesi geçerli koruma durumu |
| ProtectionGroupName_s |Metin | Yedekleme öğesi koruma grubunun adı, SC DPM ve MABS, varsa korunuyor|
| SecondaryBackupProtectionState_s |Metin |Yedekleme öğesi için ikincil koruma etkinleştirilip etkinleştirilmediği|
| SchemaVersion_s |Metin |Bu gibi bir durumda şema sürümü **V2** |
| State_s |Metin |Örneğin, etkin, silinen yedekleme öğesi nesnenin durumu |
| BackupManagementType_s |Metin |Sağlayıcı türü için bu yedekleme öğesi ait olduğu için yedekleme, örneğin, IaaSVM Dosyaklasörü gerçekleştirmek için |
| OperationName |Metin |Örneğin, BackupItem işlemin adı |
| Kategori |Metin |Tanılama veri kategorisini Azure İzleyici günlüklerine gönderildi. Her zaman AzureBackupReport |
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
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Metin |Örneğin, etkin, silinen yedekleme öğesi ilişkilendirme nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| BackupItemSourceSize_s |Metin | Yedekleme öğesi ön uç boyutu |
| BackupManagementServerUniqueId_s |Metin | Yedekleme Yönetimi Sunucusu yedekleme öğesi benzersiz olarak tanımlanabilmesi için alan, varsa korunuyor |
| Kategori |Metin |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| OperationName |Metin |Bu alan geçerli işlem - BackupItemAssociation adını temsil eder. |
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

### <a name="backupmanagementserver"></a>BackupManagementServer

Bu tabloda, çeşitli varlıklar ile yedekleme öğesi ilişkilendirmeleri hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
|BackupManagementServerName_s     |Metin         |Yedekleme Yönetimi sunucusu adını        |
|AzureBackupAgentVersion_s     |Metin         |Yedekleme yönetim sunucusundaki Azure Yedekleme aracısı sürümü          |
|BackupManagementServerVersion_s     |Metin         |Yedekleme Yönetimi Sunucusu sürümü|
|BackupManagementServerOSVersion_s    |Metin            |Yedekleme yönetim sunucusunun işletim sistemi sürümü|
|BackupManagementServerType_s     |Metin         |Yedekleme Yönetimi Sunucusu, MABS, SC DPM olarak türü|
|BackupManagementServerUniqueId_s     |Metin         |Yedekleme Yönetimi Sunucusu benzersiz olarak tanımlanabilmesi için alan       |
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
| SchemaVersion_s |Metin |Bu gibi bir durumda şema sürümü **V2** |
| State_s |Metin |Örneğin, etkin, silinen iş nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan adı geçerli işlemin - işi temsil eder. |
| Kategori |Metin |Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Metin |İşi, korumalı sunucunun benzersiz tanımlayıcı ilişkili |
| ProtectedContainerUniqueId_s |Metin | İşin çalıştırıldığı korumalı kapsayıcı tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Metin |Korumalı kasa benzersiz tanıtıcısı |
| JobOperation_s |Metin |İşlem için iş yedekleme, geri yükleme, yapılandırma yedekleme gibi çalıştırılır |
| JobStatus_s |Metin |Örneğin, tamamlandı, başarısız tamamlanmış işinin durumu |
| JobFailureCode_s |Metin |Hata kodu dizesi nedeniyle iş başarısız oldu |
| JobStartDateTime_s |Tarih/Saat |Tarih ve saat tarafından başlatılan çalışan iş yapılırken |
| BackupStorageDestination_s |Metin |Yedekleme depolama alanı, örneğin, bulut, diski hedef  |
| AdHocOrScheduledJob_s |Metin | İşin geçici veya zamanlanmış olup olmadığını belirlemek için alan |
| JobDurationInSecs_s | Sayı |Saniye cinsinden toplam iş süresi |
| DataTransferredInMB_s | Sayı |Bu proje için MB cinsinden aktarılan veriler|
| JobUniqueId_g |Metin |İşi belirlemek için benzersiz kimliği |
| RecoveryJobDestination_s |Metin | Verilerin nerede kurtarılmadan hedefi bir kurtarma işi |
| RecoveryJobRPDateTime_s |DateTime | Tarih, kurtarılmakta olan kurtarma noktasının oluşturulduğu saat |
| RecoveryJobRPLocation_s |Metin | Kurtarılmakta olan kurtarma noktasını depolandığı konumu|
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği|
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="policy"></a>İlke

Bu tablo, ilke ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Geçerli sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Metin ||Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin ||Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Metin ||Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Metin ||Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin ||Bu alan geçerli işlem - ilke adını temsil eder. |
| Kategori |Metin ||Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin ||Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Metin ||İlke tanımlamak için benzersiz kimlik |
| PolicyName_s |Metin ||Tanımlanan ilke adı |
| BackupFrequency_s |Metin ||Sıklık ile yedeklemeler çalıştırılır, örneğin, günlük, haftalık |
| BackupTimes_s |Metin ||Yedeklemeler, zamanlanan tarih ve saat |
| BackupDaysOfTheWeek_s |Metin ||Ne zaman yedeklemeler zamanlandı haftanın günleri |
| RetentionDuration_s |Tam sayı ||Yapılandırılan yedekleme bekletme süresi |
| DailyRetentionDuration_s |Tam sayı ||Toplam elde tutma süresi yapılandırılan yedekleme için gün |
| DailyRetentionTimes_s |Metin ||Tarih ve saat günlük bekletme zaman yapılandırıldı |
| WeeklyRetentionDuration_s |Ondalık sayı ||Yapılandırılmış yedeklemeler için hafta cinsinden toplam haftalık tutma süresi |
| WeeklyRetentionTimes_s |Metin ||Tarih ve saat haftalık bekletme zaman yapılandırılır |
| WeeklyRetentionDaysOfTheWeek_s |Metin ||Haftalık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionDuration_s |Ondalık sayı ||Toplam elde tutma süresi yapılandırılan yedeklemeler için bir ay içinde |
| MonthlyRetentionTimes_s |Metin ||Tarih ve saat aylık bekletme zaman yapılandırılır |
| MonthlyRetentionFormat_s |Metin ||Haftalık hafta için aylık bekletme, örneğin, bağlı olarak, gün için günlük yapılandırmasını türüne göre |
| MonthlyRetentionDaysOfTheWeek_s |Metin ||Aylık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionWeeksOfTheMonth_s |Metin ||Hafta, aylık bekletme yapılandırıldığında, ayın ilk, son vs. |
| YearlyRetentionDuration_s |Ondalık sayı ||Toplam elde tutma süresi yapılandırılan yedeklemeler için yıl içinde |
| YearlyRetentionTimes_s |Metin ||Tarih ve saat, yıllık bekletme yapılandırılır |
| YearlyRetentionMonthsOfTheYear_s |Metin ||Yıl ay yıllık bekletme için seçili |
| YearlyRetentionFormat_s |Metin ||Haftalık hafta yapılandırmasını yıllık bekletme, örneğin, bağlı olarak, gün için günlük türüne göre | |
| YearlyRetentionDaysOfTheMonth_s |Metin ||Yıllık bekletme için seçili ayın tarihleri |
| SynchronisationFrequencyPerDay_s |Tam sayı |v2|SC DPM ve MABS için bir dosya yedeklemesi eşitlenen bir gün sayısı |
| DiffBackupFormat_s |Metin |v2|Biçiminde değişiklik yedekleri için SQL Azure VM yedeklemesi |
| DiffBackupTime_s |Zaman |v2|Yedekleri için SQL'de Azure VM yedekleme için saat|
| DiffBackupRetentionDuration_s |Ondalık sayı |v2|SQL Azure VM yedeklemesi için değişiklik yedekleri bekletme süresi|
| LogBackupFrequency_s |Ondalık sayı |v2|İçin SQL günlük yedekleme sıklığı|
| LogBackupRetentionDuration_s |Ondalık sayı |v2|SQL Azure VM yedeklemesi için günlük yedeklemeler için elde tutma süresi|
| DiffBackupDaysofTheWeek_s |Metin |v2|SQL Azure VM yedeklemesi için farklı yedeklemeler için haftanın günleri|
| SourceSystem |Metin ||Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin ||Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin ||(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin ||Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin ||Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin ||Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="policyassociation"></a>PolicyAssociation

Bu tabloda, çeşitli varlıklar ile ilke ilişkilendirmesi hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Geçerli sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Metin ||Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin ||Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Metin ||Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Metin ||Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin ||Bu alan geçerli işlem - PolicyAssociation adını temsil eder. |
| Kategori |Metin ||Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin ||Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Metin ||İlke tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Metin ||Bu ilkenin ait olduğu kasanın benzersiz kimliği |
| BackupManagementServerUniqueId_s |Metin |v2 |Yedekleme Yönetimi Sunucusu yedekleme öğesi benzersiz olarak tanımlanabilmesi için alan, varsa korunuyor        |
| SourceSystem |Metin ||Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin ||Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin ||(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin ||Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin ||Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin ||Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="protected-container"></a>Korumalı kapsayıcı

Bu tabloda korumalı kapsayıcılar hakkında temel alan sağlar. (V1'de ProtectedServer idi)

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Metin | Bir korumalı kapsayıcı benzersiz olarak tanımlanabilmesi için alan |
| ProtectedContainerOSType_s |Metin |Korumalı kapsayıcı işletim sistemi türü |
| ProtectedContainerOSVersion_s |Metin |Korumalı kapsayıcının işletim sistemi sürümü |
| AgentVersion_s |Metin |Sürüm numarasını Aracısı yedekleme veya koruma Aracısı (durumunda, SC DPM ve MABS) |
| BackupManagementType_s |Metin |Yedekleme gibi IaaSVM Dosyaklasörü gerçekleştirmek için sağlayıcı türü |
| EntityState_s |Metin |Örneğin, etkin, silinen korumalı sunucu nesnenin geçerli durumu |
| ProtectedContainerFriendlyName_s |Metin |Korumalı sunucu kolay adı |
| ProtectedContainerName_s |Metin |Korumalı kapsayıcının adı |
| ProtectedContainerWorkloadType_s |Metin |Örneğin, IaaSVMContainer korumalı kapsayıcı türü desteklenen |
| ProtectedContainerLocation_s |Metin |Korumalı kapsayıcı şirket içinde ister azure'da |
| ProtectedContainerType_s |Metin |Korumalı kapsayıcı bir sunucu veya bir kapsayıcı olup |

### <a name="storage"></a>Depolama

Bu tablo depolama ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| CloudStorageInBytes_s |Ondalık sayı |Hesaplanan yedeklemeler tarafından kullanılan yedekleme depolama en son değeri temel alarak bulut |
| ProtectedInstances_s |Ondalık sayı |Ön uç depolama faturalandırma, hesaplanmış dayalı olarak en son değeri hesaplamak için kullanılan korunan örnek sayısı |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Metin |Örneğin, etkin, silinen depolama nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - depolama adını temsil eder. |
| Kategori |Metin |Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Metin |Korumalı sunucunun depolama hesaplandığı benzersiz kimliği |
| VaultUniqueId_s |Metin |Depolama kasasının benzersiz kimliği hesaplanır |
| SourceSystem |Metin |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Metin |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Metin |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Metin |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="storageassociation"></a>StorageAssociation

Bu tablo depolama diğer varlıklara bağlanma depolama ile ilgili temel alanlarda sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- |  --- |
| StorageUniqueId_s |Metin |Depolama varlık tanımlamak için kullanılan benzersiz kimliği |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V2** |
| BackupItemUniqueId_s |Metin |Depolama varlıkla ilgili yedekleme öğesi tanımlamak için kullanılan benzersiz kimliği |
| BackupManagementServerUniqueId_s |Metin |Depolama varlıkla ilgili yedekleme yönetim sunucusunu tanımlamak için kullanılan benzersiz kimliği|
| VaultUniqueId_s |Metin |Depolama varlıkla ilgili kasayı tanımlamak için kullanılan benzersiz kimliği|
| StorageConsumedInMBs_s |Sayı|Karşılık gelen depolama alanında karşılık gelen yedekleme öğesi tarafından kullanılan depolama boyutu |
| StorageAllocatedInMBs_s |Sayı |Karşılık gelen yedekleme öğesi türü Disk tarafından karşılık gelen depolama ayrılan depolama boyutu|

### <a name="vault"></a>Kasa

Bu tablo, kasa ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Metin |Örneğin, etkin, silinen kasa nesnenin geçerli durumu |
| OperationName |Metin |Bu alan geçerli işlem - kasa adını temsil eder. |
| Kategori |Metin |Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
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

### <a name="backup-management-server"></a>Yedekleme Yönetimi Sunucusu

Bu tablo, yedekleme yönetim sunucuları hakkında temel alan sağlar.

|Alan  |Veri Türü  | Açıklama  |
|---------|---------|----------|
|BackupManagmentServerName_s     |Metin         |Yedekleme Yönetimi sunucusu adını        |
|AzureBackupAgentVersion_s     |Metin         |Yedekleme yönetim sunucusundaki Azure Yedekleme aracısı sürümü          |
|BackupManagmentServerVersion_s     |Metin         |Yedekleme Yönetimi Sunucusu sürümü|
|BackupManagmentServerOSVersion_s     |Metin            |Yedekleme yönetim sunucusunun işletim sistemi sürümü|
|BackupManagementServerType_s     |Metin         |Yedekleme Yönetimi Sunucusu, MABS, SC DPM olarak türü|
|BackupManagmentServerUniqueId_s     |Metin         |Yedekleme Yönetimi Sunucusu benzersiz olarak tanımlanabilmesi için alan       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

Bu tablo, bir birim ilişkilendirildiği workload(s) belirtir.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| StorageUniqueId_s |Metin |Depolama varlık tanımlamak için kullanılan benzersiz kimliği |
| BackupItemType_s |Metin |Bu birimi tercih edilen depolama alanı olan iş yükleri|

### <a name="protectedinstance"></a>ProtectedInstance

Bu tablo, ilgili alanları temel korumalı örnekler sağlar.

| Alan | Veri Türü |Geçerli sürümler | Açıklama |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Metin |v2|DPM, MABS kullanan VM'ler için yedekleme öğesi tanımlamak için kullanılan benzersiz kimliği yedeklendi|
| ProtectedContainerUniqueId_s |Metin |v2|DPM, MABS kullanarak Vm'leri dışında her şeyi için korumalı kapsayıcı tanımlamak için kullanılan benzersiz kimliği yedeklendi|
| ProtectedInstanceCount_s |Metin |v2|Sayısı, korunan örnekler ilişkili yedekleme öğesi veya korumalı kapsayıcısını tarih-saat|

### <a name="recoverypoint"></a>RecoveryPoint

Bu tabloda temel kurtarma sağlar. ilgili alanları gelin.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| BackupItemUniqueId_s |Metin |DPM, MABS kullanan VM'ler için yedekleme öğesi tanımlamak için kullanılan benzersiz kimliği yedeklendi|
| OldestRecoveryPointTime_s |Metin |Yedekleme öğesi için en eski kurtarma noktası tarih saat|
| OldestRecoveryPointLocation_s |Metin |Yedekleme öğesi için en eski kurtarma noktası konumu|
| LatestRecoveryPointTime_s |Metin |Yedekleme öğesi için en son kurtarma noktasının tarih saat|
| LatestRecoveryPointLocation_s |Metin |Yedekleme öğesi için en son kurtarma noktası konumu|

## <a name="next-steps"></a>Sonraki adımlar

Veri modeli gözden geçirin, sonra başlatabilirsiniz [özel sorgular oluşturma](../azure-monitor/learn/tutorial-logs-dashboards.md) kendi Pano oluşturmak üzere Azure İzleyici günlüklerine.
