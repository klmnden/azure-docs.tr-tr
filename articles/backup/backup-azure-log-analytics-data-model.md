---
title: Azure Backup için Log Analytics veri modeli
description: Bu makalede, Azure yedekleme verileri için günlük analizi veri modeli ayrıntılarına hakkında alınmaktadır.
services: backup
documentationcenter: ''
author: JPallavi
manager: vijayts
editor: ''
ms.assetid: dfd5c73d-0d34-4d48-959e-1936986f9fc0
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d55ec8ac4416fe0a082812584552462292b6dbb7
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Azure yedekleme verileri için günlük analizi veri modeli
Bu makale için günlük analizi raporlama verilerini gönderilmesi için kullanılan veri modelini açıklar. Bu veri modelini kullanarak özel sorgular, panolar, oluşturma ve günlük analizi kullanma. 

## <a name="using-azure-backup-data-model"></a>Azure Backup veri modelini kullanma
Veri modelinin bir parçası olarak sağlanan aşağıdaki alanları görselleri, özel sorgular ve Pano gereksinimlerinize uygun şekilde oluşturmak için kullanabilirsiniz.

### <a name="alert"></a>Uyarı
Bu tablo uyarı ilgili alanları hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| AlertUniqueId_s |Metin |Oluşturulan uyarının benzersiz kimliği |
| AlertType_s |Metin |Oluşturulan uyarı türü, örneğin, yedekleme |
| AlertStatus_s |Metin |Uyarı, örneğin, etkin durumu |
| AlertOccurenceDateTime_s |Tarih/Saat |Tarih ve uyarının oluşturulduğu saat |
| AlertSeverity_s |Metin |Uyarının önem derecesini, örneğin, kritik |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| BackupItemUniqueId_s |Metin |Bu uyarı ait yedekleme öğesinin benzersiz kimliği |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen uyarı nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu uyarı ait yedekleme, örneğin, IaaSVM FileFolder gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - uyarı adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| ProtectedServerUniqueId_s |Metin |Bu uyarı ait benzersiz kimliğini korumalı |
| VaultUniqueId_s |Metin |Bu uyarı ait benzersiz kimliğini korumalı |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="backupitem"></a>BackupItem
Bu tablo yedekleme öğesi ilişkili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |  
| BackupItemUniqueId_s |Metin |Yedekleme öğesinin benzersiz kimliği |
| BackupItemId_s |Metin |Yedekleme öğesinin kimliği |
| BackupItemName_s |Metin |Yedekleme öğesinin adı |
| BackupItemFriendlyName_s |Metin |Yedekleme öğesinin kolay adı |
| BackupItemType_s |Metin |Yedekleme öğesi, örneğin, VM, FileFolder türü |
| ProtectedServerName_s |Metin |Hangi yedekleme öğesi için korumalı sunucunun adını ait |
| ProtectionState_s |Metin |Yedekleme öğesinin, örneğin, korumalı, ProtectionStopped geçerli koruma durumu |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen yedekleme öğesi nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu yedekleme öğesi ait yedekleme, örneğin, IaaSVM FileFolder gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - BackupItem adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="backupitemassociation"></a>BackupItemAssociation
Bu tabloda, çeşitli varlıkları yedekleme öğesi ilişkilerini hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |  
| BackupItemUniqueId_s |Metin |Yedekleme öğesinin benzersiz kimliği |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen yedekleme öğesi ilişkilendirme nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu yedekleme öğesi ait yedekleme, örneğin, IaaSVM FileFolder gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - BackupItemAssociation adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| PolicyUniqueId_g |Metin |Hangi yedekleme öğesi için ilişkili ilke tanımlamak için benzersiz kimlik |
| ProtectedServerUniqueId_s |Metin |Bu yedekleme öğesi ait olduğu korumalı sunucunun benzersiz kimliği |
| VaultUniqueId_s |Metin |Bu yedekleme öğesi ait olduğu kasasının benzersiz kimliği |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="job"></a>İş
Bu tabloyu işle ilgili alanları hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| BackupItemUniqueId_s |Metin |Bu iş ait yedekleme öğesinin benzersiz kimliği |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen iş nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu iş ait yedekleme, örneğin, IaaSVM FileFolder gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - adını işi temsil eder. |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| ProtectedServerUniqueId_s |Metin |Bu iş ait benzersiz kimliğini korumalı |
| VaultUniqueId_s |Metin |Bu iş ait benzersiz kimliğini korumalı |
| JobOperation_s |Metin |İçin proje Örneğin, yedekleme, geri yükleme, yapılandırma yedekleme çalıştığı işlemi |
| JobStatus_s |Metin |İşin durumu tamamlandı, örneğin, tamamlandı, başarısız oldu |
| JobFailureCode_s |Metin |Hata kodu dize hangi nedeniyle iş başarısız oldu |
| JobStartDateTime_s |Tarih/Saat |Tarih ve saat başlatılan çalışan iş yapılırken |
| BackupStorageDestination_s |Metin |Yedekleme depolama, örneğin, bulut, Disk hedefi  |
| JobDurationInSecs_s | Sayı |Saniye cinsinden toplam iş yürütme süresi |
| DataTransferredInMB_s | Sayı |Bu proje için MB cinsinden aktarılan veriler|
| JobUniqueId_g |Metin |İşi belirlemek için benzersiz kimliği |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="policy"></a>İlke
Bu tablo ilkesiyle ilgili alanları hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu ilke ait yedekleme, örneğin, IaaSVM FileFolder gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - ilke adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| PolicyUniqueId_g |Metin |İlke tanımlamak için benzersiz kimlik |
| PolicyName_s |Metin |Tanımlanan ilke adı |
| BackupFrequency_s |Metin |Hangi yedeklemeler çalıştırılır, örneğin, günlük, haftalık sıklığı |
| BackupTimes_s |Metin |Yedeklemelerin ne zaman zamanlanmış tarih ve saat |
| BackupDaysOfTheWeek_s |Metin |Yedeklemelerin ne zaman zamanlandı haftanın günleri |
| RetentionDuration_s |Tam sayı |Yapılandırılmış yedeklemeler için elde tutma süresi |
| DailyRetentionDuration_s |Tam sayı |Yapılandırılan yedekleme için gün cinsinden toplam tutma süresi |
| DailyRetentionTimes_s |Metin |Tarih ve saat günlük bekletme zaman yapılandırıldı |
| WeeklyRetentionDuration_s |Ondalık sayı |Yapılandırılmış yedeklemeler için hafta cinsinden toplam haftalık tutma süresi |
| WeeklyRetentionTimes_s |Metin |Tarih ve saat haftalık bekletme zaman yapılandırılır |
| WeeklyRetentionDaysOfTheWeek_s |Metin |Haftalık bekletme için haftanın günlerini seçili |
| MonthlyRetentionDuration_s |Ondalık sayı |Yapılandırılmış yedeklemeler için ay cinsinden toplam tutma süresi |
| MonthlyRetentionTimes_s |Metin |Tarih ve saat aylık bekletme zaman yapılandırılır |
| MonthlyRetentionFormat_s |Metin |Aylık bekletme, örneğin, günlük tabanlı, haftalık dayalı haftanın günü için yapılandırma türü |
| MonthlyRetentionDaysOfTheWeek_s |Metin |Aylık bekletme için haftanın günlerini seçili |
| MonthlyRetentionWeeksOfTheMonth_s |Metin |Örneğin, ilk, son vb. hafta aylık bekletme yapılandırıldığında, ayın. |
| YearlyRetentionDuration_s |Ondalık sayı |Yapılandırılmış yedeklemeleri yıldır cinsinden toplam tutma süresi |
| YearlyRetentionTimes_s |Metin |Tarih ve saat yıllık bekletme zaman yapılandırılır |
| YearlyRetentionMonthsOfTheYear_s |Metin |Yıllık bekletme için yılın ay seçili |
| YearlyRetentionFormat_s |Metin |Tür yıllık bekletme, örneğin, günlük tabanlı, haftalık dayalı haftanın günü için yapılandırma |
| YearlyRetentionDaysOfTheMonth_s |Metin |Yıllık bekletme için seçili ayın tarihleri |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="policyassociation"></a>PolicyAssociation
Bu tablo ilke ilişkilendirmelerini çeşitli varlıkları ile ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen ilke nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Örneğin, IaaSVM, bu ilkenin ait olduğu FileFolder yedeklemenin için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - PolicyAssociation adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| PolicyUniqueId_g |Metin |İlke tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Metin |Bu ilke ait olduğu kasasının benzersiz kimliği |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="protectedserver"></a>ProtectedServer
Bu tablo korumalı sunucu ilgili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| ProtectedServerName_s |Metin |Korumalı sunucunun adı |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen korumalı sunucu nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Örneğin, IaaSVM, bu korumalı sunucunun ait olduğu FileFolder yedeklemenin için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - ProtectedServer adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| ProtectedServerUniqueId_s |Metin |Korumalı sunucunun benzersiz kimliği |
| RegisteredContainerId_s |Metin |Yedekleme için kayıtlı kapsayıcı kimliği |
| ProtectedServerType_s |Metin |Örneğin, Windows Kurulumu türü korumalı sunucunun yedeği |
| ProtectedServerFriendlyName_s |Metin |Korumalı sunucusunun kolay adı |
| AzureBackupAgentVersion_s |Metin |Aracı yedekleme sürümünün sürüm numarası |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Bu tablo korumalı sunucu ilişkilendirmeleri diğer varlıklarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen korumalı sunucu ilişkilendirme nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu korumalı sunucunun ait olduğu yedekleme, örneğin, IaaSVM FileFolder gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - ProtectedServerAssociation adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| ProtectedServerUniqueId_s |Metin |Korumalı sunucunun benzersiz kimliği |
| VaultUniqueId_s |Metin |Bu korumalı sunucunun ait olduğu kasasının benzersiz kimliği |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

