---
title: "Azure Service Fabric uygulama dağıtımı | Microsoft Docs"
description: "Dağıtma ve Service Fabric PowerShell kullanarak uygulamalarda Kaldır."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/05/2017
ms.author: ryanwi
ms.openlocfilehash: 5a1279ba9626ece30491c8fc899054873f6359e2
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="deploy-and-remove-applications-using-powershell"></a>Dağıtma ve PowerShell kullanarak uygulamaları kaldırma
> [!div class="op_single_selector"]
> * [Resource Manager](service-fabric-application-arm-resource.md)
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)

<br/>

Bir kez bir [uygulama türü paketlenmiş][10], Azure Service Fabric kümesi içine dağıtımı için hazırdır. Dağıtım aşağıdaki üç adımdan oluşur:

1. Uygulama paketi görüntü deposuna karşıya yükleme
2. Uygulama türünü kaydetme
3. Uygulama örneği oluşturma

Bir uygulamanın dağıtıldığını ve bir örnek kümede çalışan sonra uygulama örneği ve uygulama türünü silebilirsiniz. Bir uygulamayı kümeden tamamen kaldırmak için aşağıdaki adımları içerir:

1. Çalışan Kaldır (veya Sil) uygulama örneği
2. Artık ihtiyacınız varsa uygulama türü kaydını kaldırma
3. Uygulama paketi görüntü deposundan kaldırır

Dağıtma ve yerel geliştirme kümenizde uygulamalarında hata ayıklama için Visual Studio kullanıyorsanız, yukarıdaki adımların tümünü otomatik olarak bir PowerShell komut dosyası gerçekleştirilir.  Bu komut dosyası içinde bulunur *betikleri* uygulama projesi klasörü. Bu makalede, Visual Studio dışında aynı işlemleri gerçekleştirebilmeleri için komut dosyası yaptıklarını üzerinde arka plan sağlar. 
 
