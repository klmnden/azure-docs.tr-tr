---
title: Bir Azure Service Fabric hizmeti sistemi ve yerel güvenlik hesapları altında Çalıştır | Microsoft Docs
description: Sistem ve yerel güvenlik hesapları altında bir Service Fabric uygulaması çalıştırmayı öğrenin.  Güvenlik ilkeleri oluşturabilir ve hizmetlerinizi güvenli bir şekilde çalıştırmak için Çalıştır ilke uygulayabilirsiniz.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: mfussell
ms.openlocfilehash: 471e5a79600d6a963a4fa5b6cec8d2cc16137489
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Bir hizmet bir yerel kullanıcı hesabı veya yerel sistem hesabı olarak çalıştırılması
Azure Service Fabric kullanarak, kümedeki farklı kullanıcı hesabı altında çalışan uygulamaları güvenliğini sağlayabilirsiniz. Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesap altında çalışır. Service Fabric ayrıca yerel bir kullanıcı veya sistem hesabı altında uygulamaları çalıştırmak için yeteneği sağlar. Desteklenen yerel sistem hesabı türleri **YerelKullanıcı**, **NetworkService**, **Yerelhizmet**, ve **LocalSystem**.  Service Fabric Windows tek başına kümede çalıştırıyorsanız altında bir hizmeti çalıştırabilirsiniz [Active Directory etki alanı hesapları](service-fabric-run-service-as-ad-user-or-group.md) veya [Grup yönetilen hizmet hesapları](service-fabric-run-service-as-gmsa.md).

Hizmetleri veya güvenli kaynaklarında çalıştırmak için gerekli olan kullanıcı hesaplarını tanımlayan uygulama bildiriminde **sorumluları** bölümü. Ayrıca, tanımlayabilir ve kullanıcı grupları oluşturun, böylece bir veya daha fazla kullanıcı birlikte yönetilebilir. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilen ortak ayrıcalıkları gerekir yararlıdır.  Kullanıcılar, ardından belirli bir hizmet veya uygulamanın tüm hizmetlere uygulanan bir RunAs ilke başvurulur. 

Varsayılan olarak, ana giriş noktasını RunAs ilkesi uygulanır.  Gerekirse bir RunAs ilke Kurulum giriş noktası için aynı zamanda uygulayabilirsiniz [belirli yüksek ayrıcalıklı kurulum işlemlerini sistem hesabı altında Çalıştır](service-fabric-run-script-at-service-startup.md), veya her iki ana ve Kurulum giriş noktaları.  

> [!NOTE] 
> Bir hizmet için bir RunAs ilkesi uygulamak ve uç nokta kaynakları HTTP protokolü ile hizmet bildirimi bildirir, belirtmelisiniz bir **SecurityAccessPolicy**.  Daha fazla bilgi için bkz: [HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="run-a-service-as-a-local-user"></a>Bir hizmet yerel bir kullanıcı olarak çalıştırın
Uygulamasında bir hizmeti güvenli hale getirmek için kullanılan yerel bir kullanıcı oluşturabilirsiniz. Zaman bir **YerelKullanıcı** hesap türü belirtilen uygulama bildirimi ilkeleri bölümünde Service Fabric, uygulamanın dağıtıldığı makinelerde yerel kullanıcı hesaplarını oluşturur. Varsayılan olarak, bu hesaplar, uygulama bildiriminde belirtilenlerle aynı adları yok (örneğin, *Customer3* aşağıdaki uygulama bildirim örnekte). Bunun yerine, dinamik olarak oluşturulur ve rastgele parolalara sahip.

İçinde **RunAsPolicy** bölümünde bir **ServiceManifestImport**, kullanıcı hesabını belirtmek **sorumluları** hizmet kod paketi çalıştırmak için bölüm.  Aşağıdaki örnek, yerel bir kullanıcı oluşturmak ve ana giriş noktası için bir RunAs ilkesi uygulamak gösterilmektedir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="Customer3" />
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="create-a-local-user-group"></a>Bir yerel kullanıcı grubu oluşturun
Kullanıcı grupları oluşturun ve bir veya daha fazla kullanıcı grubuna ekleyin. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilen bazı ortak ayrıcalıklara sahip olması gerekir yararlıdır. Aşağıdaki uygulama bildirim örnek adlı bir yerel Grup gösterir *LocalAdminGroup* yönetici ayrıcalıklarına sahip. İki kullanıcı *Customer1* ve *Customer2*, bu yerel grubun üyesi yapılır. İçinde **ServiceManifestImport** bölümünde, ilkenin çalıştırmak için geçerli olduğu bir RunAs *Stateful1Pkg* kod paketi olarak *Customer2*.  Başka bir RunAs İlkesi çalıştırmak için uygulanan *Web1Pkg* kod paketi olarak *Customer1*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer2" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="Stateful1" ServicePackageActivationMode="ExclusiveProcess">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Groups>
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <User Name="Customer1">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <User Name="Customer2">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Varsayılan bir ilke tüm hizmet kod paketleri için geçerlidir
Kullandığınız **DefaultRunAsPolicy** bölüm tüm kodları için varsayılan bir kullanıcı hesabı paketleri belirtmek için belirli bir sahip değilseniz **RunAsPolicy** tanımlanmış. Bir uygulama tarafından kullanılan hizmet bildiriminde belirtilen kod paketleri çoğunu aynı kullanıcı altında çalıştırmanız gerekiyorsa, uygulamayı yalnızca bu kullanıcı hesabıyla varsayılan bir RunAs ilkesi tanımlayabilirsiniz. Kod paketi yoksa belirleyen aşağıdaki örnekte bir **RunAsPolicy** belirtilen kod paketi altında çalışması gereken **MyDefaultAccount** ilkeleri bölümünde belirtilen kullanıcı.  Desteklenen hesap YerelKullanıcı, NetworkService veya LocalSystem ve Yerelhizmet türleridir.  Ayrıca bir yerel kullanıcı veya hizmet kullanıyorsanız, hesap adı ve parola belirtin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <Policies>
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
  </Policies>
</ApplicationManifest>
```

## <a name="debug-a-code-package-locally-using-console-redirection"></a>Konsol yeniden yönlendirme özelliğini kullanarak yerel olarak bir kod paketi hata ayıklama
Bazen, çalışan bir hizmeti konsol çıktısı görmek hata ayıklama amacıyla yararlıdır. Çıktıyı bir dosyaya yazar hizmet bildiriminde Giriş noktasındaki bir konsol yeniden yönlendirme ilkesi ayarlayabilirsiniz. Dosya çıktısı adlı uygulama klasörüne yazılır **günlük** küme düğümünde burada uygulamanın dağıtıldığı ve çalıştırın. 

> [!WARNING]
> Hiçbir zaman bu uygulamayı yük devretme etkileyebileceğinden üretimde dağıtılan bir uygulamada konsol yeniden yönlendirme ilkesi kullanın. *Yalnızca* bunu yerel geliştirme ve hata ayıklama amacıyla kullanın.  
> 
> 

Aşağıdaki hizmet örnekte gösterildiği bir FileRetentionCount değerle konsol yeniden yönlendirmesini etkinleştirme bildirimi:

```xml
<CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
        <ConsoleRedirection FileRetentionCount="10"/>
      </ExeHost>
    </EntryPoint>
</CodePackage>

```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama modeli anlama](service-fabric-application-model.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Bir uygulamayı dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
