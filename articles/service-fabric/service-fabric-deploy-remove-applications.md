---
title: Azure Service Fabric uygulama dağıtımı | Microsoft Docs
description: Nasıl dağıtılacağı ve PowerShell kullanarak Service Fabric uygulamaları kaldırın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: aljo
ms.openlocfilehash: 6bd3f45958870a20ac0386bd2f8a67ef4b4c0010
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670566"
---
# <a name="deploy-and-remove-applications-using-powershell"></a>Dağıtma ve PowerShell kullanarak uygulamaları kaldırma
> [!div class="op_single_selector"]
> * [Resource Manager](service-fabric-application-arm-resource.md)
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)

<br/>

Bir kez bir [uygulama türü paketlenmiş][10], bir Azure Service Fabric kümesine dağıtım için hazırdır. Dağıtım, aşağıdaki üç adımdan oluşur:

1. Uygulama paketi görüntü deposuna yükleyin.
2. Görüntü deposu göreli yolu ile uygulama türünü kaydedin.
3. Uygulama örneğini oluşturun.

Dağıtılan uygulamayı artık gerekli olduğunda uygulama örneğini ve uygulama türünü silebilirsiniz. Bir uygulamayı kümeden tamamen kaldırmak için aşağıdaki adımları içerir:

1. Çalışan Kaldır (veya sildiğinizde) uygulama örneği.
2. Artık ihtiyacınız yoksa, uygulama türünün kaydını silin.
3. Uygulama paketi görüntü deposundan kaldırır.

Dağıtma ve hata ayıklama, yerel geliştirme kümenizde uygulamaları için Visual Studio kullanıyorsanız, tüm önceki adımları otomatik olarak bir PowerShell Betiği işlenir.  Bu betik bulunan *betikleri* uygulama projesinin klasörü. Bu makalede, Visual Studio dışında aynı işlemleri gerçekleştirebilmeleri için bu betiği yaptığı hakkında arka plan sağlar. 

