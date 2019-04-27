---
title: Bir Azure hizmeti paketi Fabric uygulama | Microsoft Docs
description: Bir kümeye dağıtmadan önce bir Service Fabric uygulama paketini nasıl.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: mani-ramaswamy
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: atsenthi
ms.openlocfilehash: b8e66a9d5bba0c48f15b1ccd3f2d47e5405db792
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60718379"
---
# <a name="package-an-application"></a>Uygulamaları paketleme

Bu makalede bir Service Fabric uygulaması paketleme ve dağıtım için hazır olun.

## <a name="package-layout"></a>Paket düzeni

Uygulama bildirimi, bir veya daha fazla hizmet bildirimleri ve diğer gerekli paket dosyaları, bir Service Fabric kümesine dağıtım için belirli bir düzende düzenlenmelidir. Bu makaledeki örnek bildirimleri içinde aşağıdaki dizin yapısını düzenlenmesine olanak gerekir:

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

Klasörler eşleşecek şekilde adlandırılır **adı** karşılık gelen her öğenin öznitelikleri. Örneğin, hizmet bildirimi adlara sahip iki kod paketleri içeriyorsa **MyCodeA** ve **MyCodeB**, sonra da her kod paketi için gerekli ikili dosyaları aynı ada sahip iki klasör içerir.

## <a name="use-setupentrypoint"></a>SetupEntryPoint kullanın

Kullanma için tipik senaryoları **SetupEntryPoint** ne zaman hizmeti başlamadan önce bir yürütülebilir dosyayı çalıştırmak için gereken veya yükseltilmiş ayrıcalıklarla bir işlemi gerçekleştirmek için ihtiyacınız. Örneğin:

* Ayarlama ve hizmet yürütülebilir gereken ortam değişkenlerini başlatılıyor. Yalnızca Service Fabric programlama modelleri yazılan yürütülebilir dosyalar için sınırlı değildir. Örneğin, bir node.js uygulaması dağıtmak için yapılandırılmış bazı ortam değişkenlerini npm.exe gerekir.
* Erişim denetimi, güvenlik sertifikalarını yükleyerek ayarlama.

Yapılandırma hakkında daha fazla bilgi için **SetupEntryPoint**, bkz: [Hizmet Kurulumu giriş noktası için ilke yapılandırma](service-fabric-application-runas-security.md)

<a id="Package-App"></a>

## <a name="configure"></a>Yapılandırma

### <a name="build-a-package-by-using-visual-studio"></a>Visual Studio kullanarak bir paket oluşturun

Uygulamanızı oluşturmak için Visual Studio 2015 kullanıyorsanız, yukarıda açıklanan düzenini eşleşen bir paket otomatik olarak oluşturmak için paketi komutu kullanabilirsiniz.

Bir paketi oluşturmak için Çözüm Gezgini'nde uygulama projesine sağ tıklayın ve aşağıda gösterildiği gibi paket komutu seçin:

![Visual Studio ile bir uygulama paketleme][vs-package-command]

Paketleme işlemi tamamlandığında, pakette konumunu bulabilirsiniz **çıkış** penceresi. Dağıttığınızda veya Visual Studio'da uygulamanızın hatalarını paketleme adımı otomatik olarak gerçekleşir.

### <a name="build-a-package-by-command-line"></a>Komut satırı tarafından bir paket oluşturun

Programlı olarak kullanarak uygulama paketlemek mümkündür `msbuild.exe`. Çıkış aynı olacak şekilde bileşenler, Visual Studio, çalışıyor.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Test paketi

Yerel olarak PowerShell üzerinden paket yapısı kullanarak doğrulayabilirsiniz [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) komutu.
Bu komut için bildirimi ayrıştırma sorunları denetler ve tüm başvurularını doğrulayın. Bu komut yalnızca paketteki dosyaları ve dizinleri yapısal doğruluğunu doğrular.
Herhangi bir kod veya veri paket içeriğinin tüm gerekli dosyaları mevcut olduğunu kontrol ötesinde doğrulamaz.

```powershell
Test-ServiceFabricApplicationPackage .\MyApplicationType
```

