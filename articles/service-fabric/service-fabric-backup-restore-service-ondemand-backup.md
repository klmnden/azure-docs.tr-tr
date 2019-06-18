---
title: Azure Service fabric'te isteğe bağlı yedekleme | Microsoft Docs
description: Yedekleme ve geri yükleme gereksinimi temelinde, uygulama verileri yedeklemek için Service fabric'te özellik.
services: service-fabric
documentationcenter: .net
author: aagup
manager: chackdan
editor: aagup
ms.assetid: 02DA262A-EEF6-4F90-842E-FFC4A09003E5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: bed3402de83984cae9134fe44058980ec18861b3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65413948"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Azure Service fabric'te isteğe bağlı yedekleme

Olağanüstü durum veya veri kaybı senaryolara güvenilir durum bilgisi olan hizmetler ve Reliable Actors verileri yedekleyebilirsiniz.

Azure Service Fabric için özelliklere sahiptir [düzenli yedekleme verilerinin](service-fabric-backuprestoreservice-quickstart-azurecluster.md) ve verilerinin gereksinim olarak. İsteğe bağlı yedekleme, karşı koruduğu için yararlıdır _veri kaybı_/_veri bozulması_ planlı değişiklikler temel alınan hizmete veya kendi ortamında nedeniyle.

İsteğe bağlı yedekleme özellikleri el ile bir hizmeti veya hizmeti ortam işlem tetiklemeden önce hizmetlerinin durumunu yakalamak için yararlıdır. Örneğin, bir değişiklik yaparsanız yükseltirken hizmeti ikili dosyalarını veya hizmet eski sürüme düşürme. Böyle bir durumda, uygulama kod hataları tarafından veri bozulmasına karşı koruma sağlamak isteğe bağlı Yedekleme yardımcı olabilir.
## <a name="prerequisites"></a>Önkoşullar

- Yapılandırma aramaların Microsoft.ServiceFabric.Powershell.Http modülü [Önizleme içinde] yükleyin.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

- Küme kullanarak bağlı olduğundan emin olun `Connect-SFCluster` Microsoft.ServiceFabric.Powershell.Http modülünü kullanarak herhangi bir yapılandırma isteğini yapmadan önce komutu.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```


## <a name="triggering-on-demand-backup"></a>İsteğe bağlı yedekleme tetikleniyor

İsteğe bağlı yedekleme, yedekleme dosyaları karşıya yükleme için depolama ayrıntıları gerektirir. Düzenli yedekleme ilkesini veya isteğe bağlı yedekleme isteği isteğe bağlı yedekleme konumunu belirtin.

### <a name="on-demand-backup-to-storage-specified-by-a-periodic-backup-policy"></a>İsteğe bağlı yedekleme düzenli bir yedekleme İlkesi tarafından belirtilen depolama

İsteğe bağlı ek yedekleme depolama alanı için bir güvenilir durum bilgisi olan hizmet veya Reliable Actor ilişkin bir bölüm kullanmak için düzenli yedekleme ilkesi yapılandırabilirsiniz.

Aşağıdaki senaryoda, devamlılık olduğu [güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). Bu durumda, bir bölüm kullanmak bir yedekleme İlkesi etkinleştirin ve Azure Depolama'da bir kümesi sıklıkta yedekleme gerçekleşir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' 

```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

Kullanım [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) bölüm kimliği için talep üzerine yedekleme için tetikleme yukarı ayarlamak için API `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Kullanım [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) için izlemeyi etkinleştirmek için API [isteğe bağlı yedekleme ilerleme](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress).

### <a name="on-demand-backup-to-specified-storage"></a>İsteğe bağlı yedekleme için belirtilen depolama

Güvenilir durum bilgisi olan hizmet veya Reliable Actor ilişkin bir bölüm için isteğe bağlı yedekleme isteyebilir. İsteğe bağlı yedekleme isteği bir parçası olarak depolama bilgileri sağlayın.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' -AzureBlobStore -ConnectionString  'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

Kullanım [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) bölüm kimliği için talep üzerine yedekleme için tetikleme yukarı ayarlamak için API `974bd92a-b395-4631-8a7f-53bd4ae9cf22`. Aşağıdaki Azure Depolama bilgileri ekleyin:

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

Kullanabileceğiniz [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) için izlemeyi ayarlamak için API [isteğe bağlı yedekleme ilerleme](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress).

## <a name="tracking-on-demand-backup-progress"></a>İsteğe bağlı yedekleme ilerlemesini izleme

Güvenilir durum bilgisi olan hizmet veya Reliable Actor ilişkin bir bölüm aynı anda yalnızca bir talep üzerine yedekleme isteği kabul eder. Yalnızca geçerli isteğe bağlı yedekleme istek tamamlandıktan sonra başka bir istek kabul edilebilir.

Farklı bölümleri aynı anda isteğe bağlı yedekleme istekleri tetikleyebilirsiniz.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell

Get-SFPartitionBackupProgress -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22'

```
#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

İsteğe bağlı yedekleme isteği şu durumlarda olabilir:

- **Kabul edilen**: Yedekleme bölüme başlatıldı ve devam ediyor.
  ```
  BackupState             : Accepted
  TimeStampUtc            : 0001-01-01T00:00:00Z
  BackupId                : 00000000-0000-0000-0000-000000000000
  BackupLocation          :
  EpochOfLastBackupRecord :
  LsnOfLastBackupRecord   : 0
  FailureError            :
  ```
- **Başarı**, **hatası**, veya **zaman aşımı**: İstenen bir isteğe bağlı yedekleme aşağıdaki durumlardan birinde tamamlayabilirsiniz:
  - **Başarı**: A _başarı_ yedekleme durum, bölüm durumu başarıyla yedeklendi olduğunu gösterir. İsteğin yanıtını verir _BackupEpoch_ ve _BackupLSN_ saat (UTC) ile birlikte bölümü için.
    ```
    BackupState             : Success
    TimeStampUtc            : 2018-11-21T20:00:01Z
    BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
    BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
    EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
    LsnOfLastBackupRecord   : 36
    FailureError            :
    ```
  - **Hata**: A _hatası_ yedekleme durumunu belirten bir hata bölümün durumunun bir yedeğini sırasında oluştu. Hatanın nedeni, yanıt olarak belirtilir.
    ```
    BackupState             : Failure
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
    ```
  - **Zaman aşımı**: A _zaman aşımı_ yedekleme durumunu belirten bölüm durumu yedekleme belirli bir süre içinde oluşturulamadı. Varsayılan zaman aşımı değeri 10 dakikadır. İsteğe bağlı yeni bir yedekleme isteği ile başlatın büyük [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) Bu senaryoda.
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
- [BackupRestore REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
