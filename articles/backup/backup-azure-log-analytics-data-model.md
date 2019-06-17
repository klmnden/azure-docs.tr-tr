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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61234984"
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
| AlertUniqueId_s |Text |Oluşturulan uyarı benzersiz tanıtıcısı |
| AlertType_s |Text |Uyarı, örneğin, yedekleme türü |
| AlertStatus_s |Text |Uyarının durumunu, örneğin, etkin |
| AlertOccurrenceDateTime_s |Tarih/Saat |Tarihi ve uyarının oluşturulduğu saat |
| AlertSeverity_s |Text |Uyarının önem derecesini, örneğin, kritik |
|AlertTimeToResolveInMinutes_s    | Sayı        |Bir uyarıyı çözümlemek için geçen süre. Etkin uyarılar için boş.         |
|AlertConsolidationStatus_s   |Text         |Uyarı birleştirilmiş bir uyarı olup olmadığını belirleyin         |
|CountOfAlertsConsolidated_s     |Sayı         |Birleştirilmiş bir uyarı olup olmadığını birleştirilmiş uyarı sayısı          |
|AlertRaisedOn_s     |Text         |Varlık uyarı veriliş türü         |
|AlertCode_s     |Text         |Bir uyarı türü benzersiz olarak tanımlanabilmesi için kod         |
|RecommendedAction_s   |Text         |Uyarıyı çözmek için önerilen eylem         |
| EventName_s |Text |Olayın adı. Her zaman AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Uyarıyla ilişkili yedekleme öğenin benzersiz tanıtıcısı |
| SchemaVersion_s |Text |Geçerli örneğin şema sürümü **V2** |
| State_s |Text |Örneğin, etkin, silinen uyarı nesnenin geçerli durumu |
| BackupManagementType_s |Text |Bu uyarı için ait olduğu için yedekleme, örneğin, IaaSVM Dosyaklasörü gerçekleştirmek için sağlayıcı türü |
| OperationName |Text |Geçerli işlem, örneğin, uyarı adı |
| Kategori |Text |Tanılama veri kategorisini Azure İzleyici günlüklerine gönderildi. Her zaman AzureBackupReport |
| Resource |Text |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Text |Uyarıyla ilişkili korumalı sunucunun benzersiz tanıtıcısı |
| VaultUniqueId_s |Text |Uyarıyla ilişkili korumalı kasa benzersiz tanıtıcısı |
| SourceSystem |Text |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text |Toplanan veriler hakkında kaynağın benzersiz tanımlayıcısı. Örneğin, bir kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Text |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Text |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Text |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Text |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="backupitem"></a>BackupItem

Bu tablo, yedekleme öğesi ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Olayın adı. Her zaman AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Yedekleme öğenin benzersiz tanıtıcısı |
| BackupItemId_s |Text |Yedekleme öğesi tanıtıcısı |
| BackupItemName_s |Text |Yedekleme öğesinin adı |
| BackupItemFriendlyName_s |Text |Yedekleme öğesi kolay adı |
| BackupItemType_s |Text |Yedekleme öğesi, örneğin, VM Dosyaklasörü türü |
| ProtectionState_s |Text |Örneğin, korumalı, ProtectionStopped yedekleme öğesi geçerli koruma durumu |
| ProtectionGroupName_s |Text | Yedekleme öğesi koruma grubunun adı, SC DPM ve MABS, varsa korunuyor|
| SecondaryBackupProtectionState_s |Text |Yedekleme öğesi için ikincil koruma etkinleştirilip etkinleştirilmediği|
| SchemaVersion_s |Text |Bu gibi bir durumda şema sürümü **V2** |
| State_s |Text |Örneğin, etkin, silinen yedekleme öğesi nesnenin durumu |
| BackupManagementType_s |Text |Sağlayıcı türü için bu yedekleme öğesi ait olduğu için yedekleme, örneğin, IaaSVM Dosyaklasörü gerçekleştirmek için |
| OperationName |Text |Örneğin, BackupItem işlemin adı |
| Kategori |Text |Tanılama veri kategorisini Azure İzleyici günlüklerine gönderildi. Her zaman AzureBackupReport |
| Resource |Text |Hangi veri kaynağı toplanır, örneğin, adı kurtarma Hizmetleri kasası |
| SourceSystem |Text |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text |Toplanmakta olan veriler için kaynak kimliği, kaynak kimliği, Kurtarma Hizmetleri kasası |
| SubscriptionId |Text |Kaynak abonelik tanımlayıcısı (için örnek. Kurtarma Hizmetleri kasası) toplanmakta olan veriler için |
| ResourceGroup |Text |Kaynak kaynak grubu (için örnek. Kurtarma Hizmetleri kasası) toplanmakta olan veriler için |
| ResourceProvider |Text |Toplanmakta olan veriler, Microsoft.RecoveryServices için kaynak sağlayıcısı |
| ResourceType |Text |Örneğin, toplanmakta olan veriler için kaynak türünü kasaları |