```Output
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Bu hatayı gösteren *MySetup.bat* hizmet bildiriminde başvurulan dosya **SetupEntryPoint** kod paketi eksik. Eksik dosya eklendikten sonra uygulama doğrulama geçer:

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

```powershell
Test-ServiceFabricApplicationPackage .\MyApplicationType
```

```Output
True
```

Uygulamanızın varsa [uygulama parametreleri](service-fabric-manage-multiple-environment-app-configuration.md) tanımlanan, bunları geçirebilirsiniz [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) uygun doğrulama için.

Uygulamanın dağıtılacağı küme biliyorsanız, geçirdiğiniz önerilir `ImageStoreConnectionString` parametresi. Bu durumda, paket, ayrıca uygulamanın kümede zaten çalışmakta olan önceki sürümlerini karşılaştırılarak doğrulanır. Örneğin, bir paket olup aynı sürümle doğrulama algılayabilir ancak farklı içerik zaten dağıtıldı.  

Uygulama düzgün şekilde paketlenmiş ve doğrulama başarılı sonra daha hızlı dağıtım işlemleri için paketi sıkıştırılıyor göz önünde bulundurun.

## <a name="compress-a-package"></a>Bir paket Sıkıştır

Bir paket büyük veya çok sayıda dosya sahip olduğunda, daha hızlı dağıtım için sıkıştırabilirsiniz. Sıkıştırma, paket boyutu ve dosya sayısını azaltır.
Bir sıkıştırılmış uygulama paketi için [uygulama paketini karşıya](service-fabric-deploy-remove-applications.md#upload-the-application-package) uzun sürebilir sıkıştırılmamış paket karşıya yükleme için özellikle sıkıştırma kopyalama işleminin bir parçası olarak yapıldıysa karşılaştırılan. Sıkıştırmayla [kaydetme](service-fabric-deploy-remove-applications.md#register-the-application-package) ve [kayıt uygulama türünü](service-fabric-deploy-remove-applications.md#unregister-an-application-type) daha hızlıdır.

Sıkıştırılmış ve sıkıştırılmamış paketleri için aynı dağıtım mekanizmadır. Sıkıştırılmış paket ise, bu nedenle küme görüntü deposunda depolanır ve uygulamayı çalıştırmadan önce düğümde sıkıştırılmamış.
Sıkıştırma, geçerli bir Service Fabric paketi sıkıştırılmış sürümüyle değiştirir. Klasörde yazma izni izin vermeniz gerekir. Sıkıştırma zaten sıkıştırılmış bir paketi üzerinde çalışan herhangi bir değişiklik verir.

Powershell komutunu çalıştırarak bir paket sıkıştırabilirsiniz [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) ile `CompressPackage` geçin. Aynı paketi sıkıştırmasını kaldırma komutu `UncompressPackage` geçin.

Aşağıdaki komut, görüntü deposuna kopyalamadan paket sıkıştırır. Sıkıştırılmış bir paketi kullanarak gerektiği gibi bir veya daha fazla Service Fabric kümeleri için kopyalayabilirsiniz [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) olmadan `SkipCopy` bayrağı.
Paket artık sıkıştırılmış dosyaları içeren `code`, `config`, ve `data` paketleri. İç birçok işlem için gerekli olduğu uygulama bildiriminin ve hizmet bildirimleri, daraltılmış değil. İçin örnek, paket paylaşımı, uygulama türü adı ve sürümü ayıklama bildirimleri erişmek belirli tüm doğrulamaları gerekir. Bildirimleri sıkıştırma, bu işlemleri verimli hale getirir.

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Alternatif olarak, sıkıştırma ve paket ile kopyalama [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) tek bir adımda.
Paket büyükse, hem paket sıkıştırma hem de Küme yükleme için zaman tanıyın yeterince yüksek bir zaman aşımı sağlar.

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Dahili olarak, Service Fabric uygulama paketlerini doğrulama için sağlama toplamları hesaplar. Sıkıştırma kullanırken, sağlama toplamları sıkıştırılmış her paket sürümleri üzerinde hesaplanır. Aynı uygulama paketinden yeni bir zip oluşturma farklı sağlama toplamları oluşturur. Doğrulama hatalarını önlemek için [fark sağlama](service-fabric-application-upgrade-advanced.md). Bu seçenek belirtilmişse, yeni sürümde değişmeden paketleri dahil değildir. Bunun yerine, bunları yeni hizmet bildiriminden bir doğrudan başvuru.

Fark sağlama seçeneği değil ve paketleri içermelidir, yeni sürümleri için oluşturmak `code`, `config`, ve `data` sağlama toplamı eşleşmezliği önlemek için paketler. Sıkıştırılmış bir paketi kullanıldığında, önceki sürümü sıkıştırma veya kullanıp kullanmadığını bağımsız olarak, yeni sürümleri değişmeden paketleri için oluşturma gereklidir.

Paket artık doğru şekilde paketlenmiş doğrulandı ve için hazır olması (gerekirse), sıkıştırılmış [dağıtım](service-fabric-deploy-remove-applications.md) bir veya daha fazla Service Fabric kümeleri için.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Visual Studio kullanarak dağıtılırken paketleri Sıkıştır

Dağıtım paketleri ekleyerek sıkıştırmak için Visual Studio bildirebilirsiniz `CopyPackageParameters` öğesi yayımlama profilini ve kümesi `CompressPackage` özniteliğini `true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="create-an-sfpkg"></a>Bir sfpkg oluşturma

