---
title: Bir Azure hizmet paketini doku uygulama | Microsoft Docs
description: "Bir kümeye dağıtmadan önce bir Service Fabric uygulaması için nasıl."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 486a27d7ca576c8fe1552c02eb24ece6b8bb2ba8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="package-an-application"></a>Bir uygulama paketi
Bu makale, Service Fabric Uygulama paketleme ve dağıtım için hazır hale getirmek açıklamaktadır.

## <a name="package-layout"></a>Paket düzeni
Uygulama bildirimi, bir veya daha fazla hizmet bildirimleri ve diğer gerekli paket dosyalarını Service Fabric kümesi içine dağıtımı için belirli bir düzende düzenlenmiş olması gerekir. Bu makaledeki örnek bildirimleri aşağıdaki dizin yapısını düzenlenmesine olanak gerekir:

```
PS D:\temp> tree /f .\MyApplicationType

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

Klasörleri adlı eşleştirmek için **adı** karşılık gelen her öğenin öznitelikleri. Örneğin, hizmet bildirimi adlarıyla iki farklı kod paketi içeriyorsa **MyCodeA** ve **MyCodeB**, sonra da aynı adı taşıyan iki klasör her kod paketi için gerekli ikili dosyalarını içerir.

## <a name="use-setupentrypoint"></a>SetupEntryPoint kullanın
Kullanma için tipik senaryolar **SetupEntryPoint** zaman hizmeti başlamadan önce bir yürütülebilir dosyayı çalıştırmak için gereken veya yükseltilmiş ayrıcalıklarla bir işlem gerçekleştirmeniz gerekir. Örneğin:

* Ayarlama ve hizmeti yürütülebilir dosyası gerekli ortam değişkenleri başlatılıyor. Yalnızca Service Fabric programlama modeli yazılmış yürütülebilir dosyalar için sınırlı değildir. Örneğin, bir node.js uygulamasını dağıtmak için yapılandırılmış bazı ortam değişkenleri npm.exe gerekir.
* Güvenlik sertifikaları yükleyerek erişim denetimini ayarlama.

Nasıl yapılandırılacağı hakkında daha fazla bilgi için **SetupEntryPoint**, bkz: [hizmet Kurulum giriş noktası için ilkeyi yapılandırın](service-fabric-application-runas-security.md)

<a id="Package-App"></a>
## <a name="configure"></a>Yapılandırma
### <a name="build-a-package-by-using-visual-studio"></a>Visual Studio kullanarak bir paket oluşturun
Uygulamanızı oluşturmak için Visual Studio 2015 kullanıyorsanız, yukarıda açıklanan düzeni eşleşen bir paket otomatik olarak oluşturmak için paket komutunu kullanabilirsiniz.

Bir paketi oluşturmak için Çözüm Gezgini'nde uygulama projesine sağ tıklayın ve aşağıda gösterildiği gibi Paketi komutu seçin:

![Visual Studio ile bir uygulama paketleme][vs-package-command]

Paketleme tamamlandığında pakette konumunu bulabilirsiniz **çıkış** penceresi. Visual Studio uygulamanızda hata ayıklama veya dağıttığınızda paketleme adım otomatik olarak gerçekleşir.

### <a name="build-a-package-by-command-line"></a>Komut satırı tarafından bir paket oluşturun
Program aracılığıyla kullanarak uygulamanızı paketlemek mümkündür `msbuild.exe`. Çıktı aynı olacak şekilde başlık altında Visual Studio çalışıyor.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Paketi sınayın
Yerel olarak PowerShell aracılığıyla paket yapısı kullanarak doğrulayabilirsiniz [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) komutu.
Bu komut için bildirimi ayrıştırma sorunları denetler ve tüm başvuruları doğrulayın. Bu komut, yalnızca paketteki dosyaları ve dizinleri yapısal doğruluğunu doğrular.
Tüm gerekli dosyaların mevcut olduğunu denetleme ötesinde kod veya veri paket içeriğini hiçbirini doğrulamak değil.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Bu hatayı gösterir *MySetup.bat* hizmet bildiriminde başvurulan dosya **SetupEntryPoint** kod paketi eksik. Eksik dosya eklendikten sonra uygulama doğrulama geçirir:

```
PS D:\temp> tree /f .\MyApplicationType

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

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Uygulamanız varsa, [uygulama parametreleri](service-fabric-manage-multiple-environment-app-configuration.md) tanımlanan, bunları geçirebilirsiniz [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) uygun doğrulama için.

Uygulamanın dağıtılacağı küme biliyorsanız, geçirdiğiniz önerilir `ImageStoreConnectionString` parametresi. Bu durumda, paket ayrıca uygulamanın zaten kümede çalışan önceki sürümlerini doğrulanır. Örneğin, bir paket olup olmadığını aynı sürüme sahip doğrulama algılayabilir ancak farklı içerik zaten dağıtıldı.  

