---
title: Azure Service Fabric uygulamaları ve Hizmetleri açıklayan | Microsoft Docs
description: Bildirimleri Service Fabric uygulamaları ve hizmetleri tanımlamak için nasıl kullanıldığını açıklar.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: ryanwi
ms.openlocfilehash: b79206b9d456226d14984e8a1c1002c07c4f626a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-application-and-service-manifests"></a>Service Fabric uygulaması ve hizmeti bildirimleri
Bu makalede, tanımlı ve sürümü tutulan ApplicationManifest.xml ve ServiceManifest.xml dosyalarını kullanarak nasıl Service Fabric uygulamaları ve Hizmetleri açıklanmaktadır.  Bu bildirim dosyalar için XML Şeması belgelenen [ServiceFabricServiceModel.xsd şema belgelerine](service-fabric-service-model-schema.md).

## <a name="describe-a-service-in-servicemanifestxml"></a>ServiceManifest.xml hizmetinde açıklayın
Hizmet bildirimi bildirimli olarak hizmet türü ve sürümü tanımlar. Hizmet türü, sistem durumu özellikleri, Yük Dengeleme ölçümleri, hizmeti ikili dosyalarını ve yapılandırma dosyaları gibi hizmet meta verilerini belirtir.  Başka bir deyişle, bir veya daha fazla hizmet türlerini desteklemek için bir hizmet paketi oluşturan kod, yapılandırma ve veri paketleri açıklar. Bir hizmet bildirimi birden çok kod, yapılandırma ve bağımsız olarak sürümlü olabilir veri paketleri içerebilir. ASP.NET web ön uç hizmetine yönelik hizmet bildirim işte [örnek uygulama oylama](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

**Sürüm** öznitelikleri yapılandırılmamış dizelerdir ve sistem tarafından çözümlenmemiş. Sürüm öznitelikleri sürüme her bileşenin yükseltmeleri için kullanılır.

**ServiceTypes** hangi hizmet türleri tarafından desteklenen bildirir **CodePackages** bu bildiriminde. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda bu bildirimde belirtilen tüm kod paketleri kendi giriş noktaları çalıştırarak etkinleştirilir. Sonuçta elde edilen işlemleri çalışma zamanında desteklenen hizmet türlerinin kaydetmek beklenir. Hizmet türlerini, bildirim düzeyini ve kod paketi düzeyinde bildirilir. Birden çok kod paket olduğunda bildirilen hizmet türleri herhangi biri için sistem bakar olduğunda bu nedenle bunlar tüm etkinleştirilir.

Tarafından belirtilen yürütülebilir **EntryPoint** genellikle uzun süre çalışan hizmet yöneticisidir. **SetupEntryPoint** Service Fabric kimlik bilgileriyle çalışır ayrıcalıklı giriş noktasıdır (genellikle *LocalSystem* hesabı) önce başka bir giriş noktası.  Ayrı Kurulum giriş noktası varlığını hizmet konağı yüksek ayrıcalıklara sahip uzun süre için çalıştırmanız gereğini ortadan kaldırır. Tarafından belirtilen yürütülebilir **EntryPoint** çalıştırıldıktan **SetupEntryPoint** başarıyla çıkar. İşlemin her zamankinden sonlandırır veya çöküyor, sonuçta elde edilen işlem yeniden ve izlendiği (yeniden itibaren **SetupEntryPoint**).  

Kullanma için tipik senaryolar **SetupEntryPoint** hizmeti başlamadan önce bir yürütülebilir dosyayı çalıştırmak veya yükseltilmiş ayrıcalıklarla bir işlem gerçekleştirdiğinizde durumdadır. Örneğin:

* Ayarlama ve hizmeti yürütülebilir dosyası gerekli ortam değişkenleri başlatılıyor. Bu, yalnızca Service Fabric programlama modeli yazılmış yürütülebilir dosyalar için sınırlı değildir. Örneğin, bir node.js uygulamasını dağıtmak için yapılandırılmış bazı ortam değişkenleri npm.exe gerekir.
* Güvenlik sertifikaları yükleyerek erişim denetimini ayarlama.

Nasıl yapılandırılacağı hakkında daha fazla ayrıntı için **SetupEntryPoint**, bkz: [hizmet Kurulum giriş noktası için ilkeyi yapılandırın](service-fabric-application-runas-security.md)

**EnvironmentVariables** (ayarlarsanız önceki örnekte) Bu kod paketi için ayarlanan ortam değişkenlerini listesini sağlar. Ortam değişkenleri geçersiz kılınmış içinde `ApplicationManifest.xml` farklı hizmet örnekleri için farklı değerler sağlamak için. 

**Nın DataPackage** (ayarlarsanız önceki örnekte) tarafından adlı bir klasör bildirir **adı** işlem tarafından çalışma zamanında kullanılacak rastgele statik verileri içeren öznitelik.

**ConfigPackage** tarafından adlı bir klasör bildirir **adı** içeren öznitelik, bir *Settings.xml* dosya. Ayarlar dosyası işlemi geri çalışma zamanında okur kullanıcı tanımlı, anahtar-değer çifti ayarları bölümleri içerir. Yalnızca yükseltme sırasında **ConfigPackage** **sürüm** değişti, çalışan işlem yeniden sonra. Bunun yerine, bir geri çağırma bunlar dinamik olarak yeniden yüklendi için yapılandırma ayarları değişti işlem bildirir. İşte bir örnek *Settings.xml* dosyası:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

**Kaynakları**, uç noktaları gibi hizmet tarafından bildirilen ve değiştirilen derlenmiş kod değiştirmeden için kullanılır.  Hizmet bildiriminde belirtilen kaynaklara erişimi aracılığıyla denetlenebilir **güvenlik grubuna** uygulama bildiriminde.  Zaman bir **Endpoint** kaynak hizmet bildiriminde tanımlanır, Service Fabric bir bağlantı noktası açıkça belirtilmediğinde Bu bağlantı noktalarını ayrılmış uygulama bağlantı noktası aralığından atar.  Daha fazla bilgi edinin [belirterek veya geçersiz kılma uç nokta kaynakları](service-fabric-service-manifest-resources.md).


<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>Bir uygulamada ApplicationManifest.xml açıklayın
Uygulama bildirimini bildirimli olarak uygulama türü ve sürümü açıklar. Hizmet oluşturma meta veri şema, örnek sayısı/çoğaltma faktörü, güvenlik/yalıtım İlkesi, yerleştirme kısıtlamaları, yapılandırma geçersiz kılmaları ve bağlı hizmet türü bölümleme kararlı adları gibi belirtir. Uygulama üzerine yerleştirilen yük dengeleyici etki alanları da açıklanmaktadır.

Bu nedenle, bir uygulama bildirimi uygulama düzeyinde öğeleri açıklar ve uygulama türü oluşturmak için bir veya daha fazla hizmet bildirimlerini başvuruyor. Uygulama bildirimi için işte [örnek uygulama oylama](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

Gibi hizmet bildirimlerini **sürüm** öznitelikleri yapılandırılmamış dizelerdir ve sistem tarafından çözümlenmemiş. Sürüm öznitelikleridir de sürüme her bileşenin yükseltmeleri için kullanılır.

**Parametreleri** uygulama bildirimi kullanılan parametreleri tanımlar. Uygulama instatiated olduğunda ve uygulama veya hizmet yapılandırma ayarları geçersiz kılabilir bu parametrelerin değerlerini sağlanabilir.  Değer uygulama örnek oluşturma sırasında değiştirilmez varsayılan parametre değeri kullanılır. Farklı bir uygulama ve hizmet parametreleri tek tek ortamlar için korumak öğrenmek için bkz: [birden çok ortamlar için uygulama parametreleri yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

**ServiceManifestImport** bu uygulama türü oluşturan hizmet bildirimlerini başvurular içeriyor. Bir uygulama bildirimi birden çok hizmet bildirimi içeri aktarmalar içerebilir, her biri bağımsız olarak sürümlü olabilir. Alınan hizmet bildirimlerini hangi hizmet türleri içindeki bu uygulama türü geçerli belirler. ServiceManifestImport içinde ServiceManifest.xml dosyalarında Settings.xml ve ortam değişkenleri içindeki yapılandırma değerleri geçersiz. **İlkeleri** (önceki örnekte ayarlı değil) uç noktası bağlama, güvenlik ve erişim ve paket paylaşımı alınan hizmet bildirimlere ayarlanabilir.  Daha fazla bilgi için bkz: [uygulamanız için güvenlik ilkelerini yapılandırmak](service-fabric-application-runas-security.md).

**DefaultServices** bir uygulama bu uygulama türü karşı örneği olduğunda, otomatik olarak oluşturulan hizmet örnekleri bildirir. Varsayılan Hizmetleri yalnızca kolaylık sağlamak ve oluşturulduktan sonra her açısından normal services gibi davranır. Bunlar, herhangi bir uygulama örneği Hizmetleri'nde birlikte yükseltilir ve de kaldırılabilir. Bir uygulama bildirimi birden fazla varsayılan hizmet içerebilir.

**Sertifikaları** (ayarlarsanız önceki örnekte) için kullanılan sertifikaları bildirir [Kurulum HTTPS uç noktalarının](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) veya [uygulama bildiriminde parolaları şifrelemek](service-fabric-application-secret-management.md).

**İlkeleri** (ayarlarsanız önceki örnekte) açıklayan günlük toplama [Çalıştır varsayılan](service-fabric-application-runas-security.md), [sistem durumu](service-fabric-health-introduction.md#health-policies), ve [güvenlik erişimi](service-fabric-application-runas-security.md) belirlenmiş ilkeleri Uygulama düzeyi.

**Asıl adlar** (önceki örnekte ayarlı değil) tanımlamak için gerekli olan güvenlik sorumlularının (kullanıcılar veya gruplar) [çalışma Hizmetleri ve güvenli hizmet kaynakları](service-fabric-application-runas-security.md).  İlkeleri başvuruda bulunulamıyor **ilkeleri** bölümler.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Sonraki adımlar
- [Bir uygulama paketi](service-fabric-package-apps.md) ve dağıtmak hazır olun.
- [Dağıtma ve uygulamaları kaldırma](service-fabric-deploy-remove-applications.md).
- [Parametreler ve farklı uygulama örnekleri için ortam değişkenleri yapılandırma](service-fabric-manage-multiple-environment-app-configuration.md).
- [Uygulamanız için güvenlik ilkelerini yapılandırmak](service-fabric-application-runas-security.md).
- [HTTPS uç noktalarının Kurulum](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service).
- [Uygulama bildiriminde parolaları şifrelemek](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



