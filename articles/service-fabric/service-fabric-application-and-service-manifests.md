---
title: Azure Service Fabric uygulamaları ve Hizmetleri | Microsoft Docs
description: Service Fabric uygulamaları ve hizmetleri tanımlamak için bildirimleri nasıl kullanıldığını açıklar.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/19/2018
ms.author: atsenthi
ms.openlocfilehash: 5e93bb3b206fbef6beb09b7aca6df0742a80ccf1
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662151"
---
# <a name="service-fabric-application-and-service-manifests"></a>Service Fabric uygulama ve hizmet bildirimleri
Bu makalede tanımlanan ve tutulan ApplicationManifest.xml ve ServiceManifest.xml dosyalarını kullanarak Service Fabric uygulamaları ve hizmetleri nasıl açıklar.  Daha ayrıntılı örnekler için bkz: [uygulama ve hizmet bildirimi örnekleri](service-fabric-manifest-examples.md).  Bu bildirim dosyaları için XML Şeması belgelenen [ServiceFabricServiceModel.xsd şema belgeleri](service-fabric-service-model-schema.md).

> [!WARNING]
> Bildirim XML dosyası şeması, doğru alt öğelerinin sıralama zorlar.  Kısmi bir geçici çözüm olarak "C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd" yazma veya herhangi bir Service Fabric bildirimleri değiştirme Visual Studio'da açın. Bu, alt öğelerinin sırasını denetleyin olanak tanıyacak ve IntelliSense sağlar.

## <a name="describe-a-service-in-servicemanifestxml"></a>ServiceManifest.xml hizmetinde açıklayın
Hizmet bildirimi, hizmet türü ve sürümü bildirimli olarak tanımlar. Bu hizmet türü, sistem durumu özellikleri, Yük Dengeleme ölçümleri, hizmet ikili dosyaları ve yapılandırma dosyaları gibi hizmet meta verileri belirtir.  Başka bir deyişle, bir veya daha fazla hizmet türlerini desteklemek için bir hizmet paketi oluşturan kod, yapılandırma ve veri paketleri açıklar. Birden çok kod, yapılandırma ve bağımsız olarak tutulan olabilir veri paketleri, bir hizmet bildirimi içerebilir. ASP.NET Core web ön uç hizmeti için bir hizmet bildirimi işte [Voting örnek uygulamasını](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) (ve bazı [daha ayrıntılı örnekler](service-fabric-manifest-examples.md)):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
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

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
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

**Sürüm** öznitelikleri yapılandırılmamış dizelerdir ve sistem tarafından ayrıştırılan değil. Sürüm özniteliklerinin sürümüne her bileşenin yükseltmeleri için kullanılır.

**ServiceTypes** hangi hizmet türleri tarafından desteklendiğini bildirir **CodePackages** bu katıştırır. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Sonuçta elde edilen işlemler, çalışma zamanında desteklenen hizmet türlerinin kaydetme beklenir. Hizmet türlerini, bildirim düzeyini ve kod paket düzeyinde bildirilir. Birden çok kod paketleri olduğunda, sistemin bildirilen hizmet türlerini herhangi biri için görünür olduğunda bu nedenle bunlar tüm etkinleştirilir.

Tarafından belirtilen yürütülebilir **EntryPoint** genellikle uzun süre çalışan hizmet yöneticisidir. **SetupEntryPoint** Service Fabric ile aynı kimlik bilgileri ile birlikte çalıştıran bir ayrıcalıklı giriş noktasıdır (genellikle *LocalSystem* hesabı) önce başka bir giriş noktası.  Ayrı bir Kurulum giriş noktası varlığını, hizmet ana bilgisayarı uzun sürelerle yüksek ayrıcalıklarla çalıştır gereğini ortadan kaldırır. Tarafından belirtilen yürütülebilir **EntryPoint** çalıştırıldıktan **SetupEntryPoint** başarıyla çıkılıyor. İşlemini her zamankinden sonlandırır veya çöküyor, sonuçta elde edilen işlem yeniden ve izlendiği (yeniden başlayarak **SetupEntryPoint**).  