### <a name="backupitemassociation"></a>BackupItemAssociation

Bu tabloda, çeşitli varlıklar ile yedekleme öğesi ilişkilendirmeleri hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |  
| BackupItemUniqueId_s |Text |Yedekleme öğesinin benzersiz kimliği |
| SchemaVersion_s |Text |Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Text |Örneğin, etkin, silinen yedekleme öğesi ilişkilendirme nesnenin geçerli durumu |
| BackupManagementType_s |Text |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| BackupItemSourceSize_s |Text | Yedekleme öğesi ön uç boyutu |
| BackupManagementServerUniqueId_s |Text | Yedekleme Yönetimi Sunucusu yedekleme öğesi benzersiz olarak tanımlanabilmesi için alan, varsa korunuyor |
| Kategori |Text |Bu alan, Log Analytics'e gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| OperationName |Text |Bu alan geçerli işlem - BackupItemAssociation adını temsil eder. |
| Resource |Text |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Text |Yedekleme öğesi ile ilişkilendirilen ilkesi için benzersiz tanımlayıcı |
| ProtectedServerUniqueId_s |Text |Yedekleme öğesi ile ilişkilendirilen korumalı sunucu benzersiz tanıtıcısı |
| VaultUniqueId_s |Text |Yedekleme öğesi içeren kasa benzersiz tanıtıcısı |
| SourceSystem |Text |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Text |Kaynak abonelik tanımlayıcısı (için örnek. Veri toplanmakta kurtarma Hizmetleri kasası) |
| ResourceGroup |Text |Kaynak kaynak grubu (için örnek. Veri toplanmakta kurtarma Hizmetleri kasası) |
| ResourceProvider |Text |Toplanmakta olan veriler, Microsoft.RecoveryServices için kaynak sağlayıcısı |
| ResourceType |Text |Örneğin, kasa için veri kaynağının türü toplanır |

### <a name="backupmanagementserver"></a>BackupManagementServer

Bu tabloda, çeşitli varlıklar ile yedekleme öğesi ilişkilendirmeleri hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
|BackupManagementServerName_s     |Text         |Yedekleme Yönetimi sunucusu adını        |
|AzureBackupAgentVersion_s     |Text         |Yedekleme yönetim sunucusundaki Azure Yedekleme aracısı sürümü          |
|BackupManagementServerVersion_s     |Text         |Yedekleme Yönetimi Sunucusu sürümü|
|BackupManagementServerOSVersion_s    |Text            |Yedekleme yönetim sunucusunun işletim sistemi sürümü|
|BackupManagementServerType_s     |Text         |Yedekleme Yönetimi Sunucusu, MABS, SC DPM olarak türü|
|BackupManagementServerUniqueId_s     |Text         |Yedekleme Yönetimi Sunucusu benzersiz olarak tanımlanabilmesi için alan       |
| SourceSystem |Text |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Text |Kaynak abonelik tanımlayıcısı (için örnek. Veri toplanmakta kurtarma Hizmetleri kasası) |
| ResourceGroup |Text |Kaynak kaynak grubu (için örnek. Veri toplanmakta kurtarma Hizmetleri kasası) |
| ResourceProvider |Text |Toplanmakta olan veriler, Microsoft.RecoveryServices için kaynak sağlayıcısı |
| ResourceType |Text |Örneğin, kasa için veri kaynağının türü toplanır |

