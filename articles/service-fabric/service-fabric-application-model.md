---
title: Azure Service Fabric uygulama modeli | Microsoft Docs
description: "Nasıl model ve uygulamaları ve Hizmetleri Service Fabric açıklanmaktadır."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: e30482427b88eb3e58d39075c7f0734664b79aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="model-an-application-in-service-fabric"></a>Service Fabric uygulamada modeli
Bu makalede Azure Service Fabric uygulama modeli ve bir uygulama ve hizmet bildirim dosyaları aracılığıyla nasıl tanımlanacağı genel bakış sağlar.

## <a name="understand-the-application-model"></a>Uygulama modeli anlama
Bir uygulama, belirli bir işlevi veya işlevleri gerçekleştirmek bağlı hizmetler topluluğudur. Bir hizmet tam ve tek başına bir işlevi gerçekleştirir ve başlayın ve diğer hizmetler bağımsız olarak çalışır.  Kod, yapılandırma ve verileri bir servis oluşur. Her hizmet için kodu yürütülebilir ikili dosyaları oluşur, çalışma zamanında yüklenen hizmet ayarlarını yapılandırma oluşur ve veri hizmeti tarafından kullanılacak rasgele statik verileri oluşur. Bu hiyerarşik uygulama modeli her bileşenin bağımsız olarak sürümlü hem de yükseltilmiş olabilir.

![Service Fabric uygulama modeli][appmodel-diagram]

Uygulama türü, bir uygulamanın bir kategori değil ve hizmet türlerinin bir dizi oluşur. Bir hizmetin kategori bir hizmet türüdür. Kategori farklı ayarları ve yapılandırmaları olabilir, ancak temel işlevleri aynı kalır. Farklı hizmet yapılandırma farklılıkları aynı hizmet türünün hizmet örnekleridir.  

Sınıflar (veya "türleri") uygulamaları ve Hizmetleri XML dosyalarını (uygulama bildirimlerini ve hizmet bildirimlerini) açıklanan.  Bildirimler, kümenin görüntü deposundan göre uygulamaları oluşturulabilir şablonlarıdır. ServiceManifest.xml ve ApplicationManifest.xml dosyası için şema tanımı Service Fabric SDK ile birlikte yüklenir ve araçların *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Farklı uygulama örnekleri için kod bile aynı Service Fabric düğümüne barındırıldığında ayrı işlemler olarak çalıştırın. Ayrıca, her uygulama örneği yaşam döngüsü (örneğin yükseltilmiş) yönetilebilir bağımsız olarak. Aşağıdaki diyagramda uygulama türleri sırayla kod, yapılandırma ve veri paketleri oluşan hizmet türlerini tespiti gösterir. Diyagram basitleştirmek için yalnızca kod/config/verileri paketler için `ServiceType4` her hizmet türü bazı içerir veya bu tüm paket türlerinin de gösterilir.

![Service Fabric uygulama türleri ve hizmet türleri][cluster-imagestore-apptypes]

İki farklı bildirim dosyası, uygulamaları ve hizmetleri tanımlamak için kullanılır: hizmet bildirimi ve uygulama bildirimi. Bildirimler, aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

Kümedeki etkin bir hizmet türünün bir veya daha fazla örneğini olabilir. Örneğin, durum bilgisi olan hizmet örneği veya çoğaltmalar, yüksek güvenilirlik kümedeki farklı düğümlerde bulunan çoğaltmalar arasında durumu yineleyerek elde edin. Çoğaltma temelde hizmetinin bir kümedeki bir düğümün başarısız olsa bile kullanılabilir olması artıklık sağlar. A [bölümlenmiş hizmet](service-fabric-concepts-partitioning.md) daha fazla kümedeki düğümler arasında alt durumu (ve bu durum için erişim düzenlerini) böler.

Aşağıdaki diyagram, uygulamaları ve hizmet örnekleri, bölümler ve çoğaltmalar arasındaki ilişkiyi gösterir.

