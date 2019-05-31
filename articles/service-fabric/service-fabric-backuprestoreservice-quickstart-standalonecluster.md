---
title: Düzenli yedekleme ve geri yükleme Azure Service fabric'te | Microsoft Docs
description: Service Fabric'in düzenli yedekleme ve geri yükleme, uygulama verilerinin düzenli veri yedeklemeyi etkinleştirme özelliği.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: chackdan
editor: hrushib
ms.assetid: FAADBCAB-F0CF-4CBC-B663-4A6DCCB4DEE1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/24/2019
ms.author: hrushib
ms.openlocfilehash: 154efffcb1f86907fefecc060419c1d9450470f8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237337"
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric"></a>Düzenli yedekleme ve geri yükleme Azure Service fabric'te
> [!div class="op_single_selector"]
> * [Azure'da kümeler](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Tek başına kümeler](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric, güvenilir, dağıtılmış, mikro hizmet tabanlı bulut uygulamalarınızı geliştirin ve yönetin kolay bir dağıtılmış sistemler platformudur. Durum bilgisiz ve durum bilgisi olan mikro hizmetler çalışmasını sağlar. Durum bilgisi olan hizmetler, istek ve yanıt ya da bir işlemin ötesinde değişebilir, yetkili durumu koruyabilir. Durum bilgisi olan hizmet uzun bir süredir devre dışı kalması veya bir olağanüstü durum nedeniyle bilgileri kaybeder, son bazı yedekleme durumunun geri açıldığında sonra hizmet sağlamaya devam etmek için geri yüklenmesi gerekebilir.

Service Fabric hizmeti yüksek oranda kullanılabilir olmasını sağlamak için birden fazla düğümde durumu çoğaltır. Kümedeki bir düğümün başarısız olsa bile, hizmetin kullanılabilir olmaya devam eder. Bazı durumlarda, ancak, yine de hizmet verilerin daha geniş hatalarına karşı güvenilir olması önerilir.
 
Örneğin, aşağıdaki senaryolardan birini korumak için verileri yedeklemek bir hizmet isteyebilirsiniz:
- Tüm Service Fabric kümesinin kalıcı kaybı.
- Hizmet bölüm çoğaltmalarını çoğunu kalıcı kaybı
- Yönetim hataları yapabildiği durumu yanlışlıkla silinirse veya bozulursa. Örneğin, yeterli ayrıcalığa sahip bir yönetici, hizmet yanlışlıkla siler.
- Veri bozulması neden hataları hizmetinde. Örneğin, bozuk verileri güvenilir bir koleksiyona yazma hizmeti kod yükseltmesi başladığında, bu sorunla karşılaşabilirsiniz. Böyle bir durumda, bir önceki durumuna geri döndürülmesi hem kod hem de veri olabilir.
- Çevrimdışı veri işleme. Çevrimdışı verileri üreten hizmetten ayrı olarak gerçekleşen iş zekası için verilerin işlenmesi kullanışlı olabilir.

Service Fabric, belirli bir noktadaki için yerleşik bir API sağlar [yedekleme ve geri yükleme](service-fabric-reliable-services-backup-restore.md). Uygulama geliştiricileri, hizmetin durumunu düzenli aralıklarla yedekleme için bu API'leri kullanabilir. Hizmet yöneticileri belirli bir zamanda hizmet dışında bir yedekten tetiklemek istiyorsanız, ayrıca, gibi uygulama yükseltmeden önce geliştiriciler yedekleme kullanıma sunma (ve geri yükleme) hizmeti API olarak gerekir. Yedekleri bakım, bunun üzerinde ek bir maliyetidir. Örneğin, tam bir yedekleme tarafından izlenen her yarım saatte beş artımlı yedeklemeleri almak isteyebilirsiniz. Tam yedeklemeden sonra önceki artımlı yedeklemeleri silebilirsiniz. Bu yaklaşım, uygulama geliştirme sırasında ek maliyet baştaki ek kod gerektirir.

Düzenli aralıklarla uygulama verilerinin yedeği, dağıtılmış bir uygulama yönetmek ve veri kaybı veya hizmet kullanılabilirliği Süren kaybına karşı kullanılan koruyarak için temel bir gereksinimidir. Service Fabric, bir isteğe bağlı yedekleme ve herhangi ek bir kod yazmak zorunda kalmadan düzenli yedekleme (aktör Hizmetleri dahil olmak üzere) durum bilgisi olan Reliable Services özelliğinin yapılandırmanıza olanak sağlayan hizmet geri yükleme sağlar. Ayrıca, yedeklemeleri daha önce gerçekleştirilen geri kolaylaştırır. 

Service Fabric, bir dizi API aşağıdaki işlevselliği ilgili düzenli yedekleme ve geri yükleme özelliği sağlar:

- Yedekleme (harici) için depolama konumları yüklenecek düzenli yedekleme güvenilir durum bilgisi olan hizmetler ve Reliable Actors desteğiyle zamanlayın. Desteklenen depolama konumları
    - Azure Storage
    - Dosya Paylaşımı (şirket içi)
- Yedeklemeleri listeleme
- Bir bölümün geçici bir yedeklemeyi tetikleyin
- Önceki yedeklemeyi kullanarak bir bölüm geri yükleme
- Yedeklemeleri geçici olarak askıya alma
- Bekletme yönetim yedeklerini (yakında)

## <a name="prerequisites"></a>Önkoşullar
* Service Fabric kümesi yapıyla 6.2 sürümü ve üzeri. Windows Server'da küme ayarlanması. Bu [makale](service-fabric-cluster-creation-for-windows-server.md) gerekli paketi indirmek adımlar.
* Yedeklemeleri depolamak için depolama alanına bağlanmak için gereken gizli şifreleme için X.509 sertifikası. Başvuru [makale](service-fabric-windows-cluster-x509-security.md) nasıl almaya veya bir otomatik olarak imzalanan X.509 sertifikası oluşturmak için bilmeniz gereken.

* Service Fabric SDK'sı sürüm 3.0 kullanılarak oluşturulan Service Fabric durum bilgisi güvenilir olan uygulama veya üzeri. .Net Core hedefleyen uygulamalar için 2.0, uygulama kullanarak Service Fabric SDK'sı sürüm 3.1 oluşturulur veya üzeri.
* Yapılandırma aramaların Microsoft.ServiceFabric.Powershell.Http modülü [Önizleme içinde] yükleyin.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

* Küme kullanarak bağlı olduğundan emin olun `Connect-SFCluster` Microsoft.ServiceFabric.Powershell.Http modülünü kullanarak herhangi bir yapılandırma isteğini yapmadan önce komutu.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```

## <a name="enabling-backup-and-restore-service"></a>Yedekleme ve geri yükleme Hizmeti'ni etkinleştirme
Etkinleştirmek gereken ilk _yedekleme ve geri yükleme hizmeti_ kümenizdeki. Şablonu dağıtmak istediğiniz küme için alın. Kullanabileceğiniz [örnek şablonlarından](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples). Etkinleştirme _yedekleme ve geri yükleme hizmeti_ aşağıdaki adımları:

1. Bu maddeyi `apiversion` ayarlanır `10-2017` küme yapılandırmasında dosya ve aksi takdirde, aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    {
        "apiVersion": "10-2017",
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        ...
    }
    ```

2. Şimdi etkinleştirmek _yedekleme ve geri yükleme hizmeti_ aşağıdakileri ekleyerek `addonFeatures` bölümüne `properties` bölümü aşağıdaki kod parçacığında gösterildiği gibi: 

    ```json
        "properties": {
            ...
            "addonFeatures": ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```

3. Kimlik bilgilerinin şifrelenmesi için X.509 sertifikası yapılandırın. Bu, varsa, depolama alanına bağlanmak için devam ettirmeden önce şifrelenir, sağlanan kimlik bilgileri sağlamak önemlidir. Şifreleme sertifikası aşağıdakileri ekleyerek yapılandırma `BackupRestoreService` bölümüne `fabricSettings` bölümü aşağıdaki kod parçacığında gösterildiği gibi: 

    ```json
    "properties": {
        ...
        "addonFeatures": ["BackupRestoreService"],
        "fabricSettings": [{
            "name": "BackupRestoreService",
            "parameters":  [{
                "name": "SecretEncryptionCertThumbprint",
                "value": "[Thumbprint]"
            }]
        }
        ...
    }
    ```

4. Küme yapılandırma dosyanızı önceki değişiklikleriyle güncelleştirilmiş, bunları uygulayın ve izin sonra dağıtım/yükseltmeyi tamamlayın. Tamamlandıktan sonra _yedekleme ve geri yükleme hizmeti_ kümenizde çalışan başlatır. Bu hizmetin URI `fabric:/System/BackupRestoreService` ve hizmet Sistem Service Fabric explorer hizmet bölümü altında bulunabilir. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme olanağı
Şimdi güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme adımlarında yol. Bu adımlarda varsayılır
- Küme ile ayarlanmış _yedekleme ve geri yükleme hizmeti_.
- Güvenilir durum bilgisi olan hizmet, küme üzerinde dağıtılır. Bu Hızlı Başlangıç Kılavuzu amacıyla uygulamasıdır URI `fabric:/SampleApp` ve bu uygulamaya ait güvenilir durum bilgisi olan hizmet için URI `fabric:/SampleApp/MyStatefulService`. Bu hizmet ile tek bölüm dağıtıldıktan ve bölüm kimliği `23aebc1e-e9ea-4e16-9d5c-e91a614fefa7`.  

### <a name="create-backup-policy"></a>Yedekleme ilkesi oluşturma

İlk adım, yedekleme zamanlaması açıklayan bir yedekleme ilkesi oluşturmak için depolama, yedekleme verileri, ilke adı, tam yedekleme ve bekletme ilkesi Yedekleme depolaması için tetiklemeden önce izin verilecek en fazla artımlı yedeklemeler için hedef olacaktır. 

Yedekleme depolaması için dosya paylaşımı oluşturduğunuzda ve ReadWrite erişim için tüm Service Fabric düğümünü makineler bu dosya paylaşımına verin. Bu örnek paylaşımı adı ile varsayar `BackupStore` üzerinde mevcut olduğundan `StorageServer`.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell

New-SFBackupPolicy -Name 'BackupPolicy1' -AutoRestoreOnDataLoss $true -MaxIncrementalBackups 20 -FrequencyBased -Interval 00:15:00 -FileShare -Path '\\StorageServer\BackupStore' -Basic -RetentionDuration '10.00:00:00'

```
#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

Aşağıdaki yeni ilke oluşturmak için gerekli REST API çağırmak için PowerShell betiğini yürütün.

```powershell
$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}   

