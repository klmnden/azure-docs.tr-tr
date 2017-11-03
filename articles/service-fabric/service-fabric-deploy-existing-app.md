---
title: "Azure Service Fabric varolan yürütülebilir bir dağıtma | Microsoft Docs"
description: "Bir Service Fabric kümesi dağıtılabilmesi amacıyla yürütülebilir, konuk olarak var olan bir uygulama paketi konusunda gözden geçirme"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: a8579c66cbfb0968a3659316aa5f03b798f4e332
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-guest-executable-to-service-fabric"></a>Service Fabric yürütülebilir Konuk dağıtma
Azure Service Fabric kod, Node.js, Java ya da C++ gibi herhangi bir türde bir hizmet olarak çalıştırabilirsiniz. Service Fabric bu tür hizmet Konuk yürütülebilir başvurur.

Konuk yürütülebilir dosyalar gibi durum bilgisi olmayan hizmetler Service Fabric tarafından kabul edilir. Sonuç olarak, bunlar kullanılabilirlik ve diğer ölçümleri temelinde, bir kümedeki düğümlere yerleştirilir. Bu makalede, paket ve Visual Studio veya komut satırı yardımcı programını kullanarak bir Service Fabric kümesi için yürütülebilir bir konuk dağıtma açıklar.

Bu makalede, bir konuk yürütülebilir paketini ve Service Fabric dağıtma adımları kapsar.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Service Fabric yürütülebilir Konuk çalıştıran avantajları
Bir Service Fabric kümesi yürütülebilir Konuk çalıştırmak için çeşitli avantajları vardır:

* Yüksek kullanılabilirlik. Service Fabric çalışan uygulamaları yüksek oranda kullanılabilir hale getirilir. Service Fabric uygulaması örneğini çalıştırmasını sağlar.
* Sistem durumu izleme. Service Fabric sistem durumu izleme uygulama çalışıp çalışmadığını ve bir hata olduğunda tanılama bilgileri sağlayan olmadığını algılar.   
* Uygulama yaşam döngüsü yönetimi. Yükseltme sırasında bildirilen hatalı sistem durumu olayı ise yükseltmeler kapalı kalma süresi ile sağlamanın yanı sıra, Service Fabric önceki sürümüne otomatik geri alma işlemi sağlar.    
* Yoğunluğu. Kendi donanım üzerinde çalıştırmak her bir uygulama için ihtiyacını ortadan kaldıran bir kümede birden çok uygulama çalıştırabilirsiniz.
* Bulunabilirliği: REST kullanarak kümedeki diğer hizmetler bulmak için Service Fabric adlandırma hizmeti çağırabilirsiniz. 

