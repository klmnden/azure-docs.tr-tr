---
title: Bir Azure Service Fabric hizmetinin sistemi ve yerel güvenlik hesapları altında çalıştırmak | Microsoft Docs
description: Service Fabric uygulaması sistem ve yerel güvenlik hesapları altında çalıştırmayı öğrenin.  Güvenlik ilkeleri oluşturma ve güvenli bir şekilde hizmetlerinizi çalıştırmak için Çalıştır ilkesini uygulayın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: aljo
ms.openlocfilehash: 28cd1162d7cae2b3a16062bdf18a2971e1f05aad
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664446"
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Bir yerel kullanıcı hesabı veya yerel sistem hesabı bir hizmet çalıştırma
Azure Service Fabric kullanarak kümedeki farklı bir kullanıcı hesabı altında çalışan uygulamaların güvenliğini sağlayabilirsiniz. Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesabın altına çalıştırın. Service Fabric uygulamaları yerel bir kullanıcı veya sistem hesabı altında çalıştırma özelliği de sağlar. Desteklenen yerel sistem hesabı türleri **LocalUser**, **NetworkService**, **LocalService**, ve **LocalSystem**.  Service Fabric Windows tek başına küme üzerinde çalıştırıyorsanız, altında bir hizmeti çalıştırabileceğiniz [Active Directory etki alanı hesapları](service-fabric-run-service-as-ad-user-or-group.md) veya [Grup yönetilen hizmet hesapları](service-fabric-run-service-as-gmsa.md).

Uygulama bildiriminde Hizmetleri veya güvenli kaynaklara çalıştırmak için gereken kullanıcı hesaplarını tanımladığınız **sorumluları** bölümü. Ayrıca, tanımlamak ve kullanıcı grupları oluşturun, böylece bir veya daha fazla kullanıcı birlikte yönetilebilir. Bu, birden çok kullanıcı için farklı hizmet giriş noktaları vardır ve bunlar grubu düzeyinde kullanılabilir olan ortak ayrıcalıklara gerektiğinde kullanışlıdır.  Kullanıcılar, ardından belirli bir hizmet veya uygulamanın tüm hizmetler için uygulanan bir RunAs ilke başvurulur. 

Varsayılan olarak, ana giriş noktasını RunAs ilke uygulanır.  Gerekirse Kurulum Giriş noktasına RunAs İlkesi de uygulayabilirsiniz [belirli yüksek ayrıcalıklı kurulum işlemlerini bir sistem hesabı altında Çalıştır](service-fabric-run-script-at-service-startup.md), veya her iki ana ve Kurulum giriş noktaları.  

> [!NOTE] 
> Bir hizmete bir RunAs ilke uygulayın ve hizmet bildirimi uç noktası HTTP protokolü kaynaklarla bildirir, belirtmelisiniz bir **SecurityAccessPolicy**.  Daha fazla bilgi için [HTTP ve HTTPS Uç noktalara yönelik güvenlik erişim ilkesi atama](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="run-a-service-as-a-local-user"></a>Bir hizmet yerel kullanıcı olarak çalıştırın
Uygulama içinde bir hizmeti güvenli hale getirmek için kullanılan yerel bir kullanıcı oluşturabilirsiniz. Olduğunda bir **LocalUser** hesap türü, Service Fabric, uygulamanın dağıtıldığı makinelerde yerel kullanıcı hesaplarını oluşturur uygulama bildiriminin ilkeleri bölümünde belirtilir. Varsayılan olarak, bu hesaplar, uygulama bildiriminde belirtilenlerle aynı adları yoktur (örneğin, *Customer3* aşağıdaki uygulama bildirim örnekte). Bunun yerine, dinamik olarak oluşturulur ve rastgele parolalar vardır.

İçinde **RunAsPolicy** bölümü bir **Servicemanifestımport**, oluşturduğunuz kullanıcı hesabını belirtin **sorumluları** hizmet kod paketinin alınmalıdır.  Aşağıdaki örnek, yerel bir kullanıcı oluşturun ve ana giriş noktası bir RunAs ilkeyi uygulamak gösterilmektedir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
Kullanıcı grupları oluşturun ve bir veya daha fazla kullanıcı grubuna ekleyin. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilir olan belirli ortak ayrıcalıklara sahip olması için ihtiyaç duydukları kullanışlıdır. Aşağıdaki uygulama bildirim örnek adlı bir yerel Grup gösterir *LocalAdminGroup* yönetici ayrıcalıklarına sahip. İki kullanıcı *Customer1* ve *Customer2*, bu yerel grubun üyeleri yapılır. İçinde **Servicemanifestımport** bölümünde, ilkenin uygulandığı çalıştırmak için bir RunAs *Stateful1Pkg* kod paketi olarak *Customer2*.  Çalıştırmak için başka bir RunAs ilke uygulanır *Web1Pkg* kod paketi olarak *Customer1*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Tüm hizmet kod paketleri için varsayılan ilke geçerlidir
Kullandığınız **DefaultRunAsPolicy** paketleri tüm kodlar için varsayılan bir kullanıcı hesabı belirtmek için bölüm, belirli bir sahip değilseniz **RunAsPolicy** tanımlı. Bir uygulama tarafından kullanılan hizmet bildiriminde belirtilen kod paketleri çoğunu altında aynı kullanıcı çalıştırmanız gerekiyorsa, uygulama yalnızca söz konusu kullanıcı hesabı ile varsayılan bir RunAs ilkesi tanımlayabilirsiniz. Aşağıdaki örnek, bir kod paketi yoksa belirtir bir **RunAsPolicy** belirtilen kod paketi çalışması gereken **MyDefaultAccount** ilkeleri bölümünde belirtilen kullanıcı.  Desteklenen hesabı LocalUser, NetworkService veya LocalSystem ve LocalService türleridir.  Aynı zamanda bir yerel kullanıcı veya hizmet kullanırken, hesap adı ve parola belirtin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

## <a name="debug-a-code-package-locally-using-console-redirection"></a>Konsol yönlendirmesi kullanarak yerel olarak bir kod paketi hatalarını ayıklama
Bazen, çalışan bir hizmete konsol çıktısı görmek hata ayıklama amaçları için yararlıdır. Çıktıyı bir dosyaya yazar hizmet bildirimindeki Giriş noktasında, konsol yeniden yönlendirme ilkesi ayarlayabilirsiniz. Dosya çıktısı adlı uygulama klasörüne yazılır **günlük** küme düğümünde uygulama burada dağıtılan ve çalıştırın. 

> [!WARNING]
> Hiçbir zaman konsol yeniden yönlendirme ilkesi, bu uygulamanın yük devretme etkileyebileceğinden, üretimde dağıtılan bir uygulamada kullanın. *Yalnızca* bunu yerel geliştirme ve hata ayıklama amacıyla kullanın.  
> 
> 

Şu hizmet örnekte gösterildiği konsol yönlendirmesi FileRetentionCount değeriyle etkinleştirme bildirimi:

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
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