### <a name="storage"></a>Depolama
Bu tablo depolama ilişkili alanlar hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| CloudStorageInBytes_s |Ondalık sayı |Bulut yedekleme depolama hesaplanan yedekleri tarafından kullanılan son değere göre |
| ProtectedInstances_s |Ondalık sayı |Ön uç depolama faturalama, hesaplanan dayalı olarak en son değeri hesaplamak için kullanılan korumalı örneği sayısı |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen depolama nesnenin geçerli durumu |
| BackupManagementType_s |Metin |Bu depolama ait yedekleme, örneğin, IaaSVM FileFolder gerçekleştirmek için sağlayıcı türü |
| OperationName |Metin |Bu alan geçerli işlem - depolama adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| ProtectedServerUniqueId_s |Metin |Depolama hesaplandığı korumalı sunucunun benzersiz kimliği |
| VaultUniqueId_s |Metin |Depolama kasası benzersiz kimliğini hesaplanır |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan representse kendisi için veri toplanırken - kaynağın kasalarını yazın |

### <a name="vault"></a>Kasa
Bu tablo, kasa ile ilgili alanları hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olay adını temsil eder, her zaman AzureBackupCentralReport. |
| SchemaVersion_s |Metin |Bu alan geçerli şema sürümünü gösterir, **V1** |
| State_s |Metin |Örneğin, etkin, silinen kasası nesnenin geçerli durumu |
| OperationName |Metin |Bu alan geçerli işlem - kasa adını temsil eder |
| Kategori |Metin |Bu alan için günlük analizi gönderilen tanılama verilerini kategorisini temsil eder, AzureBackupReport. |
| Kaynak |Metin |Bu veri toplanmakta kaynak, Kurtarma Hizmetleri kasa adı gösterir |
| VaultUniqueId_s |Metin |Kasa benzersiz kimliği |
| VaultName_s |Metin |Kasa adı |
| AzureDataCenter_s |Metin |Kasa bulunduğu veri merkezi |
| StorageReplicationType_s |Metin |Örneğin, GeoRedundant kasa için depolama çoğaltma türü |
| SourceSystem |Metin |Kaynak sistemi geçerli verilerin - Azure |
| ResourceId |Metin |İsteğe bağlı olarak veri toplanmakta Bu alan temsil kaynak kimliği kurtarma Hizmetleri kasası kaynak kimliği gösterir |
| SubscriptionId |Metin |Bu alan veri toplanmakta kaynak (RS kasa) abonelik kimliğini temsil eder |
| ResourceGroup |Metin |Bu alan için veri toplanırken kaynak (RS kasa) kaynak grubunu temsil eder |
| ResourceProvider |Metin |Bu alan için veri toplanırken - kaynak sağlayıcısı Microsoft.RecoveryServices temsil eder |
| ResourceType |Metin |Bu alan için veri toplanırken - kaynak kasalarını türünü temsil eder |

## <a name="next-steps"></a>Sonraki adımlar
Azure Backup raporları oluşturmak için veri modeli gözden sonra başlatabilirsiniz [Pano oluşturma](../log-analytics/log-analytics-dashboards.md) günlük analizi içinde.
