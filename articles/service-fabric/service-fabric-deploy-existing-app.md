---
title: Varolan bir yürütülebilir dosya, Azure Service Fabric'e dağıtma | Microsoft Docs
description: Bir Service Fabric kümesine dağıtılabilmesi olarak bir konuk yürütülebilir dosyası, mevcut bir uygulama paketi hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: aljo
ms.openlocfilehash: bfac14c598b405a398cad916787aa3312589bfd1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60393580"
---
# <a name="package-and-deploy-an-existing-executable-to-service-fabric"></a>Paketleme ve varolan bir yürütülebilir dosya Service Fabric'e dağıtma
Varolan bir yürütülebilir dosya olarak paketlenirken bir [Konuk yürütülebilir dosyası](service-fabric-guest-executables-introduction.md), Visual Studio Proje şablonu kullanmayı seçebilirsiniz veya [uygulama paketini el ile oluşturmak](#manually). Visual Studio kullanarak, uygulama paketi yapısı ve bildirim dosyalarını yeni bir proje şablonu tarafından sizin için oluşturulur.

> [!TIP]
> Bir hizmette yürütülebilir bir var olan Windows paketini en kolay yolu, Visual Studio kullanmaktır ve Yeoman'ı kullanmak için Linux
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a>Paket ve varolan bir yürütülebilir dosya dağıtmak için Visual Studio'yu kullanın.
Visual Studio, bir konuk yürütülebilir bir Service Fabric kümesine dağıtmanıza yardımcı olması için bir Service Fabric hizmet şablonu sağlar.

1. Seçin **dosya** > **yeni proje**ve bir Service Fabric uygulaması oluşturma.
2. Seçin **Konuk yürütülebilir dosyası** hizmet şablonu olarak.
3. Tıklayın **Gözat** yürütülebilir dosyanızın klasörü seçin ve hizmet oluşturmak için parametreleri geri kalanı doldurmak için.
   * *Kod paketi davranışı*. Yürütülebilir değişmezse faydalı olan Visual Studio projesi için tüm içerik klasörünüzün kopyalamak için ayarlanabilir. Yürütülebilir dosyayı değiştirmek ve yeni derlemeler dinamik olarak çekme olanağı bekliyorsanız, klasöre bağlamaya seçebilirsiniz. Visual Studio uygulama projesini oluştururken, bağlantılı klasör kullanabilirsiniz. Bu projedeki kaynak hedefine içinde yürütülebilir Konuk güncelleştirilecek edinerek kaynak konumdan bağlar. Bu güncelleştirmeler, derlemede uygulama paketinin parçası haline gelir.
   * *Program* hizmeti başlatmak için çalıştırılması gereken yürütülebilir dosyayı belirtir.
   * *Bağımsız değişkenler* yürütülebilir dosyaya geçirilen bağımsız değişkenleri belirtir. Bu bağımsız değişkenler parametrelerle listesi olabilir.
   * *WorkingFolder* başlatılması gitme işlemi için çalışma dizini belirtir. Üç değer belirtebilirsiniz:
     * `CodeBase` Uygulama paketi kod dizinine ayarlanacak çalışma dizini geçiyor belirtir (`Code` önceki dosya yapısı içinde gösterilen dizin).
     * `CodePackage` Uygulama paketini kök dizinine ayarlanacak çalışma dizini geçiyor belirtir (`GuestService1Pkg` önceki dosya yapısı içinde gösterilen).
     * `Work` dosyalar çalışma adlı bir alt dizinine yerleştirilir belirtir.
4. Hizmetinize bir ad verin ve **Tamam**’a tıklayın.
5. Hizmetiniz iletişim için bir uç nokta gerekiyorsa, ServiceManifest.xml dosyasına protokol, bağlantı noktası ve tür artık ekleyebilirsiniz. Örneğin: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Şimdi, paketi kullanan ve çözümünü Visual Studio'da hata ayıklama tarafından eylem yerel kümenizdeki yayımlayın. Hazır olduğunda uygulamayı uzak kümeye yayımlama veya çözüm kaynak denetimine iade edin.
7. Okuma [çalışan uygulamanıza denetleyin](#check-your-running-application) çalışan Service Fabric Explorer'ın, Konuk yürütülebilir hizmet görüntüleme görmek için.

Bir örnek için bkz [Visual Studio kullanarak ilk Konuk yürütülebilir uygulamanızı oluşturma](quickstart-guest-app.md).

## <a name="use-yeoman-to-package-and-deploy-an-existing-executable-on-linux"></a>Paket için Yeoman'ı kullanın ve Linux üzerinde varolan bir yürütülebilir dosya dağıtma

Oluşturma ve Linux'ta yürütülebilir Konuk dağıtma yordamı bir csharp veya java uygulamasını dağıtma aynıdır.

1. Bir terminal penceresinde `yo azuresfguest` yazın.
2. Uygulamanızı adlandırın.
3. Hizmetinizi adlandırın ve yürütülebilir dosyanın yolu ve ile çağrılmalıdır parametreleri gibi ayrıntıları sağlayın.

Yeoman uygun uygulama ile bir uygulama paketi oluşturur ve bildirim dosyaları ile birlikte yükler ve komut dosyaları kaldırın.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>El ile paket ve varolan bir yürütülebilir dosya dağıtma
El ile konuk tarafından yürütülebilir bir paketleme işlemi, aşağıdaki genel adımları temel alır:

1. Paket dizin yapısı oluşturun.
2. Uygulamanın kod ve yapılandırma dosyalarını ekleyin.
3. Hizmet bildirim dosyasını düzenleyin.
4. Uygulama bildirim dosyasını düzenleyin.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](https://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Paket dizin yapısı oluşturma
Dizin yapısı oluşturarak açıklandığı başlatabilirsiniz [Azure Service Fabric uygulama paketini](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps).

### <a name="add-the-applications-code-and-configuration-files"></a>Uygulamanın kod ve yapılandırma dosyaları Ekle
Dizin yapısı oluşturduktan sonra uygulamanın kod ve yapılandırma dosyalarını kodda ve yapılandırma dizinleri altında ekleyebilirsiniz. Ayrıca, ek dizinleri veya kod veya yapılandırma dizin alt dizinler de oluşturabilirsiniz.

Service Fabric mu bir `xcopy` içeriğini diğer iki üst dizini, kod ve ayarları oluşturmaktan kullanmak için önceden tanımlanmış hiçbir yapısı olduğundan uygulaması kök dizini. (Dilerseniz farklı adlar seçim yapabilirsiniz. Daha fazla ayrıntı sonraki bölümde vardır.)

> [!NOTE]
> Tüm dosyaları ve bağımlılıkları uygulamanız için gereken eklediğinizden emin olun. Service Fabric uygulamanın hizmetlerine dağıtılması nerede bulunacağını kümedeki tüm düğümlere uygulama paketinin içeriği kopyalar. Paket, uygulama çalıştırmak için gereken tüm kod içermelidir. Bağımlılıkları yüklü varsaymayın.
>
>

### <a name="edit-the-service-manifest-file"></a>Hizmet bildirimi dosyası Düzenle
Sonraki adım, aşağıdaki bilgileri içerecek şekilde hizmet bildirim dosyasını düzenlemek için içerir:

* Hizmet türünün adı. Bu hizmet tanımlamak için Service Fabric kullanan bir kimliğidir.
* Uygulama (ExeHost) başlatmak için kullanılacak komutu.
* Uygulama (SetupEntrypoint) ayarlama için çalıştırılması gereken komut dosyaları.

Aşağıdaki örneğidir bir `ServiceManifest.xml` dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

Aşağıdaki bölümlerde farklı bölümlerini güncelleştirmeye gerek duyduğunuz dosyanın gidin.

#### <a name="update-servicetypes"></a>Güncelleştirme ServiceTypes
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* İstediğiniz herhangi bir ad seçebilirsiniz `ServiceTypeName`. Değeri kullanılır `ApplicationManifest.xml` hizmetinizi tanımlayabilmek için dosya.
* Belirtin `UseImplicitHost="true"`. Bu öznitelik, Service Fabric yapmak için tüm Service Fabric gereksinimlerini olacak şekilde farklı bir işlem olarak başlatmak ve sistem durumu izlemek için hizmet kendi içinde bir uygulamada dayalı olduğunu söyler.

#### <a name="update-codepackage"></a>CodePackage güncelleştir
CodePackage öğesi hizmet kodunun konumu (ve sürümü) belirtir.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

`Name` Öğesi hizmet kodunun içeren uygulama paketinde dizinin adını belirtmek için kullanılır. `CodePackage` Ayrıca `version` özniteliği. Bu kod sürümünü belirtmek için kullanılabilir ve hizmet kodunun Service Fabric'te uygulama yaşam döngüsü yönetimi altyapısını kullanarak yükseltmek için de kullanılabilir.

#### <a name="optional-update-setupentrypoint"></a>İsteğe bağlı: Güncelleştirme SetupEntrypoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
SetupEntryPoint öğesi hizmet kodunun başlatılmadan önce yürütülmesi gereken yürütülebilir dosyası ya da toplu iş dosyası belirtmek için kullanılır. Başlatma gerekli olduğunda dahil edilmesi gerekmez, isteğe bağlı bir adım olduğundan. Hizmeti her başlatıldığında SetupEntryPoint yürütülür.

Kurulum betikleri birden fazla komut dosyası uygulamanın kurulum gerektiriyorsa, tek bir toplu iş dosyasında gruplanması gereken şekilde tek SetupEntryPoint yoktur. Tüm dosya türlerinin SetupEntryPoint yürütebilirsiniz: yürütülebilir dosyaları, toplu iş dosyaları ve PowerShell cmdlet'leri. Daha fazla ayrıntı için [yapılandırma SetupEntryPoint](service-fabric-application-runas-security.md).

Önceki örnekte SetupEntryPoint adlı bir toplu iş dosyası çalıştırır `LaunchConfig.cmd` bulunan `scripts` (WorkingFolder öğesi, kod tabanıyla ayarlandığında varsayılarak) kod dizininin alt.

#### <a name="update-entrypoint"></a>Giriş noktası güncelleştirme
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

`EntryPoint` Öğesi hizmet bildirimi dosyasında hizmeti başlatmak nasıl belirtmek için kullanılır.

`ExeHost` Öğesi belirtir yürütülebilir dosya (ve bağımsız değişkenler) hizmeti başlatmak için kullanılmalıdır. İsteğe bağlı olarak ekleyebileceğiniz `IsExternalExecutable="true"` özniteliğini `ExeHost` program dış yürütülebilir kod paketi dışında olduğunu belirtmek için. Örneğin, `<ExeHost IsExternalExecutable="true">`.

* `Program` hizmetin başlaması gereken yürütülebilir dosyanın adını belirtir.
* `Arguments` yürütülebilir dosyaya geçirilen bağımsız değişkenleri belirtir. Bu bağımsız değişkenler parametrelerle listesi olabilir.
* `WorkingFolder` başlatılması gitme işlemi için çalışma dizini belirtir. Üç değer belirtebilirsiniz:
  * `CodeBase` Uygulama paketi kod dizinine ayarlanacak çalışma dizini geçiyor belirtir (`Code` önceki dosya yapısı içinde dizin).
  * `CodePackage` Uygulama paketini kök dizinine ayarlanacak çalışma dizini geçiyor belirtir (`GuestService1Pkg` önceki dosya yapısı içinde).
    * `Work` dosyalar çalışma adlı bir alt dizinine yerleştirilir belirtir.

WorkingFolder doğru çalışma dizini göreli yollar uygulama veya başlatma komut dosyaları tarafından kullanılabilecek şekilde ayarlamak yararlıdır.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Uç noktalarını güncelleştirin ve iletişim için adlandırma hizmeti ile kaydetme
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Önceki örnekte `Endpoint` öğesi üzerinde uygulama dinleyebilirsiniz uç noktalarını belirtir. Bu örnekte, Node.js uygulaması HTTP 3000 bağlantı noktasında dinler.

Ayrıca diğer hizmetlerin bu hizmet için uç nokta adresini bulabilir. Bu nedenle, bu uç noktayı adlandırma hizmetinde yayımlamak için Service Fabric sorabilirsiniz. Bu, Konuk yürütülebilir dosyaları, hizmetler arasında iletişim kurabilmesi sağlar.
Yayımlanan bir uç nokta adresini biçimindedir `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme` ve `PathSuffix` isteğe bağlı öznitelikleri. `IPAddressOrFQDN` IP adresi ya da bu yürütülebilir dosya çubuğunda yer düğümün tam etki alanı adı ve sizin için hesaplanır.

Hizmet dağıtıldıktan sonra aşağıdaki örnekte, Service Fabric Explorer'ın benzer bir uç nokta gördüğünüz `http://10.1.4.92:3000/myapp/` hizmet örneği için yayımlanmış. Veya bir yerel makineye buysa, gördüğünüz `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Bu adresleri ile kullanabileceğiniz [ters proxy](service-fabric-reverseproxy.md) Hizmetleri iletişim.

### <a name="edit-the-application-manifest-file"></a>Uygulama bildirim dosyasını Düzenle
Yapılandırdıktan sonra `Servicemanifest.xml` dosyasını gereken bazı değişiklikler yapmak `ApplicationManifest.xml` dosyasının adını ve doğru hizmet türü kullanıldığından emin olun.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>Servicemanifestımport
İçinde `ServiceManifestImport` öğesi, uygulamada dahil etmek istediğiniz bir veya daha fazla hizmet belirtebilirsiniz. Hizmetleri ile başvuru `ServiceManifestName`, dizinin adını belirten burada `ServiceManifest.xml` dosyasının bulunduğu.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Günlük kaydını ayarlama
Konuk yürütülebilir dosyaları için uygulama ve yapılandırma betiklerini hataları göster, öğrenmek için konsol günlüklerinin görebilmeniz kullanışlıdır.
Konsol yönlendirmesi, içinde yapılandırılabilir `ServiceManifest.xml` kullanarak dosya `ConsoleRedirection` öğesi.

> [!WARNING]
> Hiçbir zaman konsol yeniden yönlendirme ilkesi, bu uygulamanın yük devretme etkileyebileceğinden, üretimde dağıtılan bir uygulamada kullanın. *Yalnızca* bunu yerel geliştirme ve hata ayıklama amacıyla kullanın.  
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

`ConsoleRedirection` Konsol çıkış (stdout ve stderr) yeniden yönlendirmek için bir çalışma dizini için kullanılabilir. Bu hata Kurulum veya Service Fabric kümesine uygulama yürütme sırasında doğrulayın olanağı sağlar.

`FileRetentionCount` kaç tane dosya çalışma dizinine kaydedilir belirler. Örneğin, bir değeri 5, günlük dosyaları önceki beş yürütme için çalışma dizininde depolanır anlamına gelir.

`FileMaxSizeInKb` Günlük dosyalarının maksimum boyutunu belirtir.

Günlük dosyaları, hizmetin çalışma dizinleri birinde kaydedilir. Dosyaların nerede olduğunu belirlemek için hizmetin hangi düğümün belirlemek için Service Fabric Explorer kullanacaksınız çalıştıran ve hangi çalışma dizini kullanılır. Bu işlem, bu makalenin sonraki bölümlerinde ele alınmıştır.

## <a name="deployment"></a>Dağıtım
Son adım [uygulamanızı dağıtmak](service-fabric-deploy-remove-applications.md). Aşağıdaki PowerShell komut dosyası, uygulamanızı yerel geliştirme kümesi dağıtmak ve yeni bir Service Fabric hizmeti başlatmak gösterilmektedir.

```powershell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Paket sıkıştırma](service-fabric-package-apps.md#compress-a-package) paketi büyük veya çok sayıda dosya varsa, görüntü deposuna kopyalamak önce. Daha fazla bilgi edinin [burada](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Bir Service Fabric hizmeti çeşitli "yapılandırmalarda." yeniden dağıtılabilir Örneğin, tek veya birden çok örnek dağıtılabilir veya Service Fabric kümesinin her bir düğümde hizmetinin bir örneği olduğu şekilde dağıtılabilir.

`InstanceCount` Parametresinin `New-ServiceFabricService` cmdlet'i, Service Fabric kümesinde kaç tane hizmetin başlatılması belirtmek için kullanılır. Ayarlayabileceğiniz `InstanceCount` dağıtmakta olduğunuz uygulama türüne bağlı olarak bir değer. İki en yaygın senaryolar şunlardır:

* `InstanceCount = "1"`. Bu durumda, yalnızca bir hizmet örneğini kümeye dağıtılır. Service Fabric'in Zamanlayıcı hizmeti üzerinde dağıtılmaya devam hangi düğümün belirler.
* `InstanceCount ="-1"`. Bu durumda, hizmetin bir örneği, Service Fabric kümesindeki her düğümde dağıtılır. Sonucu, kümedeki her düğüm için hizmetin bir (ve tek) örneği yaşıyor.

İstemci uygulamaları "Bağlan" herhangi bir uç nokta kullanmak için kümedeki düğümlerin gerektiğinden (örneğin, bir REST uç noktası), ön uç uygulamaları için kullanışlı bir yapılandırma budur. Örneğin, Service Fabric kümesinin tüm düğümlerine bir yük dengeleyiciye bağlı olduğunda, bu yapılandırma de kullanılabilir. İstemci trafiğini sonra kümedeki tüm düğümler üzerinde çalışan hizmet üzerinde dağıtılabilir.

## <a name="check-your-running-application"></a>Çalışan uygulamanızı denetleyin
Service Fabric Explorer'ın hizmetinin çalıştığı düğüm belirleyin. Bu örnekte, Düğüm1 üzerinde çalışır:

![Hizmetinin çalıştığı düğüm](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Düğümüne gidin ve uygulamaya göz atın, diskteki konumuna dahil olmak üzere, gerekli düğüm bilgileri görebilirsiniz.

![Disk üzerindeki konum](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Sunucu Gezgini kullanarak dizine göz atarsanız, aşağıdaki ekran görüntüsünde gösterildiği gibi çalışma dizinini ve hizmetin Günlük klasörü bulabilirsiniz: 

![Günlük konumu](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, konuk tarafından yürütülebilir bir paketleme ve Service Fabric'e dağıtma öğrendiniz. İlgili bilgi ve görevler için aşağıdaki makalelere bakın.

* [Paketleme ve dağıtma Konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), yayın öncesi paketleme aracı bağlantı
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-containers)
* [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
* [Visual Studio kullanarak ilk Service Fabric uygulamanızı oluşturma](service-fabric-tutorial-create-dotnet-app.md)