Kullanma için tipik senaryoları **SetupEntryPoint** hizmet başlatılmadan önce çalıştırılabilir çalıştırın veya yükseltilmiş ayrıcalıklarla bir işlem gerçekleştirdiğinizde şunlardır. Örneğin:

* Ayarlama ve hizmet yürütülebilir gereken ortam değişkenlerini başlatılıyor. Bu, yalnızca Service Fabric programlama modelleri yazılan yürütülebilir dosyalar için sınırlı değildir. Örneğin, bir node.js uygulaması dağıtmak için yapılandırılmış bazı ortam değişkenlerini npm.exe gerekir.
* Erişim denetimi, güvenlik sertifikalarını yükleyerek ayarlama.

SetupEntryPoint yapılandırma hakkında daha fazla bilgi için bkz. [Hizmet Kurulumu giriş noktası için ilke yapılandırma](service-fabric-application-runas-security.md)

**EnvironmentVariables** (yukarıdaki örnekte ayarlanmadıysa), bu kod paketi için ayarlanan ortam değişkenlerinin bir listesini sağlar. Ortam değişkenlerini kılınabilir `ApplicationManifest.xml` farklı hizmet örnekleri için farklı değerler sağlamak için. 

**Nın DataPackage** (yukarıdaki örnekte ayarlanmadıysa) tarafından adlandırılan bir klasöre bildirir **adı** çalışma zamanında işlem tarafından kullanılacak rastgele statik verileri içeren bir öznitelik.

**ConfigPackage** tarafından adlandırılan bir klasöre bildirir **adı** içeren öznitelik, bir *Settings.xml* dosya. Ayarlar dosyası, çalışma zamanında işlem okuyan kullanıcı tanımlı, anahtar-değer çifti ayarları bölümlerini içerir. Yalnızca yükseltme sırasında **ConfigPackage** **sürüm** değişti, çalışan işlem yeniden sonra. Bunun yerine, bunlar dinamik olarak yeniden yüklenebilir, böylece yapılandırma ayarları değişti işlemi bir geri çağırma bildirir. İşte bir örnek *Settings.xml* dosyası:

