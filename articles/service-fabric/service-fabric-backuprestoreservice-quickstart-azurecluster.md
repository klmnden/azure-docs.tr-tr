---
title: Düzenli yedekleme ve geri yükleme Azure Service fabric'te | Microsoft Docs
description: Service Fabric'in düzenli yedekleme ve geri yükleme, uygulama verilerinin düzenli veri yedeklemeyi etkinleştirme özelliği.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: chackdan
editor: hrushib
ms.assetid: FAA58600-897E-4CEE-9D1C-93FACF98AD1C
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: hrushib
ms.openlocfilehash: 4d4bc69f00f86bc81c353ef0cc40f37f000ba6c4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670447"
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric"></a>Düzenli yedekleme ve geri yükleme Azure Service fabric'te 
> [!div class="op_single_selector"]
> * [Azure'da kümeler](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Tek başına kümeler](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric, güvenilir, dağıtılmış, mikro hizmet tabanlı bulut uygulamalarınızı geliştirin ve yönetin kolay bir dağıtılmış sistemler platformudur. Durum bilgisiz ve durum bilgisi olan mikro hizmetler çalışmasını sağlar. Durum bilgisi olan hizmetler, istek ve yanıt ya da bir işlemin ötesinde değişebilir, yetkili durumu koruyabilir. Durum bilgisi olan hizmet uzun bir süredir devre dışı kalması veya bir olağanüstü durum nedeniyle bilgileri kaybeder, son bazı yedekleme durumunun geri açıldığında sonra hizmet sağlamaya devam etmek için geri yüklenmesi gerekebilir.

Service Fabric hizmeti yüksek oranda kullanılabilir olmasını sağlamak için birden fazla düğümde durumu çoğaltır. Kümedeki bir düğümün başarısız olsa bile, hizmetin kullanılabilir olmaya devam eder. Bazı durumlarda, ancak, yine de hizmet verilerin daha geniş hatalarına karşı güvenilir olması önerilir.
 
Örneğin, aşağıdaki senaryolardan birini korumak için verileri yedeklemek hizmet isteyebilirsiniz:
- Tüm Service Fabric kümesinin kalıcı kaybı durumunda.
- Hizmet bölüm çoğaltmalarını çoğunu kalıcı kaybı
- Yönetim hataları yapabildiği durumu yanlışlıkla silinirse veya bozulursa. Örneğin, yeterli ayrıcalığa sahip bir yönetici, hizmet yanlışlıkla siler.
- Veri bozulması neden hataları hizmetinde. Örneğin, bozuk verileri güvenilir bir koleksiyona yazma hizmeti kod yükseltmesi başladığında, bu sorunla karşılaşabilirsiniz. Böyle bir durumda, bir önceki durumuna geri döndürülmesi hem kod hem de veri olabilir.
- Çevrimdışı veri işleme. Çevrimdışı verileri üreten hizmetten ayrı olarak gerçekleşen iş zekası için verilerin işlenmesi kullanışlı olabilir.

Service Fabric, belirli bir noktadaki için yerleşik bir API sağlar [yedekleme ve geri yükleme](service-fabric-reliable-services-backup-restore.md). Uygulama geliştiricileri, hizmetin durumunu düzenli aralıklarla yedekleme için bu API'leri kullanabilir. Hizmet yöneticileri belirli bir zamanda hizmet dışında bir yedekten tetiklemek istiyorsanız, ayrıca, gibi uygulama yükseltmeden önce geliştiriciler yedekleme kullanıma sunma (ve geri yükleme) hizmeti API olarak gerekir. Yedekleri bakım, bunun üzerinde ek bir maliyetidir. Örneğin, tam bir yedekleme tarafından izlenen her yarım saatte beş artımlı yedeklemeleri almak isteyebilirsiniz. Tam yedeklemeden sonra önceki artımlı yedeklemeleri silebilirsiniz. Bu yaklaşım, uygulama geliştirme sırasında ek maliyet baştaki ek kod gerektirir.

Yedekleme ve geri yükleme hizmeti Service Fabric durum bilgisi olan Hizmetleri'nde depolanan bilgilerin kolay ve otomatik yedekleme sağlar. Uygulama verileri düzenli aralıklarla yedekleme, veri kaybı ve hizmet kullanılabilirliği karşı kullanılan koruyarak için temeldir. Service Fabric, bir isteğe bağlı yedekleme ve herhangi ek bir kod yazmak zorunda kalmadan düzenli yedekleme (aktör Hizmetleri dahil olmak üzere) durum bilgisi olan Reliable Services özelliğinin yapılandırmanıza olanak sağlayan hizmet geri yükleme sağlar. Ayrıca, yedeklemeleri daha önce gerçekleştirilen geri kolaylaştırır. 


Service Fabric, bir dizi API aşağıdaki işlevselliği ilgili düzenli yedekleme ve geri yükleme özelliği sağlar:

- Yedekleme (harici) için depolama konumları yüklenecek düzenli yedekleme güvenilir durum bilgisi olan hizmetler ve Reliable Actors desteğiyle zamanlayın. Desteklenen depolama konumları
    - Azure Depolama
    - Dosya Paylaşımı (şirket içi)
- Yedeklemeleri listeleme
- Bir bölümün geçici bir yedeklemeyi tetikleyin
- Önceki yedeklemeyi kullanarak bir bölüm geri yükleme
- Yedeklemeleri geçici olarak askıya alma
- Bekletme yönetim yedeklerini (yakında)

## <a name="prerequisites"></a>Önkoşullar
* Service Fabric kümesi yapıyla 6.2 sürümü ve üzeri. Windows Server'da küme ayarlanması. Bu [makale](service-fabric-cluster-creation-via-arm.md) Service Fabric oluşturma adımları için Azure kaynak şablonu kullanarak küme.
* Yedeklemeleri depolamak için depolama alanına bağlanmak için gereken gizli şifreleme için X.509 sertifikası. Başvuru [makale](service-fabric-cluster-creation-via-arm.md) alın veya bir X.509 sertifikası oluşturmak nasıl bilmek.
* Service Fabric SDK'sı sürüm 3.0 kullanılarak oluşturulan Service Fabric durum bilgisi güvenilir olan uygulama veya üzeri. .NET Core 2.0 hedefleyen uygulamalar için Service Fabric SDK'sı sürüm 3.1 kullanarak uygulama oluşturulmalıdır veya üzeri.
* Uygulama yedeklemeleri depolamak için Azure depolama hesabı oluşturun.

## <a name="enabling-backup-and-restore-service"></a>Yedekleme ve geri yükleme Hizmeti'ni etkinleştirme
Etkinleştirmek gereken ilk _yedekleme ve geri yükleme hizmeti_ kümenizdeki. Şablonu dağıtmak istediğiniz küme için alın. Kullanabilir [örnek şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) veya bir Resource Manager şablonu oluşturabilirsiniz. Etkinleştirme _yedekleme ve geri yükleme hizmeti_ aşağıdaki adımları:

1. Bu maddeyi `apiversion` ayarlanır **`2018-02-01`** için `Microsoft.ServiceFabric/clusters` kaynak ve aksi takdirde, aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Şimdi etkinleştirmek _yedekleme ve geri yükleme hizmeti_ aşağıdakileri ekleyerek `addonFeatures` bölümüne `properties` bölümü aşağıdaki kod parçacığında gösterildiği gibi: 

    ```json
        "properties": {
            ...
            "addonFeatures":  ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```
3. Kimlik bilgilerinin şifrelenmesi için X.509 sertifikası yapılandırın. Depolamaya bağlanmak için sağlanan kimlik bilgileri devam ettirmeden önce şifrelendiğinden emin olmak önemlidir. Şifreleme sertifikası aşağıdakileri ekleyerek yapılandırma `BackupRestoreService` bölümüne `fabricSettings` bölümü aşağıdaki kod parçacığında gösterildiği gibi: 

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

4. Dağıtım/yükseltmesi, küme şablonunuza önceki değişiklikleriyle güncelleştirdikten, bunları uygulayın ve izin sonra tamamlayın. Tamamlandıktan sonra _yedekleme ve geri yükleme hizmeti_ kümenizde çalışan başlatır. Bu hizmetin URI `fabric:/System/BackupRestoreService` ve hizmet Sistem Service Fabric explorer hizmet bölümü altında bulunabilir. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme olanağı
Şimdi güvenilir durum bilgisi olan hizmet ve Reliable Actors için düzenli aralıklarla yedeklemeyi etkinleştirme adımlarında yol. Bu adımlarda varsayılır
- Kümenin X.509 güvenlik ile kullanarak kurulum olduğunu _yedekleme ve geri yükleme hizmeti_.
- Güvenilir durum bilgisi olan hizmet, küme üzerinde dağıtılır. Bu Hızlı Başlangıç Kılavuzu amacıyla uygulamasıdır URI `fabric:/SampleApp` ve bu uygulamaya ait güvenilir durum bilgisi olan hizmet için URI `fabric:/SampleApp/MyStatefulService`. Bu hizmet ile tek bölüm dağıtıldıktan ve bölüm kimliği `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.
- Yönetici rolüne sahip istemci sertifikası yüklü olan _My_ (_kişisel_) adını depolamak _CurrentUser_ sertifika deposu konumu altında nereden makinede betikleri çağrılır. Bu örnekte `1b7ebe2174649c45474a4819dafae956712c31d3` bu sertifikanın parmak izini olarak. İstemci sertifikaları hakkında daha fazla bilgi için bkz. [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).

### <a name="create-backup-policy"></a>Yedekleme ilkesi oluşturma

İlk adım, yedekleme zamanlaması açıklayan bir yedekleme ilkesi oluşturmak için depolama, yedekleme verileri, ilke adı, tam yedekleme ve bekletme ilkesi Yedekleme depolaması için tetiklemeden önce izin verilecek en fazla artımlı yedeklemeler için hedef olacaktır. 

Azure depolama hesabı, yukarıda oluşturulan Yedekleme depolaması için kullanın. Kapsayıcı `backup-container` yedeklemeleri depolamak için yapılandırılır. Bunu zaten, yedekleme karşıya yükleme sırasında mevcut değilse, bu ada sahip bir kapsayıcı oluşturulur. Doldurma `ConnectionString` bir Azure depolama hesabı için geçerli bir bağlantı dizesiyle değiştirerek `account-name` ile depolama hesabı adınızı ve `account-key` depolama hesabınızın anahtarıyla.

Aşağıdaki yeni ilke oluşturmak için gerekli REST API çağırmak için PowerShell betiğini yürütün. Değiştirin `account-name` ile depolama hesabı adınızı ve `account-key` depolama hesabınızın anahtarıyla.

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
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

> [!IMPORTANT]
> Çalışma zamanında bir sorun nedeniyle, bekletme ilkesi bekletme süresi 24 günden az olacak şekilde yapılandırılır; Aksi takdirde, çekirdek kayıp çoğaltma yük devretme sonrasında gitmek için yedekleme geri yükleme hizmeti neden olacağından emin olun.

### <a name="enable-periodic-backup"></a>Dönemsel yedeklemeyi etkinleştirme
Uygulamanın veri koruma gereksinimlerini karşılamak için yedekleme İlkesi tanımladıktan sonra yedekleme ilkesini uygulama ile ilişkili olması gerekir. Yedekleme İlkesi gereksinim bağlı olarak, bir uygulama, hizmet veya bir bölümü ile ilişkili olabilir.

Yedekleme ilkesi adı ile ilişkilendirmek için gerekli REST API'sini çağırmak için PowerShell Betiği aşağıdaki yürütme `BackupPolicy1` uygulamayla adım yukarıda oluşturduğunuz `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/EnableBackup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Düzenli yedeklemeleri çalıştığını doğrulayın

Uygulama düzeyinde yedekleme etkinleştirdikten sonra güvenilir durum bilgisi olan hizmetler ve Reliable Actors altındaki uygulama ait tüm bölümleri düzenli aralıklarla ilişkili yedekleme İlkesi uyarınca yedeklenen başlar. 

![Bölüm yedeklenen Sistem durumu olayı][0]

### <a name="list-backups"></a>Yedekleri Listele

Güvenilir durum bilgisi olan hizmetler ve uygulamanın Reliable Actors ait tüm bölümleri ile ilişkili yedekleri numaralandırılan kullanarak _GetBackups_ API. Yedeklemeler, bir uygulama, hizmet veya bir bölümü için listelenebilir.

Tüm bölümleri için oluşturulan yedekleri numaralandırmak için HTTP API çağırmak için PowerShell Betiği aşağıdaki yürütme `SampleApp` uygulama.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

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

## <a name="limitation-caveats"></a>Sınırlama / uyarılar
- Herhangi bir Service Fabric PowerShell cmdlet'leri yerleşik.
- Linux üzerinde Service Fabric desteği kümeleri.

## <a name="known-issues"></a>Bilinen Sorunlar
- Bekletme süresi 24 günden az olacak şekilde yapılandırıldığından emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
- [Düzenli yedekleme yapılandırması anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent_Azure.png

