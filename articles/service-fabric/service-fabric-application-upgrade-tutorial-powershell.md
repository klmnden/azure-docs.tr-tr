---
title: "PowerShell kullanarak Service Fabric uygulaması yükseltme | Microsoft Docs"
description: "Bu makalede bir Service Fabric uygulaması dağıtma, kodunu değiştirme ve PowerShell kullanarak yükseltme çalışırken deneyimi anlatılmaktadır."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 9bc75748-96b0-49ca-8d8a-41fe08398f25
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 0306a219112a14121fd881a7cc52d58597a073a2
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>PowerShell kullanarak Service Fabric uygulama yükseltme
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

En sık kullanılan ve önerilen yükseltme izlenen yükseltme yaklaşımdır.  Azure Service Fabric sistem durumu ilkeleri kümesine göre yükseltilen uygulama durumunu izler. Bir güncelleştirme etki alanı (UD) yükseltildikten sonra Service Fabric uygulama durumunu değerlendirir ve bir sonraki güncelleştirme etki alanına devam eder veya sistem durumu ilkeleri bağlı olarak yükseltme başarısız olur.

İzlenen uygulama yükseltme yönetilen veya özgün API'leri, PowerShell veya REST kullanılarak gerçekleştirilebilir. Visual Studio kullanarak bir yükseltme gerçekleştirme hakkında yönergeler için bkz: [Visual Studio kullanarak uygulamanızı yükseltme](service-fabric-application-upgrade-tutorial.md).

Service Fabric izlenen kesinti olmadan yükseltme ile Uygulama Yöneticisi uygulama sağlıklı olup olmadığını belirlemek için Service Fabric kullanan sistem durumu değerlendirme ilkesi yapılandırabilirsiniz. Ayrıca, yönetici (Otomatik geri alma işleminden örneğin.) sistem durumu değerlendirmesi başarısız olduğunda gerçekleştirilecek eylem yapılandırabilirsiniz Bu bölümde bir PowerShell kullanan SDK örnekleri için izlenen bir yükseltme anlatılmaktadır. Aşağıdaki Microsoft Virtual Academy video uygulama yükseltme size da yardımcı olur: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=OrHJH66yC_6406218965">
<img src="./media/service-fabric-application-upgrade-tutorial-powershell/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>1. adım: Derleme ve görsel nesneler örnek dağıtma
Yapı ve uygulama projesine sağ tıklayarak uygulamayı yayımlama **VisualObjectsApplication,** ve seçerek **Yayımla** komutu.  Daha fazla bilgi için bkz: [Service Fabric uygulaması yükseltme Öğreticisi](service-fabric-application-upgrade-tutorial.md).  Alternatif olarak, uygulamanızı dağıtmak için PowerShell'i kullanabilirsiniz.

> [!NOTE]
> Service Fabric komutlardan herhangi birini PowerShell'de kullanılabilir önce ilk kullanarak kümeye bağlanmak ihtiyacınız `Connect-ServiceFabricCluster` cmdlet'i. Benzer şekilde, küme zaten yerel makinenizde ayarlanmış olması gerektiğini varsayılır. Makaleyi bakın [, Service Fabric geliştirme ortamını ayarlama](service-fabric-get-started.md).
> 
> 