## <a name="samples"></a>Örnekler
* [Paketleme ve dağıtma bir konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Uygulama ve hizmet bildirim dosyaları'na genel bakış
Bir konuk yürütülebilir dağıtma bir parçası olarak, Service Fabric paketleme ve dağıtım modeli açıklandığı gibi anlamak kullanışlıdır [uygulama modeli](service-fabric-application-model.md). Service Fabric paketleme model üzerinde iki XML dosyasını kullanır: uygulama ve hizmet bildirimleri. ApplicationManifest.xml ve ServiceManifest.xml dosyaları için şema tanımı Service Fabric SDK ile yüklenen *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Uygulama bildirimini** uygulama bildirimi uygulamanın açıklamak için kullanılır. Onu oluşturan hizmetleri listeler ve örnek sayısı gibi nasıl bir veya daha fazla hizmet tanımlamak için kullanılan diğer parametreleri dağıtılmalıdır.

  Service Fabric bir uygulama dağıtımı ve yükseltmesi birimidir. Bir uygulama olası hataları ve olası düzeyine yönetildiği tek bir birim olarak yükseltilebilir. Service Fabric garanti yükseltme işlemi şunlardan biri olduğundan başarılı ya da yükseltme başarısız olursa, uygulama bilinmeyen veya kararsız bir durumda bırakmaz.
* **Hizmet bildirimi** hizmet bildirimi bir hizmet bileşenlerini açıklar. Ad ve tür hizmetini ve kod ve yapılandırma gibi verileri içerir. Hizmet bildirimi dağıtıldıktan sonra hizmeti yapılandırmak için kullanılan bazı ek parametreler de içerir.

## <a name="application-package-file-structure"></a>Uygulama paketi dosya yapısı
Service Fabric bir uygulamayı dağıtmak için uygulama önceden tanımlanmış dizin yapısına uygun olmalıdır. Aşağıda, yapıyı bir örnektir.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

ApplicationPackageRoot uygulama tanımlar ApplicationManifest.xml dosyası içerir. Uygulamasına dahil edilen her hizmet için bir alt dizin hizmeti gerektiren tüm yapıları kapsamak için kullanılır. Bu alt dizinlere ServiceManifest.xml ve genellikle aşağıdaki şunlardır:

* *Kod*. Bu dizin hizmeti kodunu içerir.
* *Config*. Bu dizin hizmeti belirli bir yapılandırma ayarlarını almak için çalışma zamanında erişebilmesini Settings.xml dosya (ve gerekirse diğer dosyaları) içerir.
* *Veri*. Bu, hizmet gerekebilir ek yerel verileri depolamak için ek bir dizindir. Verileri yalnızca kısa ömürlü verileri depolamak için kullanılmalıdır. Service Fabric kopyalamayın veya hizmet (örneğin, yük devretme sırasında) yeniden konumlandırılmasını gerekiyorsa, değişiklikleri veri dizinine çoğaltır.

> [!NOTE]
> Oluşturmanız gerekmez `config` ve `data` ihtiyaç duymuyorsanız dizinleri.
>
>

## <a name="package-an-existing-executable"></a>Paketin var olan bir yürütülebilir dosya
Bir konuk yürütülebilir paketleme, Visual Studio Proje şablonu kullanılacağını seçebilirsiniz veya [uygulama paketini el ile oluşturmak](#manually). Visual Studio kullanarak, uygulama paketi yapısı ve bildirim dosyası yeni proje şablonu tarafından sizin için oluşturulur.

> [!TIP]
> Bir hizmete yürütülebilir bir mevcut Windows paketini yapmanın en kolay yolu, Visual Studio kullanmaktır ve Linux Yeoman kullanmak için
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a>Visual Studio Paketi ve var olan bir yürütülebilir dosya dağıtmak için kullanın
Visual Studio yürütülebilir konuk bir Service Fabric kümesi dağıtmak için Service Fabric hizmet şablonu sağlar.

1. Seçin **dosya** > **yeni proje**ve bir Service Fabric uygulaması oluşturun.
2. Seçin **Konuk yürütülebilir** hizmet şablonu olarak.
3. Tıklatın **Gözat** yürütülebilir dosyanın klasörü seçin ve bir hizmet oluşturmak için parametreleri geri kalanı doldurmak için.
   * *Kod paketi davranış*. Yürütülebilir dosya değişmezse faydalı olan Visual Studio projesi klasörünüzün tüm içerik kopyalamak için ayarlanabilir. Değiştirebilir ve yeni yapılar dinamik olarak çekme olanağı istediğiniz yürütülebilir dosyanın bekliyorsanız, klasöre bağlamaya seçebilirsiniz. Visual Studio'da Uygulama projesi oluştururken, bağlantılı klasörleri kullanabilirsiniz. Konuk yürütülebilir kaynak hedefine güncelleştirmek edinerek projedeki kaynak konumundan bu bağlantılar. Bu güncelleştirmeler derlemede uygulama paketinin parçası haline gelir.
   * *Program* hizmetini başlatmak için çalıştırılmalıdır yürütülebilir dosya belirtir.
   * *Bağımsız değişkenler* yürütülebilir dosyaya geçirilen bağımsız değişkenlerini belirtir. Bağımsız değişkenlerle parametrelerin listesi olabilir.
   * *WorkingFolder* başlatılması giden işlem için çalışma dizini belirtir. Üç değerleri belirtebilirsiniz:
     * `CodeBase`Çalışma dizini uygulama paketi kodu dizininde olarak işaretleneceğini belirtir (`Code` önceki dosya yapısı içinde gösterilen dizin).
     * `CodePackage`Çalışma dizini uygulama paketi köküne ayarlanması işaretleneceğini belirtir (`GuestService1Pkg` önceki dosya yapısı içinde gösterilen).
     * `Work`dosyalar çalışma adlı bir alt dizinde yerleştirilir belirtir.
4. Hizmetinize bir ad verin ve **Tamam**’a tıklayın.
5. Hizmet iletişimi için bir uç nokta gerekirse, ServiceManifest.xml dosyasına protokolü, bağlantı noktası ve türü artık ekleyebilirsiniz. Örneğin: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Şimdi, paket kullanın ve Visual Studio çözümünde hata ayıklama yerel kümenizdeki karşı eylem yayımlayın. Hazır olduğunuzda, uzak bir küme uygulamayı yayımlama ya da çözüm kaynak denetimine iade edin.
7. Service Fabric Explorer'da çalıştıran Konuk yürütülebilir hizmetinizi görüntülemek nasıl görmek için bu makalenin sonuna gidin.

## <a name="use-yeoman-to-package-and-deploy-an-existing-executable-on-linux"></a>Yeoman için paket kullanın ve Linux üzerinde var olan bir yürütülebilir dosya dağıtma

Oluşturma ve Linux üzerinde çalıştırılabilir bir konuk dağıtma yordamı csharp veya java uygulamasını dağıtma aynıdır.

1. Bir terminal penceresinde `yo azuresfguest` yazın.
2. Uygulamanızı adlandırın.
3. Hizmet adı ve yürütülebilir dosyasının yolu ve ile çağrılmalıdır parametreleri de dahil olmak üzere ayrıntılarını sağlayın.

Yeoman uygun uygulama ile bir uygulama paketi oluşturur ve bildirim dosyası ile birlikte yüklemek ve komut dosyaları kaldırmak.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>El ile paketini ve varolan yürütülebilir bir dağıtma
Bir konuk yürütülebilir dosyayı el ile paketleme işlemi aşağıdaki genel adımları dayanır:

1. Paket dizin yapısını oluşturun.
2. Uygulamanın kod ve yapılandırma dosyalarını ekleyin.
3. Hizmet bildirim dosyasını düzenleyin.
4. Uygulama bildirim dosyasının düzenleyin.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Paket dizin yapısını oluşturun
Dizin yapısı oluşturarak "Uygulama paketi dosya yapısı" önceki bölümde açıklandığı gibi başlatabilirsiniz

### <a name="add-the-applications-code-and-configuration-files"></a>Uygulamanın kod ve yapılandırma dosyaları Ekle
Dizin yapısı oluşturduktan sonra uygulamanın kodunu ve yapılandırma dosyalarını kod ve yapılandırma dizinlerinin altında ekleyebilirsiniz. Ek dizinler veya kod veya yapılandırma dizinleri alt dizinler de oluşturabilirsiniz.

Service Fabric mu bir `xcopy` ; böylece iki üst dizini, kod ve ayarları oluşturma dışında kullanmak için önceden tanımlanmış hiçbir yapısı uygulama kök dizini içeriğinin. (Dilerseniz farklı adlar seçim yapabilirsiniz. Daha fazla ayrıntı sonraki bölümünde bulunur.)

> [!NOTE]
> Tüm dosyaları ve bağımlılıkları uygulama gereken eklediğinizden emin olun. Service Fabric nerede dağıtılacak uygulama hizmetleri bulunacağını kümedeki tüm düğümlerde uygulama paketinin içeriği kopyalar. Paket uygulama çalıştırmak için gereken tüm kodları içermesi gerekir. Bağımlılıkları zaten kurulu olduğunu varsayın değil.
>
>

### <a name="edit-the-service-manifest-file"></a>Hizmet bildirim dosyasını düzenleyin
Sonraki adım, aşağıdaki bilgileri içerecek şekilde hizmet bildirim dosyasını düzenlemek için olacaktır:

* Hizmet türü adı. Bu hizmet tanımlamak için Service Fabric kullanan bir kimliğidir.
* (ExeHost) uygulamayı başlatmak için kullanılacak komutu.
* (SetupEntrypoint) uygulamayı kurmak için çalışması gereken tüm komut dosyası.

Aşağıdaki örneğidir bir `ServiceManifest.xml` dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Aşağıdaki bölümlerde farklı bölümlerini güncelleştirmek için gereken dosya gidin.

#### <a name="update-servicetypes"></a>Güncelleştirme ServiceTypes
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* İstediğiniz herhangi bir ad seçin `ServiceTypeName`. Değer kullanılır `ApplicationManifest.xml` Hizmeti'ni tanımlamak için dosya.
* Belirtin `UseImplicitHost="true"`. Bu öznitelik Service Fabric yapmak için tüm Service Fabric gereksinimlerini olacak şekilde bir işlem olarak başlatın ve durumunu izlemek için hizmeti kendi başına bir uygulama üzerinde dayalı olduğunu bildirir.

#### <a name="update-codepackage"></a>CodePackage güncelleştir
CodePackage öğesi hizmet kodunun konumu (ve sürüm) belirtir.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

`Name` Öğe hizmetin kod içeren uygulama paketinde dizinin adını belirtmek için kullanılır. `CodePackage`Ayrıca `version` özniteliği. Bu kod sürümü belirtmek için kullanılır ve hizmet kodunun Service Fabric uygulama yaşam döngüsü yönetimi altyapısını kullanarak yükseltmek için de kullanılabilir.

#### <a name="optional-update-setupentrypoint"></a>İsteğe bağlı: Güncelleştirme SetupEntrypoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
SetupEntryPoint öğesi, hizmetin kod başlatılmadan önce yürütülmesi gereken çalıştırılabilir veya toplu iş dosyası belirtmek için kullanılır. Başlatma gerekli ise dahil edilmesi gerekmez için isteğe bağlı bir adım var. Hizmeti her başlatıldığında SetupEntryPoint yürütülür.

Uygulamanın kurulum birden çok komut dosyası gerektiriyorsa tek toplu iş dosyasında Gruplanacak Kurulum betikleri gerekiyor yalnızca bir SetupEntryPoint yoktur. SetupEntryPoint tüm dosya türlerinin yürütebilirsiniz: yürütülebilir dosyaları, toplu iş dosyaları ve PowerShell cmdlet'leri. Daha fazla ayrıntı için bkz: [yapılandırma SetupEntryPoint](service-fabric-application-runas-security.md).

Önceki örnekte SetupEntryPoint adlı bir toplu iş dosyasını çalıştıran `LaunchConfig.cmd` içinde bulunan `scripts` alt kod dizini (WorkingFolder öğesi CodeBase için ayarlanmış olduğu varsayılarak).

#### <a name="update-entrypoint"></a>EntryPoint güncelleştir
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

`EntryPoint` Hizmet bildirim dosyası öğesinde hizmeti başlatmak nasıl belirtmek için kullanılır. `ExeHost` Öğesi belirttiğinden yürütülebilir dosya (ve bağımsız değişkenler) hizmetini başlatmak için kullanılmalıdır.

* `Program`hizmetin başlaması gereken yürütülebilir dosyanın adını belirtir.
* `Arguments`yürütülebilir dosyaya geçirilen bağımsız değişkenlerini belirtir. Bağımsız değişkenlerle parametrelerin listesi olabilir.
* `WorkingFolder`başlatılacak giden işlem için çalışma dizini belirtir. Üç değerleri belirtebilirsiniz:
  * `CodeBase`Çalışma dizini uygulama paketi kodu dizininde olarak işaretleneceğini belirtir (`Code` önceki dosya yapısı içinde dizin).
  * `CodePackage`Çalışma dizini uygulama paketi köküne ayarlanması işaretleneceğini belirtir (`GuestService1Pkg` önceki dosya yapısı içinde).
    * `Work`dosyalar çalışma adlı bir alt dizinde yerleştirilir belirtir.

WorkingFolder doğru çalışma dizini göreli yollar uygulama veya başlatma komut dosyaları tarafından kullanılabilecek şekilde ayarlamak yararlıdır.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Uç noktaları güncelleştirin ve iletişim için adlandırma hizmetine kaydetme
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Önceki örnekte `Endpoint` öğesi uygulama üzerinde dinleme uç noktalarını belirtir. Bu örnekte Node.js uygulaması HTTP 3000 bağlantı noktasında dinler.

Ayrıca diğer hizmetler bu hizmete uç noktası adresi bulabilir şekilde Bu uç nokta için adlandırma hizmeti yayımlamak için Service Fabric sorabilirsiniz. Bu, Konuk yürütülebilir hizmetleri arasında iletişim kurabilmesi sağlar.
Yayımlanan uç nokta adresi biçimidir `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`ve `PathSuffix` isteğe bağlı öznitelikleri. `IPAddressOrFQDN`Bu yürütülebilir dosya üzerine yerleştirilen düğümün IP adresi veya tam etki alanı adıdır ve sizin için hesaplanır.

Hizmet dağıtıldıktan sonra aşağıdaki örnekte, Service Fabric Explorer'da benzer bir uç nokta gördüğünüz `http://10.1.4.92:3000/myapp/` hizmet örneği için yayımlanan. Veya yerel makine varsa gördüğünüz `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Bu adresleri ile kullanabileceğiniz [ters proxy](service-fabric-reverseproxy.md) hizmetleri arasında iletişim kurmak için.

### <a name="edit-the-application-manifest-file"></a>Uygulama bildirim dosyasının Düzenle
Yapılandırdıktan sonra `Servicemanifest.xml` dosyası almanız gereken bazı değişiklikler `ApplicationManifest.xml` dosya adını ve doğru hizmet türü kullanıldığından emin olun.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
İçinde `ServiceManifestImport` öğesi, uygulamada dahil etmek istediğiniz bir veya daha fazla hizmet belirtebilirsiniz. Hizmetleri ile başvurulan `ServiceManifestName`, dizinin adını belirten nerede `ServiceManifest.xml` dosyasının bulunduğu.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Günlük ayarlama
Konuk yürütülebilir dosyaları için uygulama ve yapılandırma betikleri hataları göster olmadığını öğrenmek için konsol günlükleri görebilmek kullanışlıdır.
Konsol yeniden yönlendirmesi yapılandırılabilir `ServiceManifest.xml` kullanarak dosya `ConsoleRedirection` öğesi.

> [!WARNING]
> Hiçbir zaman bu uygulamayı yük devretme etkileyebileceğinden üretimde dağıtılan bir uygulamada konsol yeniden yönlendirme ilkesi kullanın. *Yalnızca* bunu yerel geliştirme ve hata ayıklama amacıyla kullanın.  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection`Konsol çıkış (stdout ve stderr) yeniden yönlendirmek için bir çalışma dizini için kullanılabilir. Bu hata Kurulum veya uygulama Service Fabric kümesi içinde yürütülmesi sırasında doğrulayın olanağı sağlar.

`FileRetentionCount`kaç tane dosyaları çalışma dizinini kaydedilir belirler. Örneğin, 5, değeri önceki beş yürütmeleri için günlük dosyalarını çalışma dizininde depolanır anlamına gelir.

`FileMaxSizeInKb`Günlük dosyalarının en büyük boyutunu belirtir.

Günlük dosyaları hizmetin çalışma dizinleri birinde kaydedilir. Dosyaların bulunduğu belirlemek için hizmetin hangi düğümün belirlemek için Service Fabric Explorer kullanın çalıştıran ve hangi çalışma dizini kullanılır. Bu işlem, bu makalenin sonraki bölümlerinde ele alınmaktadır.

## <a name="deployment"></a>Dağıtım
Son adım [uygulamanızı dağıtmak](service-fabric-deploy-remove-applications.md). Aşağıdaki PowerShell betiğini yerel geliştirme küme uygulamanızı dağıtmak ve yeni bir Service Fabric hizmeti başlatmak nasıl gösterir.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Paket Sıkıştır](service-fabric-package-apps.md#compress-a-package) paketi büyük veya çok sayıda dosya varsa görüntü deposuna kopyalamadan önce. Daha fazla bilgi [burada](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Service Fabric hizmeti çeşitli "yapılandırmalarında." dağıtılabilir Örneğin, tek veya birden çok örneği dağıtılabilir veya Service Fabric kümesinin her bir düğümde Hizmeti'nin bir örneğini olduğunu şekilde dağıtılabilir.

`InstanceCount` Parametresinin `New-ServiceFabricService` cmdlet hizmetinin kaç tane Service Fabric kümesi başlatılması gerektiğini belirtmek için kullanılır. Ayarlayabileceğiniz `InstanceCount` dağıtmakta olduğunuz uygulama türüne bağlı olarak değeri. İki en yaygın senaryolar şunlardır:

* `InstanceCount = "1"`. Bu durumda, yalnızca bir hizmet örneği kümede dağıtılır. Service Fabric'ın Zamanlayıcı hizmeti üzerinde dağıtılması gittiği hangi düğümün belirler.
* `InstanceCount ="-1"`. Bu durumda, hizmetin bir örneği, Service Fabric kümedeki her düğümde dağıtılır. Sonucu kümedeki her düğüm için hizmetin bir (ve yalnızca bir) örneğinin yaşıyor.

İstemci uygulamaları "Bağlan" herhangi bir uç nokta kullanmak için kümedeki düğümlerin gerekir (örneğin, bir REST uç noktası), ön uç uygulamalar için kullanışlı bir yapılandırma olmasıdır. Örneğin, Service Fabric kümesinin tüm düğümlerine bir yük dengeleyiciye bağlı olduğunda, bu yapılandırma de kullanılabilir. İstemci trafiğini kümedeki tüm düğümler üzerinde çalışan hizmet genelinde sonra dağıtılabilir.

## <a name="check-your-running-application"></a>Çalışan uygulamanızı denetleyin
Service Fabric Explorer'da hizmetinin çalıştığı düğüm tanımlayın. Bu örnekte, Düğüm1 üzerinde çalışır:

![Hizmetinin çalıştığı düğüm](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Düğümüne gidin ve uygulamaya göz atın, diskteki konumuna dahil olmak üzere temel düğüm bilgileri görebilirsiniz.

![Disk konumu](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Sunucu Gezgini kullanarak dizinine göz atarsanız, aşağıdaki ekran görüntüsünde gösterildiği gibi çalışma dizinini ve hizmetin Günlük klasörü bulabilirsiniz: 

![Günlük konumu](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir konuk yürütülebilir paketinin ve Service Fabric dağıtmayı öğrendiniz. İlgili bilgi ve görevler için aşağıdaki makalelere bakın.

* [Paketleme ve dağıtma bir konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), yayın öncesi paketleme Aracı'nın bağlantı
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
* [Visual Studio kullanarak ilk Service Fabric uygulamanızı oluşturma](service-fabric-create-your-first-application-in-visual-studio.md)