### <a name="job"></a>İş

Bu tablo, projeyle ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Olayın adı. Her zaman AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Yedekleme öğenin benzersiz tanıtıcısı |
| SchemaVersion_s |Text |Bu gibi bir durumda şema sürümü **V2** |
| State_s |Text |Örneğin, etkin, silinen iş nesnenin geçerli durumu |
| BackupManagementType_s |Text |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Text |Bu alan adı geçerli işlemin - işi temsil eder. |
| Kategori |Text |Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Resource |Text |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Text |İşi, korumalı sunucunun benzersiz tanımlayıcı ilişkili |
| ProtectedContainerUniqueId_s |Text | İşin çalıştırıldığı korumalı kapsayıcı tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Text |Korumalı kasa benzersiz tanıtıcısı |
| JobOperation_s |Text |İşlem için iş yedekleme, geri yükleme, yapılandırma yedekleme gibi çalıştırılır |
| JobStatus_s |Text |Örneğin, tamamlandı, başarısız tamamlanmış işinin durumu |
| JobFailureCode_s |Text |Hata kodu dizesi nedeniyle iş başarısız oldu |
| JobStartDateTime_s |Tarih/Saat |Tarih ve saat tarafından başlatılan çalışan iş yapılırken |
| BackupStorageDestination_s |Text |Yedekleme depolama alanı, örneğin, bulut, diski hedef  |
| AdHocOrScheduledJob_s |Text | İşin geçici veya zamanlanmış olup olmadığını belirlemek için alan |
| JobDurationInSecs_s | Sayı |Saniye cinsinden toplam iş süresi |
| DataTransferredInMB_s | Sayı |Bu proje için MB cinsinden aktarılan veriler|
| JobUniqueId_g |Text |İşi belirlemek için benzersiz kimliği |
| RecoveryJobDestination_s |Text | Verilerin nerede kurtarılmadan hedefi bir kurtarma işi |
| RecoveryJobRPDateTime_s |DateTime | Tarih, kurtarılmakta olan kurtarma noktasının oluşturulduğu saat |
| RecoveryJobRPLocation_s |Text | Kurtarılmakta olan kurtarma noktasını depolandığı konumu|
| SourceSystem |Text |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği|
| SubscriptionId |Text |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Text |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Text |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Text |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="policy"></a>İlke

