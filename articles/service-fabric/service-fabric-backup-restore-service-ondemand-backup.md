---
title: Azure Service fabric'te isteğe bağlı yedekleme | Microsoft Docs
description: Service Fabric'in yedekleme ve geri yükleme özelliği uygulama verilerinizi gereksinim olarak yedeklenmesi için.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 02DA262A-EEF6-4F90-842E-FFC4A09003E5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 35d97f1da6b5d1c75073264c70e1cd1607d5fe0d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730277"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Azure Service fabric'te isteğe bağlı yedekleme

Verileri güvenilir durum bilgisi olan hizmetler ve Reliable Actors, olağanüstü durum veya veri kaybı senaryolara yedeklenebilir.

Service Fabric için özelliklerle donatılmış [düzenli yedekleme verilerinin](service-fabric-backuprestoreservice-quickstart-azurecluster.md) ve gereksinim temelinde veri yedekleme. Karşı korur gibi isteğe bağlı yedekleme kullanışlıdır _veri kaybı_/_veri bozulması_ planlı değişiklikler temel alınan hizmete veya kendi ortamında nedeniyle.

İsteğe bağlı yedekleme özellikleri Hizmetleri, hizmet veya hizmet ortamı ile ilgili herhangi bir el ile tetiklenen işleminden önce durumu yakalanırken yararlı olur. Hizmet ikili dosyaları, yani değiştirme, yükseltme veya hizmeti, eski sürüme düşürme gibi; olarak, uygulamanın koddaki hatalar tarafından veri bozulmasına karşı korumalı veri sahip olur.

## <a name="triggering-on-demand-backup"></a>İsteğe bağlı yedekleme tetikleniyor

İsteğe bağlı yedekleme, yedekleme dosyaları karşıya yükleme için depolama ayrıntıları gerektirir. İsteğe bağlı yedekleme düzenli yedekleme İlkesi veya belirtilen depolama isteğe bağlı yedekleme isteği içinde belirtilen depolama alanında gerçekleşir.

### <a name="on-demand-backup-to-storage-specified-by-periodic-backup-policy"></a>İsteğe bağlı yedekleme düzenli yedekleme İlkesi tarafından belirtilen depolama

Güvenilir durum bilgisi olan hizmet veya Reliable Actor bölümünü düzenli yedekleme ilkesinde belirtilen depolama gereksinimi temel yedekleme hakkında ek için istenebilir. 

Aşağıdaki örnek, devamlılık belirtildiği gibi durumda [güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors)burada bölümü etkin bir yedekleme ilkesi vardır ve istenen bir sıklıkta yedekleme sürüyor Azure depolama alanında.

Bölüm kimliği için talep üzerine yedekleme `974bd92a-b395-4631-8a7f-53bd4ae9cf22` [BackupPartition] tarafından tetiklenebilir (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API. 

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

[İsteğe bağlı yedekleme ilerleme](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress) tarafından izlenen [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API.

### <a name="on-demand-backup-to-specified-storage"></a>İsteğe bağlı yedekleme için belirtilen depolama

Depolama bilgileriyle birlikte bir güvenilir durum bilgisi olan hizmet veya Reliable Actor ilişkin bir bölüm için isteğe bağlı yedekleme istenebilir. İsteğe bağlı yedekleme isteğin bir parçası olarak depolama bilgileri sağlanmalıdır.

Bölüm kimliği için talep üzerine yedekleme `974bd92a-b395-4631-8a7f-53bd4ae9cf22` [BackupPartition] tarafından tetiklenebilir (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) aşağıda gösterildiği gibi Azure depolama bilgilerini API'SİYLE.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

[İsteğe bağlı yedekleme ilerleme](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress) tarafından izlenen [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API.


## <a name="tracking-on-demand-backup-progress"></a>İsteğe bağlı yedekleme ilerlemesini izleme

Güvenilir durum bilgisi olan hizmet veya Reliable Actor ilişkin bir bölüm aynı anda yalnızca bir talep üzerine yedekleme isteği kabul eder. Yalnızca geçerli isteğe bağlı yedekleme isteği tamamlandığında, başka bir istek kabul edilebilir. 

Birden fazla talep üzerine yedekleme isteği aynı anda farklı bölümlerde tetiklenebilir.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

İsteğe bağlı yedekleme isteğin ilerleme aşağıdaki durumlardan biri olabilir.

* **Kabul edilen** -yedekleme bölüme başlatıldı ve devam ediyor.
    ```
    BackupState             : Accepted
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          : 
    EpochOfLastBackupRecord : 
    LsnOfLastBackupRecord   : 0
    FailureError            : 
    ```
    
* **Başarı / hata / zaman aşımı** -isteğe bağlı yedekleme aşağıdaki durumlardan birinde tamamlanabileceğini istendi. Her durum, aşağıdaki önemi ve yanıt ayrıntıları sahiptir.

    * **Başarı** -yedekleme durumu olarak _başarı_ bölüm durumu başarıyla yedeklenir gösterir. Yanıt sağlayacak __BackupEpoch__ ve __BackupLSN__ saat (UTC) ile birlikte bölümü için.
        ```
        BackupState             : Success
        TimeStampUtc            : 2018-11-21T20:00:01Z
        BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
        BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
        EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
        LsnOfLastBackupRecord   : 36
        FailureError            : 
        ```
        
    * **Hata** -yedekleme durumu olarak _hatası_ bölümün durumunun bir yedeğini sırasında hata oluştu gösterir. Hatanın nedenini yanıt olarak belirtilecektir.
        ```
        BackupState             : Failure
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
        ```
       
    * **Zaman aşımı** -yedekleme durumu olarak _zaman aşımı_ gösteren bölüm Durumu yedeklemesini de belirli bir zaman çerçevesi oluşturulamadı; varsayılan zaman aşımı değeri 10 dakika olan. Yeni bir yedekleme isteği başlatma büyük [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) isteğe bağlı yedekleme istekte bu senaryoda önerilir.

        ```
        BackupState             : Timeout
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
        ```

## <a name="next-steps"></a>Sonraki adımlar
- [Düzenli yedekleme yapılandırması anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

