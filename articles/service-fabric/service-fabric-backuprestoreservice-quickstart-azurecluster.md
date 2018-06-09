---
title: Düzenli yedekleme ve geri yükleme Azure Service Fabric (Önizleme) | Microsoft Docs
description: Service Fabric'ın düzenli yedekleme ve uygulamalarınızı veri kaybına karşı korumak için özelliği geri yükleme.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: timlt
editor: hrushib
ms.assetid: FAA58600-897E-4CEE-9D1C-93FACF98AD1C
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: hrushib
ms.openlocfilehash: 73b5356f63199c7530fe5eef0c4b4b7ee617ff5f
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236129"
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
* Service Fabric kümesi ve üstü ile doku sürüm 6.2. Küme kurulumu Windows Server üzerinde olmalıdır. Başvuru [makale](service-fabric-cluster-creation-via-arm.md) Service Fabric oluşturma adımları için küme Azure kaynak şablonu kullanarak.
* Yedekleri depolamak için depolama alanına bağlanmak için gereken gizli şifrelenmesi X.509 sertifikası. Başvuru [makale](service-fabric-cluster-creation-via-arm.md) almak veya bir X.509 sertifikası oluşturmak nasıl bilmeniz.
* Service Fabric SDK sürüm 3.0 kullanılarak oluşturulan Service Fabric güvenilir Stateful uygulama veya üstü. .NET hedefleyen uygulamalar için çekirdek 2.0, uygulama Service Fabric SDK sürüm 3.1 kullanarak oluşturulur veya üstü.
* Uygulama yedekleri depolamak için Azure Storage hesabı oluşturun.

## <a name="enabling-backup-and-restore-service"></a>Yedekleme ve geri yükleme hizmetini etkinleştirme
Etkinleştirmek gereken ilk _yedekleme ve geri yükleme hizmet_ kümenizdeki. Şablonu dağıtmak istediğiniz kümenin alın. Kullanabilir [örnek şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) veya Resource Manager şablonu oluşturun. Etkinleştirme _yedekleme ve geri yükleme hizmet_ aşağıdaki adımlarla:

1. Denetleyin `apiversion` ayarlanır **`2018-02-01`** için `Microsoft.ServiceFabric/clusters` kaynak ve aksi durumda aşağıdaki kod parçacığında gösterildiği gibi güncelleştirebilirsiniz:

    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Şimdi etkinleştirmek _yedekleme ve geri yükleme hizmet_ aşağıdakileri ekleyerek `addonFeatures` altında bölümünde `properties` bölümünde aşağıdaki kod parçacığında gösterildiği gibi: 

    ```json
        "properties": {
            ...
            "addonFeatures":  ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```
3. X.509 sertifikası kimlik bilgilerinin şifrelenmesi için yapılandırın. Bu depolama alanına bağlanmak için sağlanan kimlik bilgileri devam ettirmeden önce şifrelendiğinden emin olmak önemlidir. Şifreleme sertifikası aşağıdakileri ekleyerek yapılandırma `BackupRestoreService` altında bölümünde `fabricSettings` bölümünde aşağıdaki kod parçacığında gösterildiği gibi: 

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