Service Fabric, 6.1 sürümünden itibaren bir dış depodan sağlanmasına olanak tanır.
Bu seçenek ile görüntü deposuna kopyalamak uygulama paketi yok. Bunun yerine, oluşturabileceğiniz bir `sfpkg` ve bir dış depoya karşıya yükleyin ve ardından indirme URI'si sağlanırken Service Fabric'e sağlayın. Birden fazla küme için aynı paket sağlanabilir. Dış depodan sağlama, paket her kümeye kopyalamak için gerektiği zaman tasarrufu sağlar.

`sfpkg` İlk uygulama paketini içeren ve ".sfpkg" uzantısına sahip bir zip dosyasıdır.
Zip içinde uygulama paketi sıkıştırılmış veya sıkıştırılmamış. Uygulama paketi zip içinde sıkıştırmasını olarak yapılır kod, yapılandırma ve veri paket düzeyinde [daha önce bahsedilen](service-fabric-package-apps.md#compress-a-package).

Oluşturmak için bir `sfpkg`, sıkıştırılmış özgün uygulama paketini içeren bir klasör ile başlayın. Ardından, ".sfpkg" uzantılı bir klasör zip için herhangi bir hizmet kullanın. Örneğin, [ZipFile.CreateFromDirectory](https://msdn.microsoft.com/library/hh485721(v=vs.110).aspx).

```csharp
ZipFile.CreateFromDirectory(appPackageDirectoryPath, sfpkgFilePath);
```

`sfpkg` Dış depolama, Service Fabric dışında bant dışı karşıya yüklenmelidir. Dış depo, bir REST http veya https uç noktasını kullanıma sunar herhangi deposu olabilir. Sağlama işlemi sırasında Service Fabric indirmek için bir Al işlemi yürütür `sfpkg` mağaza paket için okuma erişimine izin vermeniz gerekir, böylece uygulama paketi.

Paket sağlamak için indirme URI'si ve uygulama türü bilgilerini gerektiren dış sağlama kullanın.

>[!NOTE]
> Görüntü deposu göreli yola göre sağlama şu anda desteklemiyor `sfpkg` dosyaları. Bu nedenle, `sfpkg` görüntü deposuna kopyalanmaması.

## <a name="next-steps"></a>Sonraki adımlar

[Dağıtma ve uygulamaları kaldırma] [ 10] uygulama örneklerini yönetmek için PowerShell'i kullanmayı açıklar

[Birden çok ortam için uygulama parametrelerini yönetme] [ 11] parametreleri ve farklı uygulama örneklerinin için ortam değişkenlerini nasıl yapılandırılacağını açıklar.

[Uygulamanıza yönelik güvenlik ilkeleri yapılandırma] [ 12] erişimi kısıtlamak için güvenlik ilkeleri altındaki hizmetleri çalıştırma işlemi açıklanır.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md