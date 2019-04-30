---
title: PowerShell kullanarak Service Fabric uygulaması yükseltme | Microsoft Docs
description: Bu makalede, Service Fabric uygulaması dağıtma, kod değiştirme ve PowerShell kullanarak yükseltme sunmaya yönelik deneyimi gösterilmektedir.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: 9bc75748-96b0-49ca-8d8a-41fe08398f25
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: c8f56897380bc3108cb979d9d15e7dbd0a329064
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614383"
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>PowerShell kullanarak Service Fabric uygulaması yükseltme
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

En sık kullanılan ve önerilen yükseltme izlenen sıralı yükseltmesini bir yaklaşımdır.  Azure Service Fabric, bir sistem durumu ilkeleri kümesi temel alınarak yükseltilmekte olan uygulama durumunu izler. Bir güncelleme etki alanı (UD) yükseltildikten sonra Service Fabric uygulama durumunu değerlendirir ve bir sonraki güncelleştirme etki alanına geçer veya sistem ilkelerine bağlı olarak yükseltme başarısız olur.

İzlenen uygulama yükseltmesi, yönetilen veya yerel API'ler, PowerShell, Azure CLI, Java veya REST kullanarak gerçekleştirilebilir. Visual Studio kullanarak yükseltme gerçekleştirme hakkında yönergeler için bkz. [uygulamanızı Visual Studio kullanarak yükseltme](service-fabric-application-upgrade-tutorial.md).

Service Fabric izlenen sıralı yükseltmeler, uygulama Yöneticisi uygulama sağlıklı olup olmadığını belirlemek için Service Fabric kullanan sistem durumu değerlendirme ilkesi yapılandırabilirsiniz. Ayrıca, yönetici (Otomatik geri alma işlemi yaptığınızda örneğin.) sistem durumu değerlendirmesi başarısız olduğunda gerçekleştirilecek eylemi yapılandırabilir Bu bölümde bir PowerShell kullanan bir SDK örnekleri için izlenen bir yükseltme kılavuzluk eder. 

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>1. Adım: Derleme ve görsel nesneler örneği dağıtma
Derleme ve uygulamayı uygulama projesine sağ tıklayarak yayımlama **VisualObjectsApplication,** seçerek **Yayımla** komutu.  Daha fazla bilgi için [Service Fabric uygulaması yükseltme Öğreticisi](service-fabric-application-upgrade-tutorial.md).  Alternatif olarak, uygulamanızı dağıtmak için PowerShell kullanabilirsiniz.

> [!NOTE]
> PowerShell'de Service Fabric komutları kullanılabilir önce ilk kullanarak kümeye bağlanmak ihtiyacınız `Connect-ServiceFabricCluster` cmdlet'i. Benzer şekilde, küme zaten yerel makinenizde ayarlandığını gösterdiğinde, varsayılır. Makaleye bakın [Service Fabric geliştirme ortamınızı ayarlama](service-fabric-get-started.md).
> 
> 

Visual Studio projeyi oluşturduktan sonra PowerShell komutu kullanabilirsiniz [kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage) ImageStore için uygulama paketi kopyalamak için. Uygulama paketi yerel olarak doğrulamak istediğiniz kullanırsanız [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage) cmdlet'i. Service Fabric çalışma zamanı kullanmak için uygulamayı kaydetmek için sonraki adımdır [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype) cmdlet'i. Uygulamanın bir örneğini kullanarak başlatmak için aşağıdaki adımdır [yeni ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet'i.  Bu üç adımı kullanarak benzer **Dağıt** Visual Studio'daki menü öğesi.  Sağlama tamamlandıktan sonra kullanılan kaynaklar azaltmak için görüntü deposundan kopyalanan uygulama paketi oluşturan temizlemelidir.  Uygulama türü artık gerekli değilse, aynı nedenden dolayı kaydı olmalıdır. Bkz: [PowerShell kullanarak dağıtma ve Kaldır uygulamaları](service-fabric-application-upgrade-tutorial-powershell.md) daha fazla bilgi için.

Şimdi, kullanabileceğiniz [Service Fabric Explorer'ı, küme ve uygulamayı görüntülemek için](service-fabric-visualizing-your-cluster.md). Uygulama bir web hizmeti için Internet Explorer'da yazarak gezinilebilir sahip [ http://localhost:8081/visualobjects ](http://localhost:8081/visualobjects) adres çubuğundaki.  Ekranda Dolaşma bazı kayan görsel nesneler görmeniz gerekir.  Ayrıca, kullanabileceğiniz [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) uygulama durumunu denetlemek için.

## <a name="step-2-update-the-visual-objects-sample"></a>2. Adım: Görsel nesneler örnek güncelleştirme
Adım 1'de dağıtılmış sürümle görsel nesneler değil döndürme fark edebilirsiniz. Şimdi bir görsel nesneler burada döndürmek için bu uygulamayı yükseltin.

VisualObjects çözümünde VisualObjects.ActorService projeyi seçin ve StatefulVisualObjectActor.cs dosyasını açın. Bu dosyanın içindeki yöntemine gidin `MoveObject`, açıklama `this.State.Move()`ve açıklama durumundan çıkarın `this.State.Move(true)`. Hizmet yükseltildikten sonra bu değişiklik nesnesini döndürür.

Biz de güncelleştirmeniz gerekiyor *ServiceManifest.xml* (altında PackageRoot) dosyası proje **VisualObjects.ActorService**. Güncelleştirme *CodePackage* ve hizmet sürümünden 2.0 ve karşılık gelen satır *ServiceManifest.xml* dosya.
Visual Studio kullanabileceğiniz *bildirim dosyaları düzenleme* bildirim dosyası değişiklik yapmak için çözüme sağ tıklayın, sonra seçeneği.

Değişiklikler yapıldıktan sonra bildirimin aşağıdakine benzer olması gerekir (vurgulanan bölümleri değişiklikleri göster):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Artık *ApplicationManifest.xml* dosyası (altında bulunan **VisualObjects** altındaki proje **VisualObjects** çözüm) 2.0sürümünegüncellenir**VisualObjects.ActorService** proje. Ayrıca, uygulama sürümü için 2.0.0.0 1.0.0.0 ile güncelleştirilir. *ApplicationManifest.xml* aşağıdaki kod parçacığı gibi görünmelidir:

```xml
<ApplicationManifestxmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```

Artık, seçerek proje oluşturun. yalnızca **ActorService** proje ve ardından sağ tıklayıp **derleme** Visual Studio'da seçeneği. Seçerseniz **tümünü yeniden derle**, nedeniyle kod değişmiş tüm projelerde sürümleri güncelleştirmeniz gerekir. Sonra şimdi sağ tıklayarak güncelleştirilmiş uygulama paketini ***VisualObjectsApplication***, Service Fabric menüsünü seçip **paket**. Bu eylem, dağıtılabilir bir uygulama paketi oluşturur.  Güncelleştirilmiş uygulamanız dağıtılmaya hazırdır.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>3. Adım:  Sistem durumu ilkeleri hakkında karar verin ve yükseltme parametreleri
İle kendinizi alıştırın [uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) ve [yükseltme işlemi](service-fabric-application-upgrade.md) çeşitli yükseltme parametreleri, zaman aşımları ve sistem durumu ölçütü uygulanan iyi bir anlayış edinmek için. Bu kılavuz için hizmet sistem durumu değerlendirme ölçütü Varsayılana Ayarla (ve önerilen) tüm hizmetlerin ve örnek gerektiği anlamına gelir değerleri *sağlıklı* yükseltmeden sonra.  

Bununla birlikte, şimdi artırmak *HealthCheckStableDuration* 180 saniye (Hizmetleri sonraki güncelleştirme etki alanına yükseltmeye devam etmeden önce en az 120 saniye için iyi durumda olacak şekilde).  Ayrıca ayarlayalım *UpgradeDomainTimeout* 1200 saniye olacak şekilde ve *UpgradeTimeout* 3000 saniye olacak şekilde.

Son olarak, aynı zamanda ayarlayalım *UpgradeFailureAction* geri alınacak. Bu seçenek, yükseltme sırasında herhangi bir sorun karşılaşırsa, uygulamayı önceki sürüme geri almak Service Fabric gerektirir. Bu nedenle, aşağıdaki parametreleri yükseltmesinde (4. adım) başlatma sırasında belirtilir:

FailureAction geri alma =

HealthCheckStableDurationSec = 180

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout 3000 =

## <a name="step-4-prepare-application-for-upgrade"></a>4. Adım: Uygulama yükseltme için hazırlama
Artık uygulama oluşturulmuş ve yükseltilecek hazır olacak. Yönetici olarak çalıştırıp türü bir PowerShell penceresi açın, [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), uygulama türünü 1.0.0.0 olduğunu bilmenizi vermemelisiniz **VisualObjects** dağıtılan.  

Uygulama paketi, Service Fabric SDK'sı - sıkıştırılmamış burada şu göreli yolun altında depolanan *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Uygulama paketi depolandığı bu dizinde bir "Paket" klasörünü bulmanız gerekir. En son sürüme (yolları de uygun şekilde değiştirmeniz gerekebilir) olduğundan emin olmak için zaman damgaları denetleyin.

Şimdi güncelleştirilmiş uygulama paketini (uygulama paketlerini depolandığı Service Fabric tarafından) Service Fabric ImageStore şimdi kopyalayın. Parametre *ApplicationPackagePathInImageStore* burada bulabildiği uygulama paketi Service Fabric bildirir. Biz güncelleştirilmiş uygulamayı koymak "VisualObjects\_V2" aşağıdaki komutla (uygun şekilde yeniden yolları değiştirmek gerekebilir).

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Bu uygulamayı kullanarak gerçekleştirilebilir Service Fabric ile kaydetmek için sonraki adımdır [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) komutu:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Önceki komutu başarısız olursa, tüm hizmetleri yeniden ihtiyacınız olasıdır. 2. adımda bahsedildiği gibi Web hizmeti sürümü de güncelleştirmeniz gerekiyor olabilir.

Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmak önerilir.  Uygulama paketleri görüntü deposundan silme, sistem kaynakları serbest bırakır.  Kullanılmayan uygulama paketleri tutma disk depolama alanını kullanan ve uygulama performası sorunlarını için yol açar.

```powershell
Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore "VisualObjects\_V2" -ImageStoreConnectionString fabric:ImageStore
```

## <a name="step-5-start-the-application-upgrade"></a>5. Adım: Uygulama yükseltmesi Başlat
Şimdi biz uygulama yükseltmesi kullanmaya başlamaya hazırsınız [başlangıç ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) komutu:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


İçinde açıklandığı uygulama adı aynı olan *ApplicationManifest.xml* dosya. Service Fabric, hangi uygulamanın yükseltilmeyen tanımlamak için bu adı kullanır. Zaman aşımı çok kısa olacak şekilde ayarlarsanız, sorunu bildiren bir hata iletisi karşılaşabilirsiniz. Sorun giderme bölümüne bakın veya zaman aşımları artırın.

Service Fabric Explorer kullanarak artık uygulama yükseltme işlemi olarak izleyebilir veya kullanarak [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) PowerShell komutunu: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

Birkaç dakika içinde önceki bir PowerShell komutu kullanılarak aldığınız durumu durum tüm güncelleştirme etki alanları yükseltilen (tamamlanmış). Ve Tarayıcı pencerenizde görsel nesneler döndürme başlattığınızı bulmanız gerekir!

Sürüm 3 sürüm 2 veya sürüm 1 bir alıştırma olarak 2 sürümüne yükseltme deneyebilirsiniz. Sürüm 2 ' sürüm 1 taşıma yükseltme kabul edilir. Zaman aşımları ve kendiniz bunlarla ilgili bilgi sahibi olmak için sistem durumu ilkeleri yürütün. Bir Azure kümesine dağıtırken parametreleri uygun şekilde ayarlanmış olması gerekir. Zaman aşımlarını ölçülü ayarlamak uygundur.

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak uygulamanızı yükseltmek](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Nasıl kullanılacağını öğrenerek, uygulama yükseltmeleri uyumlu olma [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).