![Bölümler ve çoğaltmalar bir hizmet kapsamındaki][cluster-application-instances]

> [!TIP]
> Http:// kullanılabilir Service Fabric Explorer aracını kullanarak bir küme uygulamalarının düzeni görüntüleyebileceği&lt;yourclusteraddress&gt;: 19080/Explorer. Daha fazla bilgi için bkz: [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Bir hizmeti açıklayın
Hizmet bildirimi bildirimli olarak hizmet türü ve sürümü tanımlar. Hizmet türü, sistem durumu özellikleri, Yük Dengeleme ölçümleri, hizmeti ikili dosyalarını ve yapılandırma dosyaları gibi hizmet meta verilerini belirtir.  Başka bir deyişle, bir veya daha fazla hizmet türlerini desteklemek için bir hizmet paketi oluşturan kod, yapılandırma ve veri paketleri açıklar. Basit bir örnek hizmet bildirimi şöyledir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

**Sürüm** öznitelikleri yapılandırılmamış dizelerdir ve sistem tarafından çözümlenmemiş. Sürüm öznitelikleri sürüme her bileşenin yükseltmeleri için kullanılır.

**ServiceTypes** hangi hizmet türleri tarafından desteklenen bildirir **CodePackages** bu bildiriminde. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda bu bildirimde belirtilen tüm kod paketleri kendi giriş noktaları çalıştırarak etkinleştirilir. Sonuçta elde edilen işlemleri çalışma zamanında desteklenen hizmet türlerinin kaydetmek beklenir. Hizmet türlerini, bildirim düzeyini ve kod paketi düzeyinde bildirilir. Birden çok kod paket olduğunda bildirilen hizmet türleri herhangi biri için sistem bakar olduğunda bu nedenle bunlar tüm etkinleştirilir.

**SetupEntryPoint** Service Fabric kimlik bilgileriyle çalışır ayrıcalıklı giriş noktasıdır (genellikle *LocalSystem* hesabı) önce başka bir giriş noktası. Tarafından belirtilen yürütülebilir **EntryPoint** genellikle uzun süre çalışan hizmet yöneticisidir. Ayrı Kurulum giriş noktası varlığını hizmet konağı yüksek ayrıcalıklara sahip uzun süre için çalıştırmanız gereğini ortadan kaldırır. Tarafından belirtilen yürütülebilir **EntryPoint** çalıştırıldıktan **SetupEntryPoint** başarıyla çıkar. İşlemin her zamankinden sonlandırır veya çöküyor, sonuçta elde edilen işlem yeniden ve izlendiği (yeniden itibaren **SetupEntryPoint**).  

Kullanma için tipik senaryolar **SetupEntryPoint** hizmeti başlamadan önce bir yürütülebilir dosyayı çalıştırmak veya yükseltilmiş ayrıcalıklarla bir işlem gerçekleştirdiğinizde durumdadır. Örneğin:

* Ayarlama ve hizmeti yürütülebilir dosyası gerekli ortam değişkenleri başlatılıyor. Bu, yalnızca Service Fabric programlama modeli yazılmış yürütülebilir dosyalar için sınırlı değildir. Örneğin, bir node.js uygulamasını dağıtmak için yapılandırılmış bazı ortam değişkenleri npm.exe gerekir.
* Güvenlik sertifikaları yükleyerek erişim denetimini ayarlama.

Nasıl yapılandırılacağı hakkında daha fazla ayrıntı için **SetupEntryPoint** görmek [hizmet Kurulum giriş noktası için ilkeyi yapılandırın](service-fabric-application-runas-security.md)

**EnvironmentVariables** Bu kod paketi için ayarlanan ortam değişkenlerini bir listesini sağlar. Environmentment değişkenleri geçersiz kılınmış içinde `ApplicationManifest.xml` farklı hizmet örnekleri için farklı değerler sağlamak için. 