Bir uygulamayı dağıtmak için başka bir yolu, dış sağlama kullanmaktır. Uygulama paketi olabilir [paketlenmiş olarak `sfpkg` ](service-fabric-package-apps.md#create-an-sfpkg) ve bir dış depoya yüklenir. Bu durumda, karşıya yükleme görüntü deposu için gerekli değildir. Dağıtım, aşağıdaki adımlar gerekir:

1. Karşıya yükleme `sfpkg` bir dış depolama. Dış depo, bir REST http veya https uç noktasını kullanıma sunar herhangi deposu olabilir.
2. Dış indirme URI'si ve uygulama türü bilgilerini kullanarak uygulama türünü kaydedin.
2. Uygulama örneğini oluşturun.

Temizleme, uygulama türünün kaydını silmek ve uygulama örneklerini kaldırın. Paket görüntü deposuna kopyalanamadı çünkü hiçbir temizleme geçici bir konuma yoktur. Dış depodan sağlama, Service Fabric 6.1 sürümünde'den itibaren kullanılabilmektedir.

>[!NOTE]
> Visual Studio, dış sağlama şu anda desteklemiyor.

 
## <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Bu makaledeki tüm PowerShell komutları çalıştırmadan önce her zaman kullanarak başlatın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) Service Fabric kümesine bağlanmak için. Yerel geliştirme kümesine bağlanmak için şu komutu çalıştırın:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Örnekleri, bir uzak kümeye veya Azure Active Directory, X509 kullanılarak güvenli bir kümeye bağlanmak için sertifikaları veya Windows Active Directory bakın [güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-the-application-package"></a>Uygulama paketini karşıya yükleyin
Uygulama paketini karşıya yükleme, iç Service Fabric bileşenleri tarafından erişilebilen bir konuma yerleştirir.
Uygulama paketi yerel olarak doğrulamak istediğiniz kullanırsanız [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet'i.

[Kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komut, uygulama paketini kümenin görüntü deposuna yükler.

Derleme ve adlı bir uygulama paketi varsayalım *MyApplication* Visual Studio 2015'te. Varsayılan olarak, "MyApplicationType" ApplicationManifest.xml içinde listelenen uygulama türü adı var.  Gerekli uygulama bildirimi, hizmet bildirimleri ve kodu/config/veri paketleri içeren uygulama paketini bulunan *C:\Users\<kullanıcıadı\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*. 

Aşağıdaki komut, uygulama paketinin içeriği listeler:

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

Uygulama paketi büyük ve/veya sahip çok sayıda dosya varsa [bu sıkıştırma](service-fabric-package-apps.md#compress-a-package). Sıkıştırma, boyutu ve dosya sayısını azaltır.
Yan etkisi, kayıt ve kayıt daha hızlı uygulama türü. Karşıya yükleme zamanı şu anda, özellikle paket sıkıştırmak için zaman eklerseniz daha yavaş olabilir. 

Bir paket sıkıştırmak için aynı kullanın [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutu. Sıkıştırma yapılabilir ayrı karşıya yükleme işlemini kullanarak `SkipCopy` bayrağı veya karşıya yükleme işlemi ile birlikte. Sıkıştırılmış bir paketi üzerinde sıkıştırma uygulama İşlemsiz.
Sıkıştırılmış bir paketi sıkıştırmasını açmak için aynı kullanın [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutunu `UncompressPackage` geçin.

Aşağıdaki cmdlet, görüntü deposuna kopyalamadan paket sıkıştırır. Paket artık sıkıştırılmış dosyaları içeren `Code` ve `Config` paketleri. (Paket paylaşımı, uygulama türü adı ve sürümü için ayıklama belirli doğrulamaları gibi) birçok iç işlemler için gerekli olduğu uygulama ve hizmet bildirimleri, daraltılmış değil. Bildirimleri sıkıştırma, bu işlemleri verimli hale getirir.

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

Büyük uygulama paketleri için sıkıştırma zaman alır. En iyi sonuçları almak için hızlı bir SSD sürücüsü kullanın. Sıkıştırma zamanları ve sıkıştırılmış paket boyutu Ayrıca paket içeriğini göre farklılık gösterir.
Örneğin, ilk ve sıkıştırma süresiyle sıkıştırılmış paket boyutu göster bazı paketler için sıkıştırma istatistikleri aşağıdadır.

|İlk boyutu (MB)|Dosya sayısı|Sıkıştırma zaman|Sıkıştırılmış paket boyutu (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Bir paket sıkıştırılmış sonra bir veya birden çok Service Fabric kümeleri için gerektiği şekilde karşıya yüklenebilir. Sıkıştırılmış ve sıkıştırılmamış paketleri için aynı dağıtım mekanizmadır. Sıkıştırılmış paketlerini kümenin görüntü deposuna şekilde depolanır. Uygulamayı çalıştırmadan önce paketleri düğüme sıkıştırılmamış.


Aşağıdaki örnek, "MyApplicationV1" adlı bir klasöre paket görüntü deposuna yükler:

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -TimeoutSec 1800
```

Siz belirtmezseniz *- ApplicationPackagePathInImageStore* parametresi, uygulama paketi, görüntü deposu "Debug" klasörüne kopyalanır.

>[!NOTE]
>**Kopyalama ServiceFabricApplicationPackage** uygun görüntü deposu bağlantı dizesi PowerShell oturumu bir Service Fabric kümesine bağlı olup olmadığını otomatik olarak algılar. Sürümleri Service Fabric 5.6 eski **- Imagestoreconnectionstring** bağımsız değişkenini açıkça belirtilmelidir.
>
>```powershell
>PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
>```
>
>**Get-ImageStoreConnectionStringFromClusterManifest** cmdlet'i, Service Fabric SDK PowerShell modülünü parçası olan resim depolama bağlantı dizesini almak için kullanılır.  SDK modülü içeri aktarmak için çalıştırın:
>
>```powershell
>Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
>```
>
>Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.
>
>
>

Bir paketi karşıya yüklemek için gereken süreyi birden çok faktörlere bağlı olarak farklılık gösterir. Bu etkenler paketin, paket boyutu ve dosya boyutları dosyaların sayısı, bazılarıdır. Kaynak makine ve Service Fabric kümesi arasındaki ağ hızı, karşıya yükleme zamanı da etkilenir. Varsayılan zaman aşımını [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) 30 dakikadır.
Açıklandığı gibi faktörlere bağlı olarak, zaman aşımı artırmanız gerekebilir. Paket Kopyala çağrısında'nu sıkıştırma, sıkıştırma zaman de dikkate almanız gerekir.



## <a name="register-the-application-package"></a>Uygulama paketini kaydetme
Uygulama türü ve sürümü, uygulama paketi kaydedildiğinde kullanıma hazır hale uygulama bildirimde belirtilen. Sistem önceki adımda karşıya yüklenen paket okur, paket doğrular, paket içeriğini işler ve işlenen paket bir iç sistem konumuna kopyalar.  

Çalıştırma [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) kümeye uygulama türünü kaydedin ve dağıtım için kullanılabilir hale getirmek için cmdlet:

### <a name="register-the-application-package-copied-to-image-store"></a>Görüntü deposuna kopyalanan uygulama paketini kaydetme
Bir paket, daha önce görüntü deposuna kopyalanır, kayıt işlemi görüntü deposundaki görece yolu belirtir.

```powershell
PS C:\> Register-ServiceFabricApplicationType -ApplicationPathInImageStore MyApplicationV1
Register application type succeeded
```

"MyApplicationV1" görüntü deposundaki uygulama paketi bulunduğu bir klasördür. Uygulama türü adı "MyApplicationType" ve "(her ikisi de, uygulama bildiriminde bulunan) 1.0.0" sürümü ile artık kümede kayıtlı.

### <a name="register-the-application-package-copied-to-an-external-store"></a>Bir dış depoya kopyalandı uygulama paketini kaydetme
Service Fabric 6.1 sürümünde ile başlayarak, bir dış depodan paket indiriliyor destekler sağlayın. URI yolu temsil eden indirme [ `sfpkg` uygulama paketini](service-fabric-package-apps.md#create-an-sfpkg) nereden uygulama paketi, HTTP veya HTTPS protokollerini kullanan indirilebilir. Paket daha önce bu dış konuma yüklenmiş olmalıdır. Service Fabric dosyayı indirebilirsiniz şekilde URI okuma erişimine izin vermeniz gerekir. `sfpkg` Dosya uzantısı ".sfpkg" olması gerekir. Sağlama işlemi, uygulama bildiriminde bulunan uygulama türü bilgilerini içermelidir.

```
PS C:\> Register-ServiceFabricApplicationType -ApplicationPackageDownloadUri "https://sftestresources.blob.core.windows.net:443/sfpkgholder/MyAppPackage.sfpkg" -ApplicationTypeName MyApp -ApplicationTypeVersion V1 -Async
```

[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) komutu, yalnızca sistem uygulama paketi başarıyla kaydolduktan sonra döndürür. Ne kadar süreyle kayıt alır boyutu ve uygulama paketinin içeriği bağlıdır. Gerekirse, **- TimeoutSec** parametresi, uzun bir zaman aşımı (60 saniye olan varsayılan zaman aşımı) sağlamak için kullanılabilir.

Büyük bir uygulama paketini veya zaman aşımları yaşıyorsanız kullanmak varsa **zaman - uyumsuz** parametresi. Küme kayıt komutu kabul ettiğinde komut döndürür. Kayıt işlemi, gerektiği şekilde devam eder.
[Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komut, uygulama türü sürümleri ve kayıt durumlarını listeler. Kayıt tamamlandığında belirlemek için bu komutu kullanabilirsiniz.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="remove-an-application-package-from-the-image-store"></a>Bir uygulama paketi görüntü deposundan kaldırmak
Bir paket görüntü deposuna kopyaladıysanız, uygulamanın başarıyla kaydedildikten sonra geçici konumundan kaldırmanız gerekir. Uygulama paketleri görüntü deposundan silme, sistem kaynakları serbest bırakır. Kullanılmayan uygulama paketleri tutma disk depolama alanını kullanan ve uygulama performası sorunlarını için yol açar.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1
```

## <a name="create-the-application"></a>Uygulama oluşturma
Bir uygulamadan kullanarak başarıyla kaydedildi herhangi bir uygulama türü sürümü oluşturabileceğiniz [yeni ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet'i. Her uygulamanın adı ile başlamalıdır *"fabric:"* şeması ve her uygulama örneği için benzersiz olmalıdır. Hedef uygulama türü uygulama bildiriminde tanımlanan varsayılan hizmetlerin de oluşturulur.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Birden fazla uygulama örneğinin belirtilen herhangi bir kayıtlı uygulama türü sürümü için oluşturulabilir. Her uygulama örneği yalıtım, kendi çalışma dizini ve işlem ile çalışır.

Hangi adlı görmek için uygulamalar ve hizmetler çalıştırmak, bir kümede çalışan [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication) ve [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlet'leri:

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

## <a name="remove-an-application"></a>Uygulamayı kaldırma
Bir uygulama örneği artık gerekli olmadığında, kalıcı olarak adını kullanarak kaldırabilirsiniz [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet'i. [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) tüm hizmet durumunu da, kalıcı olarak kaldırma uygulamaya ait tüm hizmetleri otomatik olarak kaldırır. 

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

## <a name="unregister-an-application-type"></a>Bir uygulama türünün kaydını silmek
Belirli bir uygulama türü sürümü artık gerekli değilse, uygulama türünü kullanarak kaydını [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet'i. Kullanılmayan uygulama türleri kaydı, görüntü deposu tarafından uygulama türü dosyaları kaldırarak kullanılan depolama alanı serbest bırakır. Görüntü deposu için kopyalama kullandıysanız bir uygulama türünün kaydını görüntü deposu geçici konuma kopyalanır uygulama paketi kaldırmaz. Uygulama türü, uygulama karşı örneği oluşturulur ve uygulama yükseltmeleri, Hayır başvurduğunuzdan sürece kaydı olabilir.

Çalıştırma [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kümede kayıtlı uygulama türlerini görmek için:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Çalıştırma [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) belirli uygulama türünün kaydını silmek için:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="troubleshooting"></a>Sorun Giderme
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>İçin bir Imagestoreconnectionstring kopyalama ServiceFabricApplicationPackage sorar
Service Fabric SDK'sı ortamı zaten ayarlanmış doğru varsayılan değerleri olmalıdır. Ancak, gerekirse Imagestoreconnectionstring tüm komutlar için Service Fabric kümesi kullanan değer ile eşleşmelidir. Küme bildiriminde Imagestoreconnectionstring bulabilirsiniz kullanarak [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) ve Get-ImageStoreConnectionStringFromClusterManifest komutları:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

**Get-ImageStoreConnectionStringFromClusterManifest** cmdlet'i, Service Fabric SDK PowerShell modülünü parçası olan resim depolama bağlantı dizesini almak için kullanılır.  SDK modülü içeri aktarmak için çalıştırın:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Imagestoreconnectionstring küme bildiriminde bulunur:

```xml
<ClusterManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.

### <a name="deploy-large-application-package"></a>Büyük uygulama paketini dağıtma
Sorun: [Kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) büyük uygulama paketi (sipariş GB) için zaman aşımına uğrar.
Deneyin:
- İçin daha büyük bir zaman belirtmek [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutunu `TimeoutSec` parametresi. Varsayılan olarak, zaman aşımını 30 dakikadır.
- Küme ve kaynak makine arasında ağ bağlantısını denetleyin. Bağlantısının yavaş olması daha iyi bir ağ bağlantısı ile bir makineyi kullanmayı düşünün.
İstemci makine kümesinden başka bir bölgede ise, bir istemci makine olarak aynı veya daha yakın bir bölgede kullanarak göz önünde bulundurun.
- Dış azaltma karşılaşırsınız olmadığını kontrol edin. Örneğin, görüntü deposu azure depolamanızı kullanmak için yapılandırıldığında, karşıya yüklemeyi azaltma.

Sorun: Başarılı bir şekilde tamamlandığını paketini karşıya yükleyin, ancak [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) zaman aşımına uğrar. Deneyin:
- [Paket sıkıştırma](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalamadan önce.
Sıkıştırma boyutunu azaltır ve hangi sırayla trafik miktarını azaltır ve bu Service Fabric çalışma dosyaları sayısı gerçekleştirmeniz gerekir. Karşıya yükleme işlemi (özellikle sıkıştırma zaman eklerseniz) daha yavaş olabilir, ancak kayıt ve kaydını uygulama türü daha hızlı.
- İçin daha büyük bir zaman belirtmek [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) ile `TimeoutSec` parametresi.
- Belirtin `Async` için geçiş [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). Komut, kümeyi komutu kabul eder ve uygulama türünün kaydını zaman uyumsuz olarak devam ettiğinde döndürür. Bu nedenle, bu durumda daha yüksek bir zaman aşımını belirtmek için gerek yoktur. [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komut tüm başarıyla kayıtlı uygulama türü sürümleri ve kayıt durumlarını listeler. Kayıt tamamlandığında belirlemek için bu komutu kullanabilirsiniz.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Çok sayıda dosya içeren uygulama paketini dağıtma
Sorun: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) zaman aşımına uğraması için uygulama paketi (binlerce siparişi) çok sayıda dosya içeren.
Deneyin:
- [Paket sıkıştırma](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalamadan önce. Sıkıştırma dosyalarının sayısını azaltır.
- İçin daha büyük bir zaman belirtmek [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) ile `TimeoutSec` parametresi.
- Belirtin `Async` için geçiş [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). Komut, kümeyi komutu kabul eder ve uygulama türünün kaydını zaman uyumsuz olarak devam ettiğinde döndürür.
Bu nedenle, bu durumda daha yüksek bir zaman aşımını belirtmek için gerek yoktur. [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komut tüm başarıyla kayıtlı uygulama türü sürümleri ve kayıt durumlarını listeler. Kayıt tamamlandığında belirlemek için bu komutu kullanabilirsiniz.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları paketleme](service-fabric-package-apps.md)

[Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md)

[Service Fabric sistem durumu giriş](service-fabric-health-introduction.md)

[Tanılama ve sorun giderme bir Service Fabric hizmeti](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Bir Service Fabric uygulamasında model](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md
