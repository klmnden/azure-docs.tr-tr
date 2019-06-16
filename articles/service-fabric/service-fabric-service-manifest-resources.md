---
title: Service Fabric hizmet uç noktalarını belirtme | Microsoft Docs
description: Uç nokta HTTPS uç noktaları ayarlama dahil olmak üzere bir hizmet bildirimi kaynakları nasıl
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: da36cbdb-6531-4dae-88e8-a311ab71520d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 8707a9cb90afe1bf72f3aef6377f8ada409a1c64
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837768"
---
# <a name="specify-resources-in-a-service-manifest"></a>Bir hizmet bildiriminde kaynakları belirtme
## <a name="overview"></a>Genel Bakış
Hizmet bildirimi, hizmet tarafından bildirilen ve değiştirilen derlenmiş kodunu değiştirmeden için kullanılan kaynaklar sağlar. Azure Service Fabric hizmeti için uç nokta kaynakların yapılandırmasını destekler. Hizmet bildiriminde belirtilen kaynaklara erişimi, IDAP uygulama bildiriminde aracılığıyla denetlenebilir. Kaynak bildirimi, dağıtım sırasında hizmetin yeni bir yapılandırma mekanizması tanıtmak gerekmez anlamı değiştirilmesi bu kaynakları sağlar. ServiceManifest.xml dosyasına ilişkin şema tanımı ile Service Fabric SDK'sı yüklü olduğundan ve araçları *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Uç Noktalar
Hizmet bildiriminde tanımlanan uç nokta kaynağı oluşturduğunuzda, Service Fabric açıkça bir bağlantı noktası belirtilmezse ayrılmış uygulama bağlantı noktası aralığından bağlantı noktaları atar. Örneğin, uç noktada Ara *ServiceEndpoint1* sonra bu paragrafın sağlanan bildirimi kod parçacığı belirtilen. Ayrıca, hizmetler kaynağın belirli bir bağlantı noktası isteyebilir. Çoğaltmaları aynı düğümde çalışan bir hizmetin bağlantı noktası paylaşma sırada farklı bir bağlantı noktası numaraları, farklı küme düğümleri üzerinde çalışan hizmet çoğaltmalardan atanabilir. Hizmet çoğaltmaları daha sonra bu bağlantı noktaları gerektiğinde çoğaltma ve istemci isteklerini dinlemek için kullanabilirsiniz.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Bir tek hizmet paketinde birden çok kod paketleri vardır sonra kod paketi ayrıca içinde başvurulması gereken **uç noktaları** bölümü.  Örneğin, varsa **ServiceEndpoint2a** ve **ServiceEndpoint2b** farklı kod paketleri, her uç noktaya karşılık gelen kod paketi başvuran aynı hizmet paketi uç noktalarının olduğunu gösterir. şu şekilde açıklığa kavuşturuldu:

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint2a" Protocol="http" Port="802" CodePackageRef="Code1"/>
    <Endpoint Name="ServiceEndpoint2b" Protocol="http" Port="801" CodePackageRef="Code2"/>
  </Endpoints>
</Resources>
```

Başvurmak [durum bilgisi olan Reliable Services yapılandırma](service-fabric-reliable-services-configuration.md) (settings.xml) uç noktalar başvuran yapılandırma paketi ayarları hakkında daha fazla dosya okunamıyor.

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Örnek: hizmetiniz için bir HTTP uç noktası belirtme
Aşağıdaki hizmet bildirimi bir TCP uç nokta kaynağı ve iki HTTP uç noktası kaynakları tanımlayan &lt;kaynakları&gt; öğesi.

HTTP uç noktalarını otomatik olarak Service Fabric tarafından ACL misiniz olursunuz.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Örnek: hizmetiniz için bir HTTPS uç noktası belirtme
HTTPS protokolünü, sunucu kimlik doğrulaması sağlar ve ayrıca istemci-sunucu iletişimi şifrelemek için kullanılır. Service Fabric hizmetinizi HTTPS'yi etkinleştirmek için protokolü belirtmek *Kaynakları -> uç noktalar -> uç nokta* uç nokta için daha önce gösterildiği gibi hizmet bildiriminin *ServiceEndpoint3*.

> [!NOTE]
> Bir hizmetin Protokolü uygulama yükseltme sırasında değiştirilemez. Yükseltme sırasında değiştirilir, bir değişiklik olur.
> 

> [!WARNING] 
> HTTPS kullanırken, aynı bağlantı noktası ve aynı düğüme dağıtılan sertifika farklı hizmet örnekleri (uygulamayı bağımsız olarak) için kullanmayın. Farklı uygulama örneklerinin aynı bağlantı noktası kullanarak iki farklı hizmet yükseltme bir yükseltme hatasına neden olur. Daha fazla bilgi için [HTTPS uç noktaları ile birden çok uygulama yükseltme ](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints).
>

HTTPS için ayarlanacak ihtiyacınız ApplicationManifest bir örnek aşağıda verilmiştir. Sertifikanızın parmak izi belirtilmelidir. EndpointRef EndpointResource ServiceManifest, HTTPS protokolünü ayarlayın, bir başvurudur. Birden fazla EndpointCertificate ekleyebilirsiniz.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_ ]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```

Linux kümeleri için **MY** varsayılan klasöre depolamak **/var/lib/sfcerts**.


## <a name="overriding-endpoints-in-servicemanifestxml"></a>Uç ServiceManifest.xml geçersiz kılma

İçinde ApplicationManifest ConfigOverrides bölümüne bir eşdüzey olacak bir ResourceOverrides bölümü ekleyin. Bu bölümde, hizmet bildiriminde belirtilen kaynaklar bölümünde uç noktalar bölümü için geçersiz kılma belirtebilirsiniz. Geçersiz kılma uç çalışma zamanı desteklenir 5.7.217/SDK 2.7.217 ve daha yüksek.

Uç noktası ServiceManifest ApplicationParameters değişiklik ApplicationManifest aşağıdaki gibi kullanarak, geçersiz kılmak için:

Yeni bir bölüm "ResourceOverrides" Servicemanifestımport bölümüne ekleyin.

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="ServiceEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
        <Endpoint Name="ServiceEndpoint1" Port="[Port1]" Protocol="[Protocol1] "/>
      </Endpoints>
    </ResourceOverrides>
        <Policies>
           <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint"/>
        </Policies>
  </ServiceManifestImport>
```

Aşağıdaki parametreleri ekleyin:

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

Uygulama dağıtımı sırasında bu değerleri ApplicationParameters geçirebilirsiniz.  Örneğin:

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

Not: ApplicationParameters boş olan değerleri girin, biz ServiceManifest içinde karşılık gelen Uçnoktaadı için sağlanan varsayılan değeri dönün.

Örneğin:

Eğer belirtilen ımagename'i

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

Ve uygulama parametreleri Port1 ve Protocol1 değeri null veya boş. Bağlantı noktasının hala ServiceFabric tarafından belirlenir. Ve tcp Protokolü olacaktır.

Yanlış bir değer belirtmeniz varsayalım. Gibi bağlantı noktası için bir dize değeri "Foo" yerine tamsayı belirttiğiniz  Yeni ServiceFabricApplication komutu, bir hata ile başarısız olur: 'ResourceOverrides' bölümündeki ' Port1' name 'ServiceEndpoint1' özniteliği geçersiz kılma parametresi geçersiz. Belirtilen değer 'Foo' ve 'int' gereklidir.