```xml
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

Service Fabric hizmeti **uç nokta** bir Service Fabric kaynak; örneğidir Bir Service Fabric kaynak derlenmiş kodunu değiştirmeden, bildirilen/değiştirilmiş olabilir. Hizmet bildiriminde belirtilen Service Fabric kaynaklarına erişimi aracılığıyla denetlenebilir **IDAP** uygulama bildiriminde. Hizmet bildiriminde tanımlanan uç nokta kaynağı oluşturduğunuzda, Service Fabric açıkça bir bağlantı noktası belirtilmezse ayrılmış uygulama bağlantı noktası aralığından bağlantı noktaları atar. Daha fazla bilgi edinin [belirterek veya uç nokta kaynakları geçersiz kılma](service-fabric-service-manifest-resources.md).


<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>ApplicationManifest.xml uygulamada açıklayın
Uygulama bildirimini bildirimli olarak uygulama türü ve sürümü açıklanmaktadır. Bu hizmet oluşturma meta verileri düzeni, örnek sayısı/çoğaltma faktörü, güvenlik/yalıtım İlkesi, yerleştirme kısıtlamaları, yapılandırma geçersiz kılmalar ve bağlı hizmet türlerini bölümleme kararlı adları gibi belirtir. Uygulamanın üzerine yerleştirilen yük dengeleyici etki alanları da açıklanmaktadır.

Bu nedenle, bir uygulama bildirimi, uygulama düzeyinde öğeleri açıklar ve uygulama türü oluşturmak için bir veya daha fazla hizmet bildirimleri başvurur. Uygulama bildirimi için işte [Voting örnek uygulamasını](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) (ve bazı [daha ayrıntılı örnekler](service-fabric-manifest-examples.md)):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

Hizmet bildirimleri gibi **sürüm** öznitelikleri yapılandırılmamış dizelerdir ve sistem tarafından ayrıştırılan değil. Sürüm öznitelikleridir de sürüme her bileşenin yükseltmeleri için kullanılır.

**Parametreleri** uygulama bildirimini kullanılan parametreleri tanımlar. Bu parametreleri değerleri uygulama oluşturulur ve uygulama veya hizmet yapılandırma ayarları geçersiz kılabilirsiniz sağlanabilir.  Değer, uygulama Örnekleme sırasında değiştirilmezse, varsayılan parametre değeri kullanılır. Farklı uygulama ve hizmet parametreleri tek tek ortamlar için öğrenmek için bkz. [birden çok ortam için uygulama parametrelerini yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

**Servicemanifestımport** bu uygulama türü oluşturan hizmet bildirimleri için başvurular içerir. Her biri bağımsız olarak tutulan olabilir, bir uygulama bildirimi birden çok hizmet bildirim içeri aktarma içerebilir. İçeri aktarılan hizmet bildirimleri, hangi hizmet türleri içindeki bu uygulama türü için geçerli olduğunu belirleyin. Servicemanifestımport içinde ServiceManifest.xml dosyalarında Settings.xml ve ortam değişkenlerini yapılandırma değerleri geçersiz kılar. **İlkeleri** (yukarıdaki örnekte ayarlı değil) uç noktası bağlama, güvenlik ve erişim ve paket paylaşımı içeri aktarılan hizmet bildirimleri üzerinde ayarlanabilir.  Daha fazla bilgi için [uygulamanıza yönelik güvenlik ilkeleri yapılandırma](service-fabric-application-runas-security.md).

**DefaultServices** bir uygulama bu uygulama türü karşı örneği olduğunda otomatik olarak oluşturulan hizmet örnekleri bildirir. Varsayılan Hizmetleri bir kolaylık ve oluşturulduktan sonra her açısından normal Hizmetleri gibi davranır. Bunlar herhangi bir uygulama örneği Hizmetleri'nde ile birlikte yükseltilir ve de kaldırılabilir. Bir uygulama bildirimi birden çok varsayılan hizmet içerebilir.

**Sertifikaları** (yukarıdaki örnekte ayarlanmadıysa) için kullanılan sertifikaları bildirir [Kurulum HTTPS uç noktaları](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) veya [uygulama bildiriminde parolaları şifrelemek](service-fabric-application-secret-management.md).

**İlkeleri** (yukarıdaki örnekte ayarlanmadıysa) açıklayan günlük toplama [varsayılan Çalıştır](service-fabric-application-runas-security.md), [sistem durumu](service-fabric-health-introduction.md#health-policies), ve [güvenlik erişim](service-fabric-application-runas-security.md) ilkeleri en ayarlamak için Uygulama düzeyi.

**İlkeleri** (yukarıdaki örnekte ayarlı değil) tanımlamak için gereken güvenlik Sorumlular (kullanıcılar veya gruplar) [çalışma Hizmetleri ve güvenli hizmet kaynakları](service-fabric-application-runas-security.md).  İlkeleri başvurulan **ilkeleri** bölümler.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Sonraki adımlar
- [Bir uygulama paketi](service-fabric-package-apps.md) ve dağıtılmaya hazır olun.
- [Dağıtma ve uygulamaları kaldırma](service-fabric-deploy-remove-applications.md).
- [Parametreleri ve farklı uygulama örneklerinin için ortam değişkenlerini yapılandırma](service-fabric-manage-multiple-environment-app-configuration.md).
- [Uygulamanıza yönelik güvenlik ilkeleri yapılandırma](service-fabric-application-runas-security.md).
- [HTTPS uç noktaları Kurulum](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service).
- [Uygulama bildiriminde parolaları şifrelemek](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