Uygulama doğru paketlenir ve doğrulama başarılı sonra değerlendirme sıkıştırma gerekirse boyutu ve dosya sayısını göre.

## <a name="compress-a-package"></a>Bir paket Sıkıştır
Bir paket çok büyük veya çok sayıda dosya sahip olduğunda, daha hızlı dağıtımı için sıkıştırın. Sıkıştırma, dosyaları ve paket boyutu sayısını azaltır.
Sıkıştırılmış uygulama paketi için [uygulama paketini karşıya](service-fabric-deploy-remove-applications.md#upload-the-application-package) daha uzun sürebilir sıkıştırılmamış paket (özel sıkıştırma zaman hesaba katıldığında değilse), karşıya yükleme için karşılaştırıldığında ancak [kaydetme](service-fabric-deploy-remove-applications.md#register-the-application-package) ve [kayıt uygulama türü](service-fabric-deploy-remove-applications.md#unregister-an-application-type) sıkıştırılmış uygulama paketi için daha hızlıdır.

Sıkıştırılmış ve sıkıştırılmamış paketler için aynı dağıtım mekanizmadır. Paket sıkıştırılmış ise, bu nedenle küme görüntü deposunda depolanır ve uygulamayı çalıştırmadan önce düğümde sıkıştırılmamış.
Sıkıştırma geçerli Service Fabric paketi sıkıştırılmış sürümüyle değiştirir. Klasör yazma izni izin vermeniz gerekir. Sıkıştırma zaten sıkıştırılmış bir paketi çalıştıran herhangi bir değişiklik verir.

Powershell komutunu çalıştırarak bir paket sıkıştırabilirsiniz [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) ile `CompressPackage` geçin. Aynı paketin sıkıştırmasını komutunu `UncompressPackage` geçin.

Aşağıdaki komutu, paketi görüntü deposuna kopyalama olmadan sıkıştırır. Sıkıştırılmış paketi kullanarak gerektiği gibi bir veya daha fazla Service Fabric kümesi için kopyalayabilirsiniz [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) olmadan `SkipCopy` bayrağı.
Paket sıkıştırılmış dosya için artık içerir `code`, `config`, ve `data` paketler. (Paylaşım, uygulama türü adı ve sürümü için ayıklama belirli doğrulamaları paketini gibi) çok sayıda dahili işlemleri için gerekli olduğu için uygulama bildirimi ve hizmet bildirimlerini, daraltılmış değil.
Bildirimleri sıkıştırma bu işlemleri verimli hale getirir.

```
PS D:\temp> tree /f .\MyApplicationType

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
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Alternatif olarak, sıkıştırma ve paketiyle kopyalama [kopya ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) tek bir adımda.
Paket büyükse, paket sıkıştırma ve küme için karşıya yükleme zamanını izin vermek için yeterince zaman aşımı sağlar.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Dahili olarak, Service Fabric sağlama toplamı doğrulaması için uygulama paketleri için hesaplar. Sıkıştırma kullanırken, sağlama her paket daraltılmış sürümlerinde hesaplanır.
Sıkıştırılmamış bir uygulama paketinizi kopyalanır ve sıkıştırma aynı paket için kullanmak istediğiniz, sürümleri değiştirmelisiniz `code`, `config`, ve `data` sağlama toplamı eşleşmezliği önlemek için paketler. Paketler, sürümünü değiştirme yerine, değişmeden varsa, kullanabileceğiniz [fark sağlama](service-fabric-application-upgrade-advanced.md). Bu seçenek ile değişmeden paket dahil etmeyin yerine hizmet bildirimden başvuru.

Benzer şekilde, paketin sıkıştırılmış bir sürümünü karşıya ve sıkıştırılmamış bir paketi kullanmak istiyorsanız, sağlama toplamı eşleşmezliği önlemek için sürümler güncelleştirmeniz gerekir.

Paket artık doğru paketlenmiş doğrulandı ve için hazır olması için (gerekirse), sıkıştırılmış [dağıtım](service-fabric-deploy-remove-applications.md) bir veya daha fazla Service Fabric kümesi için.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Visual Studio kullanarak dağıtırken paketleri Sıkıştır
Dağıtım paketleri ekleyerek sıkıştırmak için Visual Studio söyleyebilirsiniz `CopyPackageParameters` yayımlama profili ve kümesi öğesine `CompressPackage` özniteliğini `true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a>Sonraki adımlar
[Dağıtma ve uygulamaları kaldırma] [ 10] uygulama örnekleri yönetmek için PowerShell kullanmayı açıklar

[Birden çok ortamlar için uygulama parametreleri yönetme] [ 11] parametreleri ve farklı uygulama örnekleri için ortam değişkenleri nasıl yapılandırılacağını açıklar.

[Uygulamanız için güvenlik ilkelerini yapılandırmak] [ 12] erişimi kısıtlamak için güvenlik ilkeleri altında hizmetlerini çalıştırmak açıklar.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
