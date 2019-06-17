---
title: Azure Service fabric'te yedeğini geri yükleme | Microsoft Docs
description: Düzenli yedekleme ve Service Fabric özelliği, veri, uygulama verileri bir yedekten geri yüklemek için geri yükleme.
services: service-fabric
documentationcenter: .net
author: aagup
manager: chackdan
editor: aagup
ms.assetid: 802F55B6-6575-4AE1-8A8E-C9B03512FF88
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: e4ada412547360f97e869d3312b65d869fa3df48
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65413727"
---
# <a name="restoring-backup-in-azure-service-fabric"></a>Azure Service fabric'te yedeğini geri yükleme

Bir istek ve yanıt işlem tamamlandıktan sonra değişebilir, yetkili bir durum, Azure Service Fabric güvenilir durum bilgisi olan hizmetler ve Reliable Actors koruyabilirsiniz. Durum bilgisi olan hizmet uzun bir süredir aşağı git veya bir olağanüstü durum nedeniyle bilgileri kaybedersiniz. Bu durumda, hizmet çalışmaya devam edebilirsiniz, en son kabul edilebilir yedekten geri yüklenmesi gerekir.

Örneğin, bir hizmet aşağıdaki senaryolarda karşı korumak için verileri yedeklemek için yapılandırabilirsiniz:

- **Olağanüstü durum kurtarma durumunda**: Tüm Service Fabric kümesinin kalıcı kaybı.
- **Veri kaybı çalışması**: Hizmet bölüm çoğaltmalarını çoğunu kalıcı kaybı.
- **Veri kaybı çalışması**: Kazayla silinme veya bozulmaya hizmeti. Örneğin, bir yönetici, hizmet yanlışlıkla siler.
- **Veri bozulması durumunda**: Hizmet hataları, veri bozulmasına neden. Örneğin, bir hizmet kodu yükseltme hatalı verileri güvenilir bir koleksiyona yazdığında veri bozulması oluşabilir. Böyle bir durumda, veri ve kodu bir önceki durumuna geri yüklemek gerekebilir.

## <a name="prerequisites"></a>Önkoşullar

- Bir geri yüklemeyi tetikleyecek _hata analizi hizmeti (FAS)_ küme için etkinleştirilmesi gerekir.
- _Yedekleme geri yükleme hizmeti (BRS)_ yedekleme oluşturulur.
- Geri yükleme, yalnızca bir bölümünde tetiklenebilir.
- Yapılandırma aramaların Microsoft.ServiceFabric.Powershell.Http modülü [Önizleme içinde] yükleyin.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

- Küme kullanarak bağlı olduğundan emin olun `Connect-SFCluster` Microsoft.ServiceFabric.Powershell.Http modülünü kullanarak herhangi bir yapılandırma isteğini yapmadan önce komutu.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```


## <a name="triggered-restore"></a>Tetiklenen geri yükleme

Bir geri yükleme aşağıdaki senaryolar için tetiklenebilir:

- Verileri geri yüklemek için _olağanüstü durum kurtarma_.
- Verileri geri yüklemek için _veri bozulması/veri kaybı_.

### <a name="data-restore-in-the-case-of-disaster-recovery"></a>Olağanüstü durum kurtarma durumunda veri geri yükleme

Tüm bir Service Fabric kümesi kaybolursa, Reliable Actors ve güvenilir durum bilgisi olan hizmet bölümlerini verilerini kurtarabilirsiniz. İstenen yedekleme kullandığınızda listeden seçilebilir [yedekleme depolama ayrıntılarla GetBackupAPI](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation). Yedekleme sabit bir uygulama, hizmet veya bölüm olabilir.

Aşağıdaki örneğin kayıp küme için belirtilen kümenin olduğunu varsayın [güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). Bu durumda, `SampleApp` etkin, yedekleme ilkesiyle dağıtılır ve Azure depolama için yedeklemeleri yapılandırılır.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell
Get-SFBackupsFromBackupLocation -Application -ApplicationName 'fabric:/SampleApp' -AzureBlobStore -ConnectionString 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

Tüm bölümleri için oluşturulan yedekleri listesini döndürmek için REST API'yi kullanmak için bir PowerShell betiğini yürütün `SampleApp` uygulama. API, kullanılabilir yedekler listelemek için yedekleme depolama bilgileri gerektirir.

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

Geri yüklemeyi tetikleyecek için yedeklemeleri birini seçin. Örneğin, olağanüstü durum kurtarma için geçerli yedekleme aşağıdaki yedekleme biri olabilir:

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

Geri yükleme API'si için sağlamanız gereken _Backupıd_ ve _BackupLocation_ ayrıntıları.

Ayrıca, ayrıntılı olarak alternatif kümedeki hedef bölüm seçmeniz gerekebilir [bölüm düzeni](service-fabric-concepts-partitioning.md#get-started-with-partitioning). Diğer küme yedeği özgün kayıp kümeden bölüm düzeni belirtildiği bölüme geri yüklenir.

Bölüm kimliği alternatif kümede ise `1c42c47f-439e-4e09-98b9-88b8f60800c6`, özgün küme bölüm kimliği için harita `974bd92a-b395-4631-8a7f-53bd4ae9cf22` yüksek anahtar ve düşük anahtarı karşılaştırarak _aralıklı (UniformInt64Partition) bölümleme_.

İçin _adlı bölümleme_, hedef bölüm alternatif kümesinde belirlemek için ad değeri karşılaştırılır.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell

Restore-SFPartition  -PartitionId '1c42c47f-439e-4e09-98b9-88b8f60800c6' -BackupId 'b0035075-b327-41a5-a58f-3ea94b68faa4' -BackupLocation 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' -AzureBlobStore -ConnectionString 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

Aşağıdakileri kullanarak yedekleme kümesi bölüm karşı geri yükleme isteği [geri API](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition):

```powershell

