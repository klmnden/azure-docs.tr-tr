---
title: Düzenli yedekleme ve geri yükleme Azure Service Fabric (Önizleme) | Microsoft Docs
description: Service Fabric'ın düzenli yedekleme ve uygulamalarınızı veri kaybına karşı korumak için özelliği geri yükleme.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: timlt
editor: hrushib
ms.assetid: FAADBCAB-F0CF-4CBC-B663-4A6DCCB4DEE1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: hrushib
ms.openlocfilehash: e9bc85cec6cb1d0e35aa71f4e1934c057dbf946d
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114536"
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric-preview"></a>Düzenli yedekleme ve geri yükleme Azure Service Fabric (Önizleme)
> [!div class="op_single_selector"]
> * [Azure kümelerinde](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Tek başına kümeleri](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric geliştirmek ve temel güvenilir, dağıtılmış, mikro bulut uygulamalarını yönetmek kolaylaştıran bir dağıtılmış sistemler platformudur. Durum bilgisiz ve durum bilgisi olan mikro hizmetlerin çalışmasını sağlar. Durum bilgisi olan hizmetler değişebilir, yetkili durumu istek ve yanıt veya tam bir işlem dışında tutabilirsiniz. Bir durum bilgisi olan hizmeti uzun bir süre için arıza veya bir olağanüstü durum nedeniyle bilgileri kaybeder, durumu için bazı son yedeklemeden geri gelmesi sonra hizmet sağlamaya devam etmek için geri yüklenmesi gerekebilir.

Service Fabric durum hizmeti yüksek oranda kullanılabilir olduğundan emin olmak için birden çok düğümlere çoğaltır. Kümedeki bir düğümün başarısız olsa bile, hizmetin kullanılabilir olmaya devam eder. Bazı durumlarda, ancak bunu hala hizmet verilerin daha geniş arızalarına karşı güvenilir olması önerilir.
 
Örneğin, hizmet verilerini aşağıdaki senaryolardan birini korumak için yedekleme isteyebilirsiniz:
- Tüm bir Service Fabric kümesi kalıcı kaybolması durumunda.
- Bir hizmet bölüm kopyalarını çoğunu kalıcı kaybı
- Yapabildiği durumu yanlışlıkla silinen bozuk alır veya yönetimsel hatalar. Örneğin, yeterli ayrıcalığa sahip bir yönetici, hizmet yanlışlıkla siler.
- Veri bozulmasına neden hatalar hizmet. Örneğin, bir hizmet kod yükseltme güvenilir bir koleksiyona hatalı veri yazma başlatıldığında bu ortaya çıkabilir. Böyle bir durumda, önceki bir durumuna döndürülmesi hem kod hem de veri olabilir.
- Çevrimdışı veri işleme. Çevrimdışı veri üreten hizmetten ayrı ayrı gerçekleşir iş zekası verileri işlenmesini sağlamak kullanışlı olabilir.

Service Fabric zamanında işaret edecek şekilde yerleşik bir API sağlar [yedekleme ve geri yükleme](service-fabric-reliable-services-backup-restore.md). Uygulama geliştiricilerinin hizmetinin durumunu düzenli aralıklarla yedekleme için bu API'leri kullanabilir. Hizmet yöneticileri yedekten hizmet dışında belirli tetiklemek istiyorsanız, ayrıca, gibi uygulama yükseltmeden önce geliştiriciler yedekleme kullanıma (ve geri yükleme) bir API hizmetinden olarak gerekir. Yedeklemeleri bakımı, bunun üzerinde ek bir maliyetidir. Örneğin, 5 artımlı yedeklemeler yarım saatte bir tam yedekleme tarafından izlenen almak isteyebilirsiniz. Tam yedeklemeden sonra önceki artımlı yedeklemeleri silebilirsiniz. Bu yaklaşım, uygulama geliştirme sırasında ek bir maliyet baştaki ek kod gerektirir.

Düzenli aralıklarla uygulama verilerinin yedeği, dağıtılmış bir uygulamayı yönetmeyi ve veri kaybı veya uzun süren hizmet kullanılabilirlik kaybına karşı kullanılan koruyarak temel bir gerekiyor. Service Fabric bir isteğe bağlı yedekleme ve herhangi bir ek kod yazmak zorunda kalmadan durum bilgisi olan güvenilir (aktör Hizmetleri dahil olmak üzere) hizmetlerinin düzenli yedekleme yapılandırmanızı sağlar geri yükleme hizmet sağlar. Ayrıca, yedeklemeleri önceden alınmış geri kolaylaştırır. 

> [!NOTE]
> Düzenli yedekleme ve geri yükleme özelliği oynamakta **Önizleme** ve üretim iş yükleri için desteklenmez. 
>

Service Fabric aşağıdaki işlevselliği ilgili düzenli yedekleme elde etmek ve geri yükleme özelliğini için API kümesi sağlar:

- (Dış) yedekleme depolama konumları karşıya yüklemek için düzenli yedeklemesi güvenilir durum bilgisi olan hizmetler ve Reliable Actors desteğiyle zamanlayın. Desteklenen depolama konumları
    - Azure Storage
    - Dosya Paylaşımı (şirket içi)
- Yedeklemeleri listeleme
- Bir bölümün bir geçici yedeklemeyi tetikleyin
- Önceki Yedekleme kullanarak bir bölüm geri yükleme
- Yedeklemeleri geçici olarak askıya alma
- Bekletme yönetim yedeklerini (yakında)

## <a name="prerequisites"></a>Önkoşullar
* Service Fabric kümesi ve üstü ile doku sürüm 6.2. Küme kurulumu Windows Server üzerinde olmalıdır. Başvuru [makale](service-fabric-cluster-creation-for-windows-server.md) gerekli paketini karşıdan yüklemek adımları için.
* Yedekleri depolamak için depolama alanına bağlanmak için gereken gizli şifrelenmesi X.509 sertifikası. Başvuru [makale](service-fabric-windows-cluster-x509-security.md) nasıl edinmeye veya otomatik olarak imzalanan bir X.509 sertifikası oluşturmak için bilmeniz.
* Service Fabric SDK sürüm 3.0 kullanılarak oluşturulan Service Fabric güvenilir Stateful uygulama veya üstü. .NET hedefleyen uygulamalar için çekirdek 2.0, uygulama Service Fabric SDK sürüm 3.1 kullanarak oluşturulur veya üstü.

## <a name="enabling-backup-and-restore-service"></a>Yedekleme ve geri yükleme hizmetini etkinleştirme
Etkinleştirmek gereken ilk _yedekleme ve geri yükleme hizmet_ kümenizdeki. Şablonu dağıtmak istediğiniz kümenin alın. Kullanabileceğiniz [örnek şablonlarından](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples). Etkinleştirme _yedekleme ve geri yükleme hizmet_ aşağıdaki adımlarla:

1. Denetleyin `apiversion` ayarlanır `10-2017` küme yapılandırmasında dosya ve aksi durumda aşağıdaki kod parçacığında gösterildiği gibi güncelleştirebilirsiniz:

    ```json
    {
        "apiVersion": "10-2017",
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        ...
    }
    ```

2. Şimdi etkinleştirmek _yedekleme ve geri yükleme hizmet_ aşağıdakileri ekleyerek `addonFeatures` altında bölümünde `properties` bölümünde aşağıdaki kod parçacığında gösterildiği gibi: 

    ```json
        "properties": {
            ...
            "addonFeatures": ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```

3. X.509 sertifikası kimlik bilgilerinin şifrelenmesi için yapılandırın. Bu, varsa, depolama alanına bağlanmak için devam ettirmeden önce şifreliyse sağlanan kimlik bilgileri sağlamak önemlidir. Şifreleme sertifikası aşağıdakileri ekleyerek yapılandırma `BackupRestoreService` altında bölümünde `fabricSettings` bölümünde aşağıdaki kod parçacığında gösterildiği gibi: 

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

4. Küme yapılandırma dosyanızı önceki değişiklikleriyle güncelleştirdikten sonra bunları uygulamak ve izin dağıtım/yükseltme tamamlayın. Tamamlandıktan sonra _yedekleme ve geri yükleme hizmet_ kümenizdeki çalışmaya başlar. Bu hizmetin URI `fabric:/System/BackupRestoreService` ve hizmet sistem hizmeti Service Fabric explorer bölümünde bulunabilir. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Düzenli yedekleme güvenilir durum bilgisi olan hizmet ve güvenilir aktörler için etkinleştirme
Şimdi güvenilir durum bilgisi olan hizmeti ve güvenilir aktörler için düzenli yedeklemeyi etkinleştirmek için adımlarında yol. Bu adımlarda varsayılır
- Küme kurulumu olduğunu _yedekleme ve geri yükleme hizmet_.
- Bir güvenilir durum bilgisi olan hizmet kümede dağıtılır. Bu Hızlı Başlangıç Kılavuzu amacıyla uygulamasıdır URI `fabric:/SampleApp` ve bu uygulamaya ait güvenilir bir durum bilgisi olan hizmeti için bir URI `fabric:/SampleApp/MyStatefulService`. Bu hizmet ile tek bölüm dağıtılır ve bölüm kimliği `23aebc1e-e9ea-4e16-9d5c-e91a614fefa7`.  

### <a name="create-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme zamanlaması, yedekleme verilerini, ilke adını ve tam yedekleme tetiklemeden önce izin verilecek en fazla artımlı yedeklemeler için hedef depolama açıklayan yedekleme ilkesi oluşturmak ilk adımdır. 

Yedekleme depolama dosya paylaşımı oluşturmak ve ReadWrite erişim bu dosya paylaşımı için tüm Service Fabric düğümü makineler verin. Bu örnek adı Paylaş varsayar `BackupStore` üzerinde mevcut olduğundan `StorageServer`.

Yeni bir ilke oluşturmak için gerekli REST API çağırma için PowerShell Betiği aşağıdaki yürütün.

```powershell
$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}   

$StorageInfo = @{
    Path = '\\StorageServer\BackupStore'
    StorageKind = 'FileShare'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "http://localhost:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
```

### <a name="enable-periodic-backup"></a>Dönemsel yedeklemeyi etkinleştirme
Uygulamanın veri koruma gereksinimlerini karşılamak için ilke tanımladıktan sonra yedekleme İlkesi uygulama ile ilişkili olmalıdır. Gereksinim bağlı olarak, yedekleme İlkesi uygulama, hizmet veya bir bölümü ile ilişkili olabilir.

Yedekleme ilkesi adı ile ilişkilendirmek için gerekli REST API çağırma için PowerShell Betiği aşağıdaki yürütme `BackupPolicy1` Yukarıdaki adımı uygulama ile oluşturulan `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "http://localhost:19080/Applications/SampleApp/$/EnableBackup?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Düzenli yedeklemeler çalıştığını doğrulayın

Uygulama için yedekleme etkinleştirdikten sonra güvenilir durum bilgisi olan hizmetler ve Reliable Actors altındaki uygulama ait tüm bölümler düzenli aralıklarla ilişkili yedekleme İlkesi uyarınca yedeklenen başlar. 

![Bölüm BackedUp sistem durumu olayı][0]

### <a name="list-backups"></a>Liste yedekleri

Güvenilir durum bilgisi olan hizmetler ve uygulamanın Reliable Actors ait tüm bölümleri ile ilişkili yedeklemeler numaralandırılan kullanarak _GetBackups_ API. Gereksinim bağlı olarak, uygulama, hizmet veya bir bölüm için yedeklemeleri numaralandırılabilir.

Tüm bölümleri için oluşturulan yedekleri numaralandırmak için HTTP API'sini çağırmak için PowerShell Betiği aşağıdaki yürütme `SampleApp` uygulama.

```powershell
$url = "http://localhost:19080/Applications/SampleApp/$/GetBackups?api-version=6.2-preview"

$response = Invoke-WebRequest -Uri $url -Method Get

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Örnek çıktı yukarıdaki için çalıştırın:

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

## <a name="preview-limitation-caveats"></a>Önizleme sınırlaması / uyarılar
- Service Fabric PowerShell cmdlet'leri yerleşiktir.
- Service Fabric CLI desteği.
- Otomatik yedekleme temizleme desteği yoktur. El ile temizleyin yedeklerini gerektirir.
- Service Fabric desteği Linux kümeleri.

## <a name="next-steps"></a>Sonraki Adımlar
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent.png

