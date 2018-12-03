---
title: Azure Service fabric'te yedeğini geri yükleme | Microsoft Docs
description: Service Fabric'in düzenli yedekleme ve geri yükleme özelliği, veri, uygulama verilerinizi yedekten geri yükleme.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 802F55B6-6575-4AE1-8A8E-C9B03512FF88
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 69604decab354368f336b85bfa1497671f0c3101
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730369"
---
#  <a name="restoring-backup-in-azure-service-fabric"></a>Azure Service fabric'te yedeğini geri yükleme


Güvenilir durum bilgisi olan hizmetler ve Service fabric'te Reliable Actors istek ve yanıt ya da bir işlemin ötesinde değişebilir, yetkili durumu koruyabilir. Durum bilgisi olan hizmet uzun bir süredir devre dışı kalması veya bir olağanüstü durum nedeniyle bilgileri kaybeder, son durumunun kabul edilebilir yedekleme için geri çağrıldıktan sonra hizmet sağlamaya devam etmek için geri yüklenmesi gerekebilir.

Örneğin, hizmet verilerini aşağıdaki senaryolardan birini korumak için yedekleme isteyebilirsiniz:

- Tüm Service Fabric kümesinin kalıcı kaybı durumunda. **(Olağanüstü durum kurtarma - DR durumu)**
- Hizmet bölüm çoğaltmalarını çoğunu kalıcı kaybı. **(Veri kaybı büyük)**
- Yönetim hataları yapabildiği durumu yanlışlıkla silinirse veya bozulursa. Örneğin, yeterli ayrıcalığa sahip bir yönetici, hizmet yanlışlıkla siler. **(Veri kaybı büyük)**
- Veri bozulması neden hataları hizmetinde. Örneğin, bozuk verileri güvenilir bir koleksiyona yazma hizmeti kod yükseltmesi başladığında veri bozulması oluşabilir. Böyle bir durumda, bir önceki durumuna geri döndürülmesi hem kod hem de veri olabilir. **(Veri bozulması durumunun)**


## <a name="prerequisites"></a>Önkoşullar
* Tetiklemek için geri yükleme _hata analizi hizmeti (FAS)_ küme için etkinleştirilmelidir
* Geri yükleme yedeklemesinin tarafından alınmış _yedekleme geri yükleme hizmeti (BRS)_
* Geri yükleme, yalnızca bir bölümünde tetiklenebilir.

## <a name="triggering-restore"></a>Geri yüklemeyi tetikleme

Geri yükleme aşağıdaki senaryoları biri için olabilir 
* Verileri geri yükleme durumunda _olağanüstü durum kurtarma_ (DR)
* Verileri geri yükleme durumunda _veri bozulması / veri kaybı_



### <a name="data-restore-in-the-event-of-disaster-recovery-dr"></a>Verileri geri yükleme durumunda _olağanüstü durum kurtarma_ (DR)
Kaybolmasını bir tüm Service Fabric kümesi olması durumunda veri bölümlerin Reliable Actors ve güvenilir durum bilgisi olan hizmet için alternatif bir kümeye geri yüklenebilir. İstenen yedekleme numaralandırmasını seçilebilir [yedekleme depolama ayrıntılarla GetBackupAPI](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation). Yedekleme sabit bir uygulama, hizmet veya bölüm olabilir.

Kayıp küme, belirtilen küme olduğu varsayılmıştır sağlar [güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), hangi vardı `SampleApp` dağıtılan, burada bölüm olan sahip etkin bir yedekleme İlkesi ve yedeklemeler, Azure Depolama'da gerçekleştirilecek. 


Tüm bölümleri için oluşturulan yedekleri numaralandırmak için REST API'yi çağırmak için PowerShell Betiği aşağıdaki yürütme `SampleApp` kayıp Service Fabric kümesindeki uygulama. Sabit listesi API'si kullanılabilir yedekler numaralandırmak için bir uygulama yedeklerini depolandığı, depolama bilgileri gerektirir. 

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$BackupEntity = @{
    EntityKind = 'Application'
    ApplicationName='fabric:/SampleApp'
}

$BackupLocationAndEntityInfo = @{
    Storage = $StorageInfo
    BackupEntity = $BackupEntity
}

$body = (ConvertTo-Json $BackupLocationAndEntityInfo)
$url = "https://myalternatesfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Yukarıdaki örnek çıktısı çalıştırın:

```
BackupId                : b9577400-1131-4f88-b309-2bb1e943322c
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 20.55.16.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3334
CreationTimeUtc         : 2018-04-06T20:55:16Z
FailureError            : 
*
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
*
BackupId                : 69436834-c810-4163-9386-a7a800f78359
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.25.36.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3764
CreationTimeUtc         : 2018-04-06T21:25:36Z
FailureError            : 
```



Geri yüklemeyi tetikleme için istenen yedekleme seçmek ihtiyacımız var. İstenen yedekleme için geçerli olağanüstü durum kurtarma (DR) aşağıdaki yedekleme sağlar.

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Sağlamak için ihtiyacımız geri yükleme API'si için __Backupıd__ ve __BackupLocation__ ayrıntıları. Alternatif kümesinde bölüm başına olarak seçilen gerekiyor [bölüm düzeni](service-fabric-concepts-partitioning.md#get-started-with-partitioning). Seçmek için uygun bölümleme şeması özgün kayıp kümedeki diğer kümeden yedeklemeyi geri yüklemek için hedef bölüm kullanıcının sorumluluğundadır.

