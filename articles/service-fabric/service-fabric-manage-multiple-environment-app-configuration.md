---
title: "Service Fabric birden çok ortamlarında yönetme | Microsoft Docs"
description: "Service Fabric uygulamaları bir makine boyutu makineler binlerce aralık kümeleri üzerinde çalıştırılabilir. Bazı durumlarda, uygulamanız bu çeşitli ortamlar için farklı yapılandırma isteyeceksiniz. Bu makalede, ortam başına farklı uygulama parametrelerini tanımlamak alınmaktadır."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 671cc9b0f7b7b37fcf5b052f7e34bc98e66b2838
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a>Birden çok ortamlar için uygulama parametreleri yönetme
Bir binlerce makineler için herhangi bir yerde kullanarak Azure Service Fabric kümeleri oluşturabilirsiniz. Uygulama ikili dosyaları değişiklik yapmadan bu paylaşılabilen çok sayıda ortamlar arasında çalıştırılabilir olsa da, genellikle uygulama için dağıtımı makineler sayısına bağlı olarak farklı şekilde, yapılandırmak istediğiniz.

Basit bir örnek olarak göz önünde bulundurun `InstanceCount` durumsuz bir hizmet için. Azure uygulamalarını çalıştırırken genellikle "-1" özel değeri için bu parametreyi ayarlayın istersiniz. Bu yapılandırma, hizmetiniz her düğümde Küme (veya bir yerleştirme kısıtlaması ayarlarsanız düğüm türü kümedeki her düğüm) çalıştığını sağlar. Ancak, birden çok işlem tek bir makinede aynı uç noktasında dinleme olamaz beri bu yapılandırma bir tek makineli küme için uygun değil. Bunun yerine, genellikle ayarladığınız `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Ortama özgü parametrelerini belirtme
Bu yapılandırma sorunun çözümü, parametreli varsayılan Hizmetleri ve belirli bir ortam için bu parametre değerlerini doldurun uygulama parametreleri dosyalarını kümesidir. Varsayılan hizmet ve uygulama parametreleri uygulama ve hizmet bildirimleri yapılandırılır. Şema tanımı ServiceManifest.xml ve ApplicationManifest.xml dosyaları için Service Fabric SDK'sı ile yüklenir ve araçların *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Varsayılan Hizmetleri
Service Fabric uygulamaları hizmet örnekleri koleksiyonu yapılır. Boş bir uygulama oluşturun ve tüm hizmet örnekleri dinamik olarak oluşturmak mümkün olsa da, uygulamaların çoğu uygulama örneği oluşturulduğunda, her zaman oluşturulması gereken Çekirdek Hizmetleri kümesine sahiptir. Bunlar, "varsayılan Hizmetleri" adlandırılır. Uygulama bildiriminde köşeli parantez içine dahil ortamı başına yapılandırması yer tutucular ile belirtilir:

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

Her adlandırılmış parametreleri uygulama bildirimi parametreleri öğe içinde tanımlanmış olması gerekir:

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

DefaultValue özniteliği belirli bir ortam için daha özel parametre olmaması durumunda kullanılacak değeri belirtir.

> [!NOTE]
> Tüm hizmet örneği parametreleri ortamı başına yapılandırması için uygun değildir. Yukarıdaki örnekte, hizmetin bölümleme düzeni LowKey ve HighKey değerlerini bölüm aralığı ortam veri etki alanının işlev olduğundan tüm hizmet örnekleri için açık olarak tanımlanır.
> 
> 

### <a name="per-environment-service-configuration-settings"></a>Ortam başına hizmet yapılandırma ayarları
[Service Fabric uygulama modeli](service-fabric-application-model.md) çalışma zamanında okunabilir özel anahtar-değer çiftleri içeren yapılandırma paketleri eklenecek hizmetleri sağlar. Bu ayarları değerlerini de ortamı tarafından belirterek Ayrıştırılan bir `ConfigOverride` uygulama bildiriminde.

Config\Settings.xml dosyasında aşağıdaki ayar olduğunu varsayalım `Stateful1` hizmeti:

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
Bu değer belirli bir uygulama/ortamı çifti için geçersiz kılmak için oluşturma bir `ConfigOverride` içeri aktardığınızda uygulama bildiriminde hizmet bildirimi.

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
Bu parametre yukarıda gösterildiği gibi ortamı tarafından sonra yapılandırılabilir. Bu uygulama bildirimi Parametreler bölümünde bildirme ve ortama özgü değerleri uygulama parametresi dosyalarında belirterek yapabilirsiniz.

> [!NOTE]
> Hizmet yapılandırma ayarları söz konusu olduğunda, burada bir anahtarın değerini ayarlanabilir üç yerde vardır: hizmeti yapılandırma paketi, uygulama bildirimi ve uygulama parametre dosyası. Service Fabric her zaman uygulama parametre dosyasından seçin (belirtilmişse) ilk sonra uygulama bildirimi ve son olarak yapılandırma paketi.
> 
> 

### <a name="setting-and-using-environment-variables"></a>Ayarlama ve ortam değişkenlerini kullanma 
Belirtin ve ServiceManifest.xml dosyasında ortam değişkenlerini ayarlama ve daha sonra bu örneği başına temelinde ApplicationManifest.xml dosyasında geçersiz.
Aşağıdaki örnek, iki ortam değişkenleri, bir değer kümesiyle gösterir ve diğer geçersiz kılınır. Bu yapılandırma geçersiz kılma işlemleri için kullanılan aynı şekilde ortam değişkenlerinin değerlerini ayarlamak için uygulama parametreleri kullanabilirsiniz.

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
Ortam değişkenleri ApplicationManifest.xml geçersiz kılmak için ServiceManifest ile kod paketinde başvuru `EnvironmentOverrides` öğesi.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 Adlı hizmet örneği oluşturulduktan sonra ortam değişkenleri koddan erişebilirsiniz. Örneğin C# ile şunları yapabilirsiniz

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a>Service Fabric ortam değişkenleri
Service Fabric kümesi her hizmet örneği için ortam değişkenleri içindeki oluşturdu. Ortam değişkenlerinin tam listesi aşağıdadır, burada Listedekilerin kalın hizmetinizde, diğer Service Fabric çalışma zamanı tarafından kullanılan kullanacağınız olanlar. 

* Fabric_ApplicationHostId
* Fabric_ApplicationHostType
* Fabric_ApplicationId
* **Fabric_ApplicationName**
* Fabric_CodePackageInstanceId
* **Fabric_CodePackageName**
* **Fabric_Endpoint_ [YourServiceName] TypeEndpoint**
* **Fabric_Folder_App_Log**
* **Fabric_Folder_App_Temp**
* **Fabric_Folder_App_Work**
* **Fabric_Folder_Application**
* Fabric_NodeId
* **Fabric_NodeIPOrFQDN**
* **Fabric_NodeName**
* Fabric_RuntimeConnectionAddress
* Fabric_ServicePackageInstanceId
* Fabric_ServicePackageName
* Fabric_ServicePackageVersionInstance
* FabricPackageFileName

Kod belows Service Fabric ortam değişkenleri listesinde gösterilmiştir
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
Adlı bir uygulama türü için ortam değişkenleri örnekleri aşağıda verilmiştir `GuestExe.Application` hizmet türü ile adlı `FrontEndService` yerel geliştirme makinenizde çalıştırdığınızda.

* **Fabric_ApplicationName fabric:/GuestExe.Application =**
* **Fabric_CodePackageName kodu =**
* **Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**
* **Fabric_NodeIPOrFQDN = localhost**
* **Fabric_NodeName _Node_2 =**

### <a name="application-parameter-files"></a>Uygulama parametre dosyaları
Service Fabric uygulaması projesi bir veya daha fazla uygulama parametreleri dosyalarını içerebilir. Bunların her birini, uygulama bildiriminde tanımlanan parametreler için özel değerler tanımlar:

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
Varsayılan olarak, yeni bir uygulama Local.1Node.xml, Local.5Node.xml ve Cloud.xml adlı üç uygulama parametreleri dosyalarını içerir:

![Çözüm Gezgini'nde uygulama parametre dosyaları][app-parameters-solution-explorer]

Bir parametre dosyası oluşturmak için yalnızca kopyalayıp mevcut bir yapıştırın ve yeni bir ad verin.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Dağıtım sırasında ortama özgü parametreleri tanımlama
Dağıtım sırasında uygulamanızla uygulamak için uygun parametre dosyası seçmeniz gerekir. Visual Studio'da yayımlama iletişim kutusunu veya PowerShell aracılığıyla bunu yapabilirsiniz.

### <a name="deploy-from-visual-studio"></a>Visual Studio'dan dağıtma
Visual Studio'da uygulamanız yayımladığınızda kullanılabilir parametre dosyalar listesinden seçebilirsiniz.

![Yayımla iletişim kutusunda bir parametre dosyası seçin][publishdialog]

### <a name="deploy-from-powershell"></a>Powershell'den dağıtma
`Deploy-FabricApplication.ps1` Uygulama projesi şablona dahil PowerShell betiğini bir yayımlama profili bir parametre olarak kabul eder ve PublishProfile uygulama parametreler dosyası için bir başvuru içeriyor.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Sonraki adımlar
Bu konuda açıklanan kavramları bazıları hakkında daha fazla bilgi için bkz: [Service Fabric teknik genel bakış](service-fabric-technical-overview.md). Visual Studio'da kullanılabilir olan diğer uygulama yönetim özellikleri hakkında daha fazla bilgi için bkz: [Visual Studio'da, Service Fabric uygulamaları yönetmek](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