Bu tablo, ilke ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Geçerli sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Text ||Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Text ||Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Text ||Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Text ||Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Text ||Bu alan geçerli işlem - ilke adını temsil eder. |
| Kategori |Text ||Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Resource |Text ||Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Text ||İlke tanımlamak için benzersiz kimlik |
| PolicyName_s |Text ||Tanımlanan ilke adı |
| BackupFrequency_s |Text ||Sıklık ile yedeklemeler çalıştırılır, örneğin, günlük, haftalık |
| BackupTimes_s |Text ||Yedeklemeler, zamanlanan tarih ve saat |
| BackupDaysOfTheWeek_s |Text ||Ne zaman yedeklemeler zamanlandı haftanın günleri |
| RetentionDuration_s |Tam sayı ||Yapılandırılan yedekleme bekletme süresi |
| DailyRetentionDuration_s |Tam sayı ||Toplam elde tutma süresi yapılandırılan yedekleme için gün |
| DailyRetentionTimes_s |Text ||Tarih ve saat günlük bekletme zaman yapılandırıldı |
| WeeklyRetentionDuration_s |Ondalık sayı ||Yapılandırılmış yedeklemeler için hafta cinsinden toplam haftalık tutma süresi |
| WeeklyRetentionTimes_s |Text ||Tarih ve saat haftalık bekletme zaman yapılandırılır |
| WeeklyRetentionDaysOfTheWeek_s |Text ||Haftalık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionDuration_s |Ondalık sayı ||Toplam elde tutma süresi yapılandırılan yedeklemeler için bir ay içinde |
| MonthlyRetentionTimes_s |Text ||Tarih ve saat aylık bekletme zaman yapılandırılır |
| MonthlyRetentionFormat_s |Text ||Haftalık hafta için aylık bekletme, örneğin, bağlı olarak, gün için günlük yapılandırmasını türüne göre |
| MonthlyRetentionDaysOfTheWeek_s |Text ||Aylık bekletme için haftanın günü seçilmedi |
| MonthlyRetentionWeeksOfTheMonth_s |Text ||Hafta, aylık bekletme yapılandırıldığında, ayın ilk, son vs. |
| YearlyRetentionDuration_s |Ondalık sayı ||Toplam elde tutma süresi yapılandırılan yedeklemeler için yıl içinde |
| YearlyRetentionTimes_s |Text ||Tarih ve saat, yıllık bekletme yapılandırılır |
| YearlyRetentionMonthsOfTheYear_s |Text ||Yıl ay yıllık bekletme için seçili |
| YearlyRetentionFormat_s |Text ||Haftalık hafta yapılandırmasını yıllık bekletme, örneğin, bağlı olarak, gün için günlük türüne göre | |
| YearlyRetentionDaysOfTheMonth_s |Text ||Yıllık bekletme için seçili ayın tarihleri |
| SynchronisationFrequencyPerDay_s |Tam sayı |v2|SC DPM ve MABS için bir dosya yedeklemesi eşitlenen bir gün sayısı |
| DiffBackupFormat_s |Text |v2|Biçiminde değişiklik yedekleri için SQL Azure VM yedeklemesi |
| DiffBackupTime_s |Zaman |v2|Yedekleri için SQL'de Azure VM yedekleme için saat|
| DiffBackupRetentionDuration_s |Ondalık sayı |v2|SQL Azure VM yedeklemesi için değişiklik yedekleri bekletme süresi|
| LogBackupFrequency_s |Ondalık sayı |v2|İçin SQL günlük yedekleme sıklığı|
| LogBackupRetentionDuration_s |Ondalık sayı |v2|SQL Azure VM yedeklemesi için günlük yedeklemeler için elde tutma süresi|
| DiffBackupDaysofTheWeek_s |Text |v2|SQL Azure VM yedeklemesi için farklı yedeklemeler için haftanın günleri|
| SourceSystem |Text ||Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text ||Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Text ||(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Text ||Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Text ||Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Text ||Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="policyassociation"></a>PolicyAssociation

Bu tabloda, çeşitli varlıklar ile ilke ilişkilendirmesi hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Geçerli sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Text ||Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Text ||Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Text ||Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Text ||Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Text ||Bu alan geçerli işlem - PolicyAssociation adını temsil eder. |
| Kategori |Text ||Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Resource |Text ||Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| PolicyUniqueId_g |Text ||İlke tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Text ||Bu ilkenin ait olduğu kasanın benzersiz kimliği |
| BackupManagementServerUniqueId_s |Text |v2 |Yedekleme Yönetimi Sunucusu yedekleme öğesi benzersiz olarak tanımlanabilmesi için alan, varsa korunuyor        |
| SourceSystem |Text ||Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text ||Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Text ||(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Text ||Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Text ||Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Text ||Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="protected-container"></a>Korumalı kapsayıcı

Bu tabloda korumalı kapsayıcılar hakkında temel alan sağlar. (V1'de ProtectedServer idi)

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Text | Bir korumalı kapsayıcı benzersiz olarak tanımlanabilmesi için alan |
| ProtectedContainerOSType_s |Text |Korumalı kapsayıcı işletim sistemi türü |
| ProtectedContainerOSVersion_s |Text |Korumalı kapsayıcının işletim sistemi sürümü |
| AgentVersion_s |Text |Sürüm numarasını Aracısı yedekleme veya koruma Aracısı (durumunda, SC DPM ve MABS) |
| BackupManagementType_s |Text |Yedekleme gibi IaaSVM Dosyaklasörü gerçekleştirmek için sağlayıcı türü |
| EntityState_s |Text |Örneğin, etkin, silinen korumalı sunucu nesnenin geçerli durumu |
| ProtectedContainerFriendlyName_s |Text |Korumalı sunucu kolay adı |
| ProtectedContainerName_s |Text |Korumalı kapsayıcının adı |
| ProtectedContainerWorkloadType_s |Text |Örneğin, IaaSVMContainer korumalı kapsayıcı türü desteklenen |
| ProtectedContainerLocation_s |Text |Korumalı kapsayıcı şirket içinde ister azure'da |
| ProtectedContainerType_s |Text |Korumalı kapsayıcı bir sunucu veya bir kapsayıcı olup |

### <a name="storage"></a>Depolama

Bu tablo depolama ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| CloudStorageInBytes_s |Ondalık sayı |Hesaplanan yedeklemeler tarafından kullanılan yedekleme depolama en son değeri temel alarak bulut |
| ProtectedInstances_s |Ondalık sayı |Ön uç depolama faturalandırma, hesaplanmış dayalı olarak en son değeri hesaplamak için kullanılan korunan örnek sayısı |
| EventName_s |Text |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Text |Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Text |Örneğin, etkin, silinen depolama nesnenin geçerli durumu |
| BackupManagementType_s |Text |Sunucu yedekleme işi, örneğin, IaaSVM, Dosyaklasörü yapmak için sağlayıcı türü |
| OperationName |Text |Bu alan geçerli işlem - depolama adını temsil eder. |
| Kategori |Text |Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Resource |Text |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| ProtectedServerUniqueId_s |Text |Korumalı sunucunun depolama hesaplandığı benzersiz kimliği |
| VaultUniqueId_s |Text |Depolama kasasının benzersiz kimliği hesaplanır |
| SourceSystem |Text |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Text |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Text |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Text |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Text |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="storageassociation"></a>StorageAssociation

Bu tablo depolama diğer varlıklara bağlanma depolama ile ilgili temel alanlarda sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- |  --- |
| StorageUniqueId_s |Text |Depolama varlık tanımlamak için kullanılan benzersiz kimliği |
| SchemaVersion_s |Text |Bu alan geçerli şema sürümünü gösterir, **V2** |
| BackupItemUniqueId_s |Text |Depolama varlıkla ilgili yedekleme öğesi tanımlamak için kullanılan benzersiz kimliği |
| BackupManagementServerUniqueId_s |Text |Depolama varlıkla ilgili yedekleme yönetim sunucusunu tanımlamak için kullanılan benzersiz kimliği|
| VaultUniqueId_s |Text |Depolama varlıkla ilgili kasayı tanımlamak için kullanılan benzersiz kimliği|
| StorageConsumedInMBs_s |Sayı|Karşılık gelen depolama alanında karşılık gelen yedekleme öğesi tarafından kullanılan depolama boyutu |
| StorageAllocatedInMBs_s |Sayı |Karşılık gelen yedekleme öğesi türü Disk tarafından karşılık gelen depolama ayrılan depolama boyutu|

### <a name="vault"></a>Kasa

Bu tablo, kasa ile ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport olur |
| SchemaVersion_s |Text |Bu alan geçerli şema sürümünü gösterir, **V2** |
| State_s |Text |Örneğin, etkin, silinen kasa nesnenin geçerli durumu |
| OperationName |Text |Bu alan geçerli işlem - kasa adını temsil eder. |
| Kategori |Text |Bu alan, Azure İzleyici günlüklerine gönderilen tanılama veri kategorisini temsil eder, AzureBackupReport |
| Resource |Text |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasası adını gösterir |
| VaultUniqueId_s |Text |Kasa benzersiz kimliği |
| VaultName_s |Text |Kasa adı |
| AzureDataCenter_s |Text |Kasanın bulunduğu veri merkezi |
| StorageReplicationType_s |Text |Örneğin, GeoRedundant kasa için depolama çoğaltma türü |
| SourceSystem |Text |Geçerli verileri - Azure'nın kaynak sistem |
| ResourceId |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasasının kaynak kimliği |
| SubscriptionId |Text |(Ör kaynak abonelik tanımlayıcısı. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceGroup |Text |Kaynak (ör kaynak grubu. Toplanan veriler için kurtarma Hizmetleri kasası) |
| ResourceProvider |Text |Toplanan veriler için kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Text |Kaynak türü için verileri toplanır. Örneğin, kasaları |

### <a name="backup-management-server"></a>Yedekleme Yönetimi Sunucusu

Bu tablo, yedekleme yönetim sunucuları hakkında temel alan sağlar.

|Alan  |Veri Türü  | Açıklama  |
|---------|---------|----------|
|BackupManagmentServerName_s     |Text         |Yedekleme Yönetimi sunucusu adını        |
|AzureBackupAgentVersion_s     |Text         |Yedekleme yönetim sunucusundaki Azure Yedekleme aracısı sürümü          |
|BackupManagmentServerVersion_s     |Text         |Yedekleme Yönetimi Sunucusu sürümü|
|BackupManagmentServerOSVersion_s     |Text            |Yedekleme yönetim sunucusunun işletim sistemi sürümü|
|BackupManagementServerType_s     |Text         |Yedekleme Yönetimi Sunucusu, MABS, SC DPM olarak türü|
|BackupManagmentServerUniqueId_s     |Text         |Yedekleme Yönetimi Sunucusu benzersiz olarak tanımlanabilmesi için alan       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

Bu tablo, bir birim ilişkilendirildiği workload(s) belirtir.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| StorageUniqueId_s |Text |Depolama varlık tanımlamak için kullanılan benzersiz kimliği |
| BackupItemType_s |Text |Bu birimi tercih edilen depolama alanı olan iş yükleri|

### <a name="protectedinstance"></a>ProtectedInstance

Bu tablo, ilgili alanları temel korumalı örnekler sağlar.

| Alan | Veri Türü |Geçerli sürümler | Açıklama |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Text |v2|DPM, MABS kullanan VM'ler için yedekleme öğesi tanımlamak için kullanılan benzersiz kimliği yedeklendi|
| ProtectedContainerUniqueId_s |Text |v2|DPM, MABS kullanarak Vm'leri dışında her şeyi için korumalı kapsayıcı tanımlamak için kullanılan benzersiz kimliği yedeklendi|
| ProtectedInstanceCount_s |Text |v2|Sayısı, korunan örnekler ilişkili yedekleme öğesi veya korumalı kapsayıcısını tarih-saat|

### <a name="recoverypoint"></a>RecoveryPoint

Bu tabloda temel kurtarma sağlar. ilgili alanları gelin.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| BackupItemUniqueId_s |Text |DPM, MABS kullanan VM'ler için yedekleme öğesi tanımlamak için kullanılan benzersiz kimliği yedeklendi|
| OldestRecoveryPointTime_s |Text |Yedekleme öğesi için en eski kurtarma noktası tarih saat|
| OldestRecoveryPointLocation_s |Text |Yedekleme öğesi için en eski kurtarma noktası konumu|
| LatestRecoveryPointTime_s |Text |Yedekleme öğesi için en son kurtarma noktasının tarih saat|
| LatestRecoveryPointLocation_s |Text |Yedekleme öğesi için en son kurtarma noktası konumu|

## <a name="next-steps"></a>Sonraki adımlar

Veri modeli gözden geçirin, sonra başlatabilirsiniz [özel sorgular oluşturma](../azure-monitor/learn/tutorial-logs-dashboards.md) kendi Pano oluşturmak üzere Azure İzleyici günlüklerine.