## <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Bu makaledeki tüm PowerShell komutları çalıştırmadan önce her zaman başlamayı [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) Service Fabric kümeye bağlanmak için. Yerel geliştirme kümeye bağlanmak için şu komutu çalıştırın:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Bir uzak küme veya Azure Active Directory, X509 kullanılarak güvenlik altına kümeye bağlanma örnekleri için sertifikalar veya Windows Active Directory bkz [güvenli kümeye Bağlan](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-the-application-package"></a>Uygulama paketi yükleme
Uygulama paketini karşıya yükleme, iç Service Fabric bileşenleri tarafından erişilebilen bir konuma koyan.
Uygulama paketi yerel olarak doğrulamak istiyorsanız, kullanmak [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet'i.

[Kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutu, küme görüntü deposu için uygulama paketi yükler.

Derleme ve adlı bir uygulama paketi varsayalım *MyApplication* Visual Studio 2015'te. Varsayılan olarak, ApplicationManifest.xml listelenen uygulama türü adı "MyApplicationType" bağlıdır.  Gerekli uygulama bildirimini, hizmet bildirimlerini ve kod/config/veri paketleri içeren uygulama paketi bulunan *C:\Users\<kullanıcıadı\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*. 

Aşağıdaki komut, uygulama paketinin içeriğini listeler:

```powershell
PS C:\> $path = 'C:\Users\<user\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Uygulama paketi büyük ve/veya birçok dosyaları yoksa, şunları yapabilirsiniz [onu Sıkıştır](service-fabric-package-apps.md#compress-a-package). Sıkıştırma, boyutu ve dosya sayısını azaltır.
Yan etkisi, kaydetme ve kayıt uygulama türü daha hızlı. Özellikle paket sıkıştırmak için zaman eklerseniz karşıya yükleme zamanı şu anda, yavaş olabilir. 

Bir paket sıkıştırmak için aynı kullanın [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutu. Sıkıştırma yapılabilir ayrı karşıya yükleme işlemini kullanarak `SkipCopy` bayrağı veya karşıya yükleme işlemi ile birlikte. Sıkıştırılmış paketi üzerinde sıkıştırma uygulama yok.
Sıkıştırılmış paketi sıkıştırmasını kaldırma için aynı kullanın [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutunu `UncompressPackage` geçin.

Aşağıdaki cmdlet'i paketi görüntü deposuna kopyalama olmadan sıkıştırır. Paket sıkıştırılmış dosya için artık içerir `Code` ve `Config` paketler. (Paylaşım, uygulama türü adı ve sürümü için ayıklama belirli doğrulamaları paketini gibi) çok sayıda dahili işlemleri için gerekli olduğu için uygulama ve hizmet bildirimlerini, daraltılmış değil. Bildirimleri sıkıştırma bu işlemleri verimli hale getirir.

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

Büyük uygulama paketlerini için sıkıştırma zaman alır. En iyi sonuçlar için hızlı bir SSD sürücüsü kullanın. Sıkıştırma zamanları ve sıkıştırılmış paket boyutu Ayrıca paket içeriğini göre farklılık.
Örneğin, ilk ve sıkıştırma süresiyle sıkıştırılmış paket boyutu göster bazı paketler için sıkıştırma istatistikleri aşağıdadır.

|Başlangıç boyutu (MB)|Dosya sayısı|Sıkıştırma zamanı|Sıkıştırılmış paket boyutu (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Bir paket sıkıştırılmış sonra onu bir veya birden çok Service Fabric kümesi için gerektiği şekilde yüklenebilir. Sıkıştırılmış ve sıkıştırılmamış paketler için aynı dağıtım mekanizmadır. Paket sıkıştırılmış ise, bu nedenle küme görüntü deposunda depolanır ve uygulamayı çalıştırmadan önce düğümde sıkıştırılmamış.


Aşağıdaki örnek paketi görüntü deposuna "MyApplicationV1" adlı bir klasöre yükler:

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -TimeoutSec 1800
```

Belirtmezseniz, *- ApplicationPackagePathInImageStore* parametresi, uygulama paketi Image store "Hata ayıklama" klasörüne kopyalanır.

>[!NOTE]
>**Kopya ServiceFabricApplicationPackage** PowerShell oturumu bir Service Fabric kümesi bağlıysa, uygun görüntü deposu bağlantı dizesi otomatik olarak algılar. Service Fabric sürümleri 5.6 eski **- ImageStoreConnectionString** bağımsız değişkeni açıkça sağlanmalıdır.
>
>```powershell
>PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
>```
>
>**Get-ImageStoreConnectionStringFromClusterManifest** Service Fabric SDK PowerShell modülünün bir parçası olan cmdlet, görüntü deposu bağlantı dizesini almak için kullanılır.  SDK modülü içeri aktarmak için aşağıdakini çalıştırın:
>
>```powershell
>Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
>```
>
>Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.
>
>
>

Bir paketi yükleme süresini birden çok faktörlere bağlı olarak farklılık gösterir. Bu etkenler paketin, paket boyutu ve dosya boyutları dosya sayısı bazılarıdır. Kaynak makine ve Service Fabric kümesi arasındaki ağ hızı karşıya yükleme zamanı da etkiler. İçin varsayılan zaman aşımı [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) 30 dakikadır.
Açıklanan etkenlere bağlı olarak zaman aşımı süresini artırın gerekebilir. Paket Kopyala çağrısında sıkıştırma, sıkıştırma zaman da dikkate almanız gerekir.



## <a name="register-the-application-package"></a>Uygulama paketi Kaydet
Uygulama türü ve sürümü, uygulama paketi kaydedildikten sonra kullanılabilir hale uygulama bildiriminde bildirildi. Sistem önceki adımda karşıya yüklenen paket okur, paket doğrular, paket içeriğini işler ve işlenen paket bir iç sistem konumuna kopyalar.  

Çalıştırma [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) kümede uygulama türünü kaydetme ve dağıtım için kullanılabilir hale getirmek için cmdlet:

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

"MyApplicationV1" uygulama paketi bulunduğu Image store klasöründe bulunur. Uygulama türü adı "MyApplicationType" Sürüm "1.0.0 (her ikisi de uygulama bildiriminde bulunur)" ile şimdi kümedeki kaydedilir.

[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) komut, yalnızca sistem uygulama paketi başarıyla kaydolduktan sonra döndürür. Ne kadar süreyle kayıt alır boyutu ve uygulama paketi içeriğini bağlıdır. Gerekirse, **- TimeoutSec** parametre (60 saniye olan varsayılan zaman aşımı) daha uzun bir süre sağlamak için kullanılabilir.

Paket veya zaman aşımına uğruyor kullanırsanız büyük bir uygulamanız varsa, **- zaman uyumsuz** parametresi. Komutu, küme kayıt komutu kabul eder ve gerektiğinde işleme devam döndürür.
[Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komutu tüm başarıyla kayıtlı uygulama türü sürümleri ve kayıt durumlarını listeler. Kayıt tamamlandığında belirlemek için bu komutu kullanın.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="remove-an-application-package-from-the-image-store"></a>Bir uygulama paketi görüntü deposundan kaldırır
Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmanız önerilir.  Uygulama paketleri görüntü deposundan silme sistem kaynakları serbest bırakır.  Kullanılmayan uygulama paketleri tutma disk depolama alanı kullanır ve uygulama performans sorunlarına yol açar.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1
```

## <a name="create-the-application"></a>Uygulama oluşturma
Bir uygulama kullanarak başarıyla kaydettirildi herhangi bir uygulama türü sürümü örneği [yeni ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet'i. Her bir uygulama adı ile başlamalıdır *"fabric:"* düzen ve her bir uygulama örneği için benzersiz olmalıdır. Hedef uygulama türü uygulama bildiriminde tanımlanan varsayılan hizmetlerin de oluşturulur.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Verilen herhangi bir kayıtlı uygulama türü sürümü için birden çok uygulama örneği oluşturulabilir. Her uygulama örneği yalıtımı, kendi çalışma dizini ve işlem ile çalışır.

Hangi adlı görmek için uygulama ve hizmetlere çalıştırmak kümede çalışan [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) ve [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlet:

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Bir uygulamayı kaldırma
Uygulama örneğini artık gerekli olmadığında kalıcı olarak adını kullanarak kaldırabilirsiniz [Kaldır ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet'i. [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) tüm hizmet durumunu da, kalıcı olarak kaldırma uygulamaya ait tüm hizmetleri otomatik olarak kaldırır. 

> [!WARNING]
> Bu işlem geri alınamaz ve uygulama durumu kurtarılamıyor.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Bir uygulama türü kaydını kaldırma
Belirli bir uygulama türü sürümü artık gerekli olmadığında kullanılarak uygulama türü kaydını [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet'i. Uygulama ikili dosyaları kaldırarak Image store tarafından kullanılan kaydı siliniyor kullanılmayan uygulama türleri sürümleri depolama alanı. Uygulama türü kaydını uygulama paketi kaldırmaz. Uygulama türü olarak hiçbir uygulamaları karşı oluşturulur ve uygulama Beklemede yükseltmeler, Hayır başvurduğunuzdan sürece kaydı olabilir.

Çalıştırma [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kümede kayıtlı uygulama türlerini görmek için:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Çalıştırma [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) belirli uygulama türü kaydını kaldırmak için:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="troubleshooting"></a>Sorun giderme
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopya ServiceFabricApplicationPackage bir ImageStoreConnectionString için sorar
Service Fabric SDK ortam zaten ayarlanmış doğru Varsayılanları olması gerekir. Ancak gerekirse, tüm komutlar için ImageStoreConnectionString Service Fabric kümesi kullanarak değer eşleşmesi gerekir. Küme bildiriminde ImageStoreConnectionString bulabilirsiniz kullanarak alınan [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) ve Get-ImageStoreConnectionStringFromClusterManifest komutlar:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

**Get-ImageStoreConnectionStringFromClusterManifest** Service Fabric SDK PowerShell modülünün bir parçası olan cmdlet, görüntü deposu bağlantı dizesini almak için kullanılır.  SDK modülü içeri aktarmak için aşağıdakini çalıştırın:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

ImageStoreConnectionString küme bildiriminde bulunur:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.

### <a name="deploy-large-application-package"></a>Büyük uygulama paketini dağıtma
Sorun: [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) büyük uygulama paketi (GB sırasını) için zaman aşımına uğradı.
Deneyin:
- Daha büyük zaman aşımını belirtmek [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutu ile `TimeoutSec` parametresi. Varsayılan olarak, zaman aşımı 30 dakikadır.
- Kaynak makine ve küme arasındaki ağ bağlantısını denetleyin. Bağlantı yavaş ise, bir makine daha iyi bir ağ bağlantısı ile kullanmayı düşünün.
İstemci makine küme başka bir bölgede ise, bir istemci makine bir daha yakından ya da aynı bölgede küme olarak düşünün.
- Dış azaltma devreyi olmadığını denetleyin. Örneğin, görüntü deposu azure depolama kullanacak şekilde yapılandırıldığında, karşıya yükleme kısıtlanan.

Sorun: karşıya yükleme paketi başarıyla tamamlandı ancak [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) zaman aşımına uğradı. Deneyin:
- [Paket Sıkıştır](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalama önce.
Sıkıştırma küçültür ve hangi sırayla trafik miktarını azaltır ve bu Service Fabric çalışma dosyaları sayısı gerçekleştirmeniz gerekir. Karşıya yükleme işlemi (özellikle sıkıştırma zaman eklerseniz) daha yavaş olabilir, ancak kaydedin ve kaydı uygulama türü daha hızlı.
- Daha büyük zaman aşımını belirtmek [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) ile `TimeoutSec` parametresi.
- Belirtin `Async` için geçiş [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). Komutu, küme komutu kabul eder ve uygulama türü kaydını zaman uyumsuz olarak devam döndürür. Bu nedenle, bu durumda daha yüksek bir zaman aşımı belirtmek için gerek yoktur. [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komutu tüm başarıyla kayıtlı uygulama türü sürümleri ve kayıt durumlarını listeler. Kayıt tamamlandığında belirlemek için bu komutu kullanın.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Birçok dosyalarıyla uygulama paketini dağıtma
Sorun: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) zaman aşımına uğraması için uygulama paketi birçok dosyalarla (binlerce sırasını).
Deneyin:
- [Paket Sıkıştır](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalama önce. Sıkıştırma dosyalarının sayısını azaltır.
- Daha büyük zaman aşımını belirtmek [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) ile `TimeoutSec` parametresi.
- Belirtin `Async` için geçiş [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). Komutu, küme komutu kabul eder ve uygulama türü kaydını zaman uyumsuz olarak devam döndürür.
Bu nedenle, bu durumda daha yüksek bir zaman aşımı belirtmek için gerek yoktur. [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komutu tüm başarıyla kayıtlı uygulama türü sürümleri ve kayıt durumlarını listeler. Kayıt tamamlandığında belirlemek için bu komutu kullanın.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md)

[Service Fabric sistem durumu giriş](service-fabric-health-introduction.md)

[Tanılama ve bir Service Fabric hizmeti sorun giderme](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulamada modeli](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