Alternatif kümesinde bölüm kimliği olduğunu varsayın `1c42c47f-439e-4e09-98b9-88b8f60800c6`, özgün küme bölüm Kimliğine eşleyen `974bd92a-b395-4631-8a7f-53bd4ae9cf22` yüksek anahtar ve düşük anahtarı karşılaştırarak _aralıklı (UniformInt64Partition) bölümleme_.

İçin _adlı bölümleme_, hedef bölüm alternatif kümesinde belirlemek için ad değeri karşılaştırılır.

Yedekleme kümesinin bir bölüm karşı aşağıdaki geri yükleme istenen [API geri yükleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)

```powershell 
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 
Geri Yükleme ilerlemesini olabilir [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress)

### <a name="data-restore-in-the-event-of-data-corruption--data-loss"></a>Verileri geri yükleme durumunda _veri bozulması / veri kaybı_

Durumu için _veri kaybı_ veya _veri bozulması_ bölümlerin Reliable Actors ve güvenilir durum bilgisi olan hizmet için veri seçilen yedeklemeleri birine geri yüklenebilir. Aşağıdaki örnek, devamlılık belirtildiği gibi durumda [güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors)burada bölümü etkin bir yedekleme ilkesi vardır ve istenen bir sıklıkta yedekleme sürüyor bir Azure depolama alanında. 

İstenen yedekleme çıktısından seçili [GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups). Bu senaryoda, geçmişte aynı küme yedekleme oluşturulur.
Geri yüklemeyi tetikleme için istenen yedekleme listeden seçmek ihtiyacımız var. İstenen şablonumuzu, yedeği geçerli izin _veri kaybı_ / _veri bozulması_ aşağıdaki yedekleme

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Sağlamak için ihtiyacımız geri yükleme API'si için __Backupıd__ ve __BackupLocation__ ayrıntıları. Service Fabric kümesi bu yana yedekleme etkin olduğundan _yedekleme geri yükleme hizmeti (BRS)_ ilişkili yedekleme İlkesi doğru depolama konumundan tanımlar.

```powershell
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4', 
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Geri Yükleme ilerlemesini olabilir [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress)


## <a name="tracking-restore-progress"></a>Geri yükleme ilerlemeyi izleme

Güvenilir durum bilgisi olan hizmet veya Reliable Actor ilişkin bir bölüm aynı anda yalnızca bir geri yükleme isteği kabul eder. Yalnızca geçerli geri yükleme isteği tamamlandığında, başka bir istek kabul edilebilir. Birden çok geri yükleme isteği aynı anda farklı bölümlerde tetiklenebilir.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
 
$restoreResponse = (ConvertFrom-Json $response.Content) 
$restoreResponse | Format-List
```

aşağıdaki sırada geri yükleme isteği ilerlemesi

1. __Kabul edilen__ -geri yükleme durumu olarak _kabul edilen_ istenen doğru isteği parametreleriyle tetiklendiğini gösterir.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
2. __Inprogress__ -geri yükleme durumu olarak _Inprogress_ bölüm geri istekte belirtilen yedekleme yapılacaktır gösterir. Bölüm bildirir _dataloss_ durumu.
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. __Başarı__/ __hatası__/ __zaman aşımı__ -istenen bir geri yükleme şu durumlardan birinde tamamlanabilir. Her durum, aşağıdaki önemi ve yanıt ayrıntıları sahiptir.
       
    * __Başarı__ -geri yükleme durumu olarak _başarı_ bölüm durumunu yeniden elde edildi gösterir. Yanıt RestoreEpoch ve RestordLSN bölümün saat (UTC) ile birlikte sağlar. 
    
        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```
        
    *. __Hata__ -geri yükleme durumu olarak _hatası_ geri yükleme isteği başarısız gösterir. Hatanın nedeni, istekte belirtilecektir.
        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    *. __Zaman aşımı__ -geri yükleme durumu olarak _zaman aşımı_ istek zaman aşımı olduğunu gösterir. Yeni geri yükleme isteği ile büyük [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) önerilir; varsayılan olarak zaman aşımı olan 10 dakika. Bölüm geri yüklemeyi yeniden istemeden önce veri kaybı durumu dışında olduğundan emin olmak için önerilir.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="auto-restore"></a>Otomatik geri yükleme

Reliable Actors ve güvenilir durum bilgisi olan hizmet için Service Fabric kümesinde bölümleri için yapılandırılabilir _otomatik olarak geri yükleme_. Yedekleme ilkesi oluştururken, ilkeyi kullandığını `AutoRestore` kümesine _true_.  Etkinleştirme _otomatik olarak geri yükleme_ veri kaybı bildirilirse bir bölümü için verileri en son yedekten geri yükler.
 
 [Yedekleme İlkesi'nde otomatik geri yükleme etkinleştirme](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)


[RestorePartition API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
[GetPartitionRestoreProgress API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Sonraki adımlar
- [Düzenli yedekleme yapılandırması anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