Visual Studio projesi oluşturduktan sonra PowerShell komutunu kullanabilirsiniz [kopya ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) uygulama paketi görüntü deposuna kopyalamak için. Uygulama paketi yerel olarak doğrulamak istiyorsanız, kullanmak [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) cmdlet'i. Service Fabric çalışma zamanı kullanmak için uygulamayı kaydetmek için sonraki adımdır [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) cmdlet'i. Uygulama örneğini kullanarak başlatmak için aşağıdaki adımdır [yeni ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet'i.  Bu üç adımı kullanmaya benzer **dağıtma** Visual Studio menü öğesi.  Sağlama tamamlandıktan sonra görüntü deposundan kopyalanan uygulama paketi kullanılan kaynaklar azaltmak için temizlemeniz.  Uygulama türü artık gerekli değilse, aynı nedenden dolayı kaydı olmalıdır. Bkz: [PowerShell kullanarak uygulamaları dağıtma ve Kaldır](service-fabric-application-upgrade-tutorial-powershell.md) daha fazla bilgi için.

Şimdi, kullanabileceğiniz [küme ve uygulamayı görüntülemek için Service Fabric Explorer](service-fabric-visualizing-your-cluster.md). Uygulama için Internet Explorer'da yazarak gittiğinizde bir web hizmeti olan [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) adres çubuğundaki.  Ekranda Dolaşma bazı kayan görsel nesneler görmeniz gerekir.  Ayrıca, kullanabileceğiniz [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) uygulama durumunu denetlemek için.

## <a name="step-2-update-the-visual-objects-sample"></a>2. adım: görsel nesneler örnek güncelleştir
Adım 1'de dağıtılan sürümüyle görsel nesneler değil döndürme fark edebilirsiniz. Şimdi bir burada görsel nesneler de döndürmek için bu uygulamayı yükseltin.

VisualObjects çözümünde VisualObjects.ActorService projesini seçin ve StatefulVisualObjectActor.cs dosyasını açın. Bu dosyanın içindeki yönteme gidin `MoveObject`, çıkışı açıklama `this.State.Move()`ve açıklama durumundan çıkarmanız `this.State.Move(true)`. Hizmet yükseltildikten sonra bu değişiklik nesnesini döndürür.

Ayrıca güncelleştirmek ihtiyacımız *ServiceManifest.xml* (altında PackageRoot) dosyası projenin **VisualObjects.ActorService**. Güncelleştirme *CodePackage* ve hizmet sürüm 2.0 ve karşılık gelen satırları *ServiceManifest.xml* dosya.
Visual Studio'yu kullanabilirsiniz *bildirim dosyaları düzenleme* bildirim dosyası değişiklik yapmak için çözüm üzerinde sağ tıklatın sonra seçeneği.

Değişiklikler yapıldıktan sonra bildirimi aşağıdaki gibi görünmelidir (vurgulanan bölümleri göster değişiklikleri):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Şimdi *ApplicationManifest.xml* dosyası (altında bulunan **VisualObjects** altında proje **VisualObjects** çözüm) 2.0sürümünegüncelleştirilmiş**VisualObjects.ActorService** projesi. Ayrıca, uygulama sürümü için 2.0.0.0 1.0.0.0 ile güncelleştirilir. *ApplicationManifest.xml* aşağıdaki kod parçacığını gibi görünmelidir:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```

Şimdi, seçerek Projeyi derlemek yalnızca **ActorService** proje ve ardından sağ tıklayıp seçerek **yapı** Visual Studio'daki seçeneği. Seçerseniz **tüm yeniden**, nedeniyle kodu değişmiş sürümleri tüm projeleri için güncelleştirmeniz gerekir. Ardından, üzerinde sağ tıklayarak güncelleştirilmiş uygulamayı şimdi paketini ***VisualObjectsApplication***, Service Fabric menü seçilerek ve **paket**. Bu eylem, dağıtılabilir bir uygulama paketi oluşturur.  Güncelleştirilmiş uygulamanız dağıtılmaya hazırdır.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>3. adım: sistem durumu ilkeleri karar verin ve yükseltme parametreleri
İle öğrenmeniz [uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) ve [yükseltme işlemi](service-fabric-application-upgrade.md) çeşitli yükseltme parametreleri, zaman aşımları ve sistem durumu ölçüt uygulanan iyi anlamış alınamıyor. Bu kılavuz için hizmet sistem durumu değerlendirme ölçüt varsayılan olarak ayarla (ve önerilen) tüm hizmetleri ve örnekleri gerektiği anlamına gelir değerleri *sağlıklı* yükselttikten sonra.  

Ancak, şimdi artırmanız *HealthCheckStableDuration* 60 saniye (Hizmetleri sonraki güncelleştirme etki alanına yükseltmeye devam etmeden önce en az 20 saniye için sağlıklı; böylece).  Şimdi de ayarlayın *UpgradeDomainTimeout* 1200 saniye olmasını ve *UpgradeTimeout* 3000 saniye olmalıdır.

Son olarak, şimdi de ayarlayın *UpgradeFailureAction* geri almak için. Bu seçenek, yükseltme sırasında herhangi bir sorunla karşılaştığında uygulamayı önceki sürüme geri almak Service Fabric gerektirir. Bu nedenle, aşağıdaki parametreleri yükseltmesinde (4. adım) başlatma sırasında belirtilir:

FailureAction geri alma =

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout 3000 =

## <a name="step-4-prepare-application-for-upgrade"></a>4. adım: Uygulama yükseltme için hazırlama
Şimdi uygulamayı yerleşik ve yükseltme hazır olur. Bir yöneticiyseniz ve türü bir PowerShell penceresi açarsanız [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), uygulama türü 1.0.0.0 olduğunu size bildirmek **VisualObjects** dağıtılan.  

Uygulama paketi Service Fabric SDK - sıkıştırılmamış burada aşağıdaki göreli yolu altında depolanır *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Uygulama paketi depolandığı bu dizinde bir "Paketi" klasörü bulmanız gerekir. Zaman damgaları (yollar uygun şekilde de değiştirmeniz gerekebilir) en son sürüme olduğundan emin olmak için kontrol edin.

Şimdi şimdi güncelleştirilmiş uygulama paketi (uygulama paketleri depolandığı Service Fabric tarafından) Service Fabric görüntü kopyalayın. Parametre *ApplicationPackagePathInImageStore* Service Fabric burada da bulabilir uygulama paketi bildirir. Biz güncelleştirilmiş uygulamayı yerleştirdiğiniz "VisualObjects\_V2" (yolları yeniden uygun şekilde değiştirin gerekebilir) aşağıdaki komutu.

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Bu uygulama ile gerçekleştirilebilir Service Fabric ile kaydetmek için sonraki adımdır [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) komutu:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Yukarıdaki komut başarılı değil, tüm hizmetleri yeniden gereksinim olasıdır. Adım 2'de belirtildiği gibi Web hizmeti sürümü de güncelleştirmeniz gerekebilir.

Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmanız önerilir.  Uygulama paketleri görüntü deposundan silme sistem kaynakları serbest bırakır.  Kullanılmayan uygulama paketleri tutma disk depolama alanı kullanır ve uygulama performans sorunlarına yol açar.

```powershell
Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore "VisualObjects\_V2" -ImageStoreConnectionString fabric:ImageStore
```

## <a name="step-5-start-the-application-upgrade"></a>5. adım: Uygulama yükseltme Başlat
Şimdi, biz kullanarak uygulama yükseltme işlemini başlatmak için hazırsınız [başlangıç ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) komutu:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Bölümünde açıklanan uygulama adı aynı olan *ApplicationManifest.xml* dosya. Service Fabric, hangi uygulamanın yükseltilir tanımlamak için bu ad kullanır. Zaman aşımı çok kısa olacak şekilde ayarlarsanız sorunu bildiren bir hata iletisi karşılaşabilirsiniz. Sorun giderme bölümüne bakın veya zaman aşımları artırın.

Service Fabric Explorer kullanarak şimdi uygulama yükseltme devam eder, olarak izleyebilir veya kullanarak [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) PowerShell komutunu: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

Önceki PowerShell komutunu kullanarak aldığınız durumu birkaç dakika içinde durum tüm güncelleştirme etki alanları yükseltilen (tamamlandı). Ve Tarayıcı pencerenizde visual nesneleri döndürme başlatılmış olduğunu bulmanız gerekir!

Sürüm 3 sürüm 2 veya sürüm 1 bir alıştırma olarak 2 sürümüne yükseltme deneyebilirsiniz. Sürüm 2 ' sürüm 1 taşıma yükseltme kabul edilir. Zaman aşımları ve kendiniz bunlarla bilgi sahibi olmak için sistem durumu ilkeleri ile yürütün. Bir Azure kümeye dağıtırken parametreleri uygun şekilde ayarlanmış olması gerekir. Zaman aşımları ölçülü ayarlamak uygundur.

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak uygulamanızı yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Uygulama yükseltme nasıl kullanılacağını öğrenerek uyumlu hale getirmek [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Gelişmiş işlevselliği başvurarak uygulamanızı yükseltirken kullanmayı öğrenin [konuları Gelişmiş](service-fabric-application-upgrade-advanced.md).

Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).