**Nın DataPackage** tarafından adlı bir klasör bildirir **adı** işlem tarafından çalışma zamanında kullanılacak rastgele statik verileri içeren öznitelik.

**ConfigPackage** tarafından adlı bir klasör bildirir **adı** içeren öznitelik, bir *Settings.xml* dosya. Ayarlar dosyası işlemi geri çalışma zamanında okur kullanıcı tanımlı, anahtar-değer çifti ayarları bölümleri içerir. Yalnızca yükseltme sırasında **ConfigPackage** **sürüm** değişti, çalışan işlem yeniden sonra. Bunun yerine, bir geri çağırma bunlar dinamik olarak yeniden yüklendi için yapılandırma ayarları değişti işlem bildirir. İşte bir örnek *Settings.xml* dosyası:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> Bir hizmet bildirimi birden çok kodu, yapılandırma ve veri paketleri içerebilir. Bunların her biri bağımsız olarak sürümlü olabilir.
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Bir uygulamayı tanımlayan
Uygulama bildirimini bildirimli olarak uygulama türü ve sürümü açıklar. Hizmet oluşturma meta veri şema, örnek sayısı/çoğaltma faktörü, güvenlik/yalıtım İlkesi, yerleştirme kısıtlamaları, yapılandırma geçersiz kılmaları ve bağlı hizmet türü bölümleme kararlı adları gibi belirtir. Uygulama üzerine yerleştirilen yük dengeleyici etki alanları da açıklanmaktadır.

Bu nedenle, bir uygulama bildirimi uygulama düzeyinde öğeleri açıklar ve uygulama türü oluşturmak için bir veya daha fazla hizmet bildirimlerini başvuruyor. Basit bir örnek uygulama bildirimi şöyledir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

Gibi hizmet bildirimlerini **sürüm** öznitelikleri yapılandırılmamış dizelerdir ve sistem tarafından çözümlenmemiş. Sürüm öznitelikleridir de sürüme her bileşenin yükseltmeleri için kullanılır.

**ServiceManifestImport** bu uygulama türü oluşturan hizmet bildirimlerini başvurular içeriyor. Alınan hizmet bildirimlerini hangi hizmet türleri içindeki bu uygulama türü geçerli belirler. ServiceManifestImport içinde ServiceManifest.xml dosyalarında Settings.xml ve ortam değişkenleri içindeki yapılandırma değerleri geçersiz. 


**DefaultServices** bir uygulama bu uygulama türü karşı örneği olduğunda, otomatik olarak oluşturulan hizmet örnekleri bildirir. Varsayılan Hizmetleri yalnızca kolaylık sağlamak ve oluşturulduktan sonra her açısından normal services gibi davranır. Bunlar, herhangi bir uygulama örneği Hizmetleri'nde birlikte yükseltilir ve de kaldırılabilir.

> [!NOTE]
> Bir uygulama bildirimi birden çok hizmet bildirimi alır ve varsayılan hizmet içerebilir. Her hizmet bildirim alma bağımsız olarak sürümlü olabilir.
> 
> 

Farklı bir uygulama ve hizmet parametreleri tek tek ortamlar için korumak öğrenmek için bkz: [birden çok ortamlar için uygulama parametreleri yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulama paketi](service-fabric-package-apps.md) ve dağıtmak hazır olun.

[Dağıtma ve uygulamaları kaldırma] [ 10] uygulama örnekleri yönetmek için PowerShell kullanmayı açıklar.

[Birden çok ortamlar için uygulama parametreleri yönetme] [ 11] parametreleri ve farklı uygulama örnekleri için ortam değişkenleri nasıl yapılandırılacağını açıklar.

[Uygulamanız için güvenlik ilkelerini yapılandırmak] [ 12] erişimi kısıtlamak için güvenlik ilkeleri altında hizmetlerini çalıştırmak açıklar.

[Uygulama barındırma modellerini] [ 13] bir dağıtılan hizmet ve hizmet ana bilgisayar işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi açıklar.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