$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
    BackupStorage  = $StorageInfo
}

$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

TrackRestoreProgress ile bir geri yükleme ilerlemesini izleyebilirsiniz.

### <a name="data-restore-for-data-corruptiondata-loss"></a>Verileri geri yüklemek için _veri bozulması_/_veri kaybı_

İçin _veri kaybı_ veya _veri bozulması_, güvenilir durum bilgisi olan hizmet için yedeklenen bölümleri ve Reliable Actors bölümleri geri yükleyebilirsiniz herhangi seçilen yedeklemeler.

Aşağıdaki örnek, bir devamlılık, [güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). Bu örnekte, bir yedekleme İlkesi bölüm için etkindir ve hizmet olarak Azure Storage'da istenen bir sıklıkta yedekleme yapmadır.

Bir yedekleme çıktısından seçin [GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups). Bu senaryoda, yedekleme gibi aynı küme önce oluşturulur.

Geri yüklemeyi tetikleyecek için listeden bir yedekleme seçin. Geçerli _veri kaybı_/_veri bozulması_, aşağıdaki yedeği seçin:

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

Geri yükleme API'si için _Backupıd_ ve _BackupLocation_ ayrıntıları. Etkin yedek küme sahip böylece Service Fabric _yedekleme geri yükleme hizmeti (BRS)_ ilişkili yedekleme İlkesi doğru depolama konumundan tanımlar.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell
Restore-SFPartition  -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' -BackupId 'b0035075-b327-41a5-a58f-3ea94b68faa4' -BackupLocation 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'

```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

```powershell
$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4',
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
}

$body = (ConvertTo-Json $RestorePartitionReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

TrackRestoreProgress kullanarak geri yükleme ilerleme durumunu izleyebilirsiniz.

## <a name="track-restore-progress"></a>Geri Yükleme ilerlemesini İzle

Güvenilir durum bilgisi olan hizmet veya Reliable Actor ilişkin bir bölüm aynı anda yalnızca bir geri yükleme isteği kabul eder. Geçerli geri yükleme isteği tamamlandıktan sonra bir bölüm yalnızca başka bir istek kabul eder. Birden çok geri yükleme isteği aynı anda farklı bölümlerde tetiklenebilir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell
    Get-SFPartitionRestoreProgress -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22'
```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

$restoreResponse = (ConvertFrom-Json $response.Content)
$restoreResponse | Format-List
```

Aşağıdaki sırada geri yükleme isteği ilerler:

1. **Kabul edilen**: Bir _kabul edilen_ geri yükleme durumunu gösterir istenen bölüm doğru İstek parametreleri ile başlatıldı.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
2. **Inprogress**: Bir _Inprogress_ durumunu geri yükle, istekte belirtilen yedekleme ile bir geri yükleme bölümünde oluştuğunu gösterir. Bölüm raporları _dataloss_ durumu.
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. **Başarı**, **hatası**, veya **zaman aşımı**: İstenen bir geri yükleme, aşağıdaki durumlardan birinde tamamlanabilir. Her durum önem ve yanıt ayrıntıları aşağıdaki gibidir:
    - **Başarı**: A _başarı_ durumunu geri yükle buldum bölüm durumunu gösterir. Bölüm raporları _RestoredEpoch_ ve _RestoredLSN_ saat (UTC) ile birlikte durumları.

        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```        
    - **Hata**: A _hatası_ geri yükleme durumu, geri yükleme isteği başarısız olduğunu gösterir. Hatanın nedenini bildirilir.

        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    - **Zaman aşımı**: A _zaman aşımı_ durumunu geri yükle, istek zaman aşımı bulunduğunu belirtir. Yeni bir geri yükleme isteği oluşturma büyük [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout). Varsayılan zaman aşımı 10 dakikadır. Bölüm geri yüklemeyi yeniden istemeden önce bir veri kaybı durumunda olmadığından emin olun.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="automatic-restore"></a>Otomatik geri yükleme

Güvenilir durum bilgisi olan hizmet yapılandırabilirsiniz ve Service Fabric Reliable Actors bölümlerinde küme için _otomatik olarak geri yükleme_. Yedekleme ilkesinde ayarlamak `AutoRestore` için _true_. Etkinleştirme _otomatik olarak geri yükleme_ veri kaybı bildirildiğinde son bölüm yedekten verileri otomatik olarak yükler. Daha fazla bilgi için bkz.

- [Yedekleme İlkesi'nde otomatik geri yükleme etkinleştirme](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)
- [RestorePartition API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
- [GetPartitionRestoreProgress API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Sonraki adımlar
- [Düzenli yedekleme yapılandırması anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