$StorageInfo = @{
    Path = '\\StorageServer\BackupStore'
    StorageKind = 'FileShare'
}

$RetentionPolicy = @{ 
    RetentionPolicyType = 'Basic'
    RetentionDuration =  'P10D'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
    RetentionPolicy = $RetentionPolicy
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "http://localhost:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
```

### <a name="enable-periodic-backup"></a>Dönemsel yedeklemeyi etkinleştirme
Uygulamanın veri koruma gereksinimlerini karşılamak üzere İlkesi tanımladıktan sonra yedekleme ilkesini uygulama ile ilişkili olması gerekir. Yedekleme İlkesi gereksinim bağlı olarak, bir uygulama, hizmet veya bir bölümü ile ilişkili olabilir.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell
Enable-SFApplicationBackup -ApplicationId 'SampleApp' -BackupPolicyName 'BackupPolicy1'
```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması
Yedekleme ilkesi adı ile ilişkilendirmek için gerekli REST API'sini çağırmak için PowerShell Betiği aşağıdaki yürütme `BackupPolicy1` uygulamayla adım yukarıda oluşturduğunuz `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "http://localhost:19080/Applications/SampleApp/$/EnableBackup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Düzenli yedeklemeleri çalıştığını doğrulayın

Uygulama için yedekleme etkinleştirdikten sonra güvenilir durum bilgisi olan hizmetler ve Reliable Actors altındaki uygulama ait tüm bölümleri düzenli aralıklarla ilişkili yedekleme İlkesi uyarınca yedeklenen başlar.

![Bölüm yedeklenen Sistem durumu olayı][0]

### <a name="list-backups"></a>Yedekleri Listele

Güvenilir durum bilgisi olan hizmetler ve uygulamanın Reliable Actors ait tüm bölümleri ile ilişkili yedekleri numaralandırılan kullanarak _GetBackups_ API. Gereksinim bağlı olarak, yedeklemeler uygulama, hizmet veya bir bölümü için listelenebilir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell kullanarak Microsoft.ServiceFabric.Powershell.Http Modülü

```powershell
    Get-SFApplicationBackupList -ApplicationId WordCount     
```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak rest araması

Tüm bölümleri için oluşturulan yedekleri numaralandırmak için HTTP API çağırmak için PowerShell Betiği aşağıdaki yürütme `SampleApp` uygulama.

```powershell
$url = "http://localhost:19080/Applications/SampleApp/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```

Yukarıdaki örnek çıktısı çalıştırın:

```
BackupId                : d7e4038e-2c46-47c6-9549-10698766e714
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.39.40.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2058
CreationTimeUtc         : 2018-04-01T19:39:40Z
FailureError            : 

BackupId                : 8c21398a-2141-4133-b4d7-e1a35f0d7aac
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.54.38.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2237
CreationTimeUtc         : 2018-04-01T19:54:38Z
FailureError            : 

BackupId                : fc75bd4c-798c-4c9a-beee-e725321f73b2
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 20.09.44.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2437
CreationTimeUtc         : 2018-04-01T20:09:44Z
FailureError            : 
```

## <a name="limitation-caveats"></a>Sınırlama / uyarılar
- Service Fabric PowerShell cmdlet'leri Önizleme modundadır.
- Linux üzerinde Service Fabric desteği kümeleri.

## <a name="next-steps"></a>Sonraki adımlar
- [Düzenli yedekleme yapılandırması anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent.png