4. Küme şablonunuzu önceki değişiklikleriyle güncelleştirdikten sonra bunları uygulamak ve izin dağıtım/yükseltme tamamlayın. Tamamlandıktan sonra _yedekleme ve geri yükleme hizmet_ kümenizdeki çalışmaya başlar. Bu hizmetin URI `fabric:/System/BackupRestoreService` ve hizmet sistem hizmeti Service Fabric explorer bölümünde bulunabilir. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Düzenli yedekleme güvenilir durum bilgisi olan hizmet ve güvenilir aktörler için etkinleştirme
Şimdi güvenilir durum bilgisi olan hizmeti ve güvenilir aktörler için düzenli yedeklemeyi etkinleştirmek için adımlarında yol. Bu adımlarda varsayılır
- Kümenin X.509 güvenliği ile kullanarak kurulum olduğunu _yedekleme ve geri yükleme hizmet_.
- Bir güvenilir durum bilgisi olan hizmet kümede dağıtılır. Bu Hızlı Başlangıç Kılavuzu amacıyla uygulamasıdır URI `fabric:/SampleApp` ve bu uygulamaya ait güvenilir bir durum bilgisi olan hizmeti için bir URI `fabric:/SampleApp/MyStatefulService`. Bu hizmet ile tek bölüm dağıtılır ve bölüm kimliği `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.
- Yönetici rolü istemci sertifikasıyla yüklenir _My_ (_kişisel_) adını depolamak _Currentuser'a_ sertifika nereden makinede depolama konumu komut dosyaları çağrılır. Bu örnekte `1b7ebe2174649c45474a4819dafae956712c31d3` bu sertifikanın parmak izini olarak. İstemci sertifikaları hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).

### <a name="create-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme zamanlaması, yedekleme verilerini, ilke adını ve tam yedekleme tetiklemeden önce izin verilecek en fazla artımlı yedeklemeler için hedef depolama açıklayan yedekleme ilkesi oluşturmak ilk adımdır. 

Yedekleme depolama için Azure Storage hesabı yukarıda oluşturduğunuz kullanın. Kapsayıcı `backup-container` yedeklemelerini depolamak için yapılandırılır. Bunu zaten, yedekleme karşıya yükleme sırasında mevcut değilse, bu ada sahip bir kapsayıcı oluşturulur. Doldurmak `ConnectionString` Azure depolama hesabı için geçerli bir bağlantı dizesine ile değiştirerek `account-name` , depolama hesabı adı ile ve `account-key` değerini depolama hesabınızın anahtarıyla.

Yeni bir ilke oluşturmak için gerekli REST API çağırma için PowerShell Betiği aşağıdaki yürütün. Değiştir `account-name` , depolama hesabı adı ile ve `account-key` değerini depolama hesabınızın anahtarıyla.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

### <a name="enable-periodic-backup"></a>Dönemsel yedeklemeyi etkinleştirme
Uygulamanın veri koruma gereksinimlerini karşılamak için yedekleme ilkenizi tanımladıktan sonra yedekleme İlkesi uygulama ile ilişkili olmalıdır. Gereksinim bağlı olarak, yedekleme İlkesi uygulama, hizmet veya bir bölümü ile ilişkili olabilir.

Yedekleme ilkesi adı ile ilişkilendirmek için gerekli REST API çağırma için PowerShell Betiği aşağıdaki yürütme `BackupPolicy1` Yukarıdaki adımı uygulama ile oluşturulan `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/EnableBackup?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Düzenli yedeklemeler çalıştığını doğrulayın

Uygulama düzeyinde yedekleme etkinleştirdikten sonra güvenilir durum bilgisi olan hizmetler ve Reliable Actors altındaki uygulama ait tüm bölümler düzenli aralıklarla ilişkili yedekleme İlkesi uyarınca yedeklenen başlar. 

![Bölüm BackedUp sistem durumu olayı][0]

### <a name="list-backups"></a>Liste yedekleri

Güvenilir durum bilgisi olan hizmetler ve uygulamanın Reliable Actors ait tüm bölümleri ile ilişkili yedeklemeler numaralandırılan kullanarak _GetBackups_ API. Yedeklemeler, bir uygulama, hizmet veya bir bölüm için numaralandırılabilir.

Tüm bölümleri için oluşturulan yedekleri numaralandırmak için HTTP API'sini çağırmak için PowerShell Betiği aşağıdaki yürütme `SampleApp` uygulama.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/GetBackups?api-version=6.2-preview"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Örnek çıktı yukarıdaki için çalıştırın:

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

## <a name="preview-limitation-caveats"></a>Önizleme sınırlaması / uyarılar
- Service Fabric PowerShell cmdlet'leri yerleşiktir.
- Service Fabric CLI desteği.
- Otomatik yedekleme temizleme desteği yoktur. El ile temizleyin yedeklerini gerektirir.
- Service Fabric desteği Linux kümeleri.

## <a name="next-steps"></a>Sonraki Adımlar
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent_Azure.png

