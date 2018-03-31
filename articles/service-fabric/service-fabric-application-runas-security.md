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
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 3df5374911ee6381f25d08d23d565cdf8a7cd12f
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Bir hizmet bir yerel kullanıcı hesabı veya yerel sistem hesabı olarak çalıştırılması
Azure Service Fabric kullanarak, kümedeki farklı kullanıcı hesabı altında çalışan uygulamaları güvenliğini sağlayabilirsiniz. Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesap altında çalışır. Service Fabric de bir yerel kullanıcı hesabı veya uygulama bildirimi RunAs ilkedeki değiştirilerek yapılır yerel sistem hesabı altında uygulamaları çalıştırmak için yeteneği sağlar. Desteklenen yerel sistem hesabı türleri **YerelKullanıcı**, **NetworkService**, **Yerelhizmet**, ve **LocalSystem**.  Service Fabric Windows tek başına kümede çalıştırıyorsanız altında bir hizmeti çalıştırabilirsiniz [Active Directory etki alanı hesapları](service-fabric-run-service-as-ad-user-or-group.md) veya [Grup yönetilen hizmet hesapları](service-fabric-run-service-as-gmsa.md).

Ayrıca, tanımlayabilir ve kullanıcı grupları oluşturun, böylece bir veya daha fazla kullanıcı birlikte yönetilecek her gruba eklenebilir. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilen bazı ortak ayrıcalıklarına sahip olmanız gerekir yararlıdır.

> [!NOTE] 
> Bir hizmet için bir RunAs ilkesi uygulamak ve uç nokta kaynakları HTTP protokolü ile hizmet bildirimi bildirir, belirtmelisiniz bir **SecurityAccessPolicy**.  Daha fazla bilgi için bkz: [HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="create-local-user-groups"></a>Yerel kullanıcı grupları oluşturun
Bir gruba eklenecek bir veya daha fazla kullanıcıların kullanıcı grupları oluşturun ve tanımlayın. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilen bazı ortak ayrıcalıklara sahip olması gerekir yararlıdır. Aşağıdaki örnek adlı bir yerel Grup gösterir **LocalAdminGroup** yönetici ayrıcalıklarına sahip. İki kullanıcılar, Customer1 ve Customer2, bu yerel grubun üyeleri, bu uygulama bildirim örnekte oluşturulur:

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
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
```

## <a name="create-local-users"></a>Yerel kullanıcılar oluşturma
Uygulamasında bir hizmeti güvenli hale getirmek için kullanılan yerel bir kullanıcı oluşturabilirsiniz. Zaman bir **YerelKullanıcı** hesap türü belirtilen uygulama bildirimi ilkeleri bölümünde Service Fabric, uygulamanın dağıtıldığı makinelerde yerel kullanıcı hesaplarını oluşturur. Varsayılan olarak, bu hesapları (örneğin, aşağıdaki uygulama bildirim örnekte Customer3) uygulama bildiriminde belirtilenlerle aynı adı yok. Bunun yerine, dinamik olarak oluşturulur ve rastgele parolalara sahip.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Uygulamanın kullanıcı hesabı ve parolası (örneğin, NTLM kimlik doğrulamasını etkinleştir) tüm makinelerde aynı gerektiriyorsa, küme bildiriminde ayarlamalısınız **NTLMAuthenticationEnabled** true. Küme bildirimi de belirtmeniz gerekir bir **NTLMAuthenticationPasswordSecret** tüm makinelerde aynı parola oluşturmak için kullanılır.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

## <a name="assign-policies-to-the-service-code-packages"></a>Hizmet kodu paketler ilke ataması
**RunAsPolicy** bölümünde bir **ServiceManifestImport** kod paketi çalıştırmak için kullanılması gereken sorumluları bölümünden hesabını belirtir. Ayrıca ilkeleri bölümünde kullanıcı hesapları olan hizmet bildirimi kod paketi ilişkilendirir. Bu kurulum veya ana giriş noktaları için belirtebilirsiniz veya belirtebilirsiniz `All` her ikisini de uygulamak için. Aşağıdaki örnek, uygulanmakta olan farklı ilkeleri gösterir:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Varsa **EntryPointType** belirtilmezse, varsayılan olarak ayarlanmış `EntryPointType=”Main”`. Belirtme **SetupEntryPoint** belirli yüksek ayrıcalıklı kurulum işlemlerini sistem hesabı altında çalıştırmak istediğinizde kullanışlıdır. Daha fazla bilgi için bkz: [hizmet başlangıç komut dosyasını bir yerel kullanıcı veya sistem farklı çalıştır hesabı](service-fabric-run-script-at-service-startup.md). Gerçek servis kodu düşük ayrıcalıklı hesap altında çalışır.

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Varsayılan bir ilke tüm hizmet kod paketleri için geçerlidir
Kullandığınız **DefaultRunAsPolicy** bölüm tüm kodları için varsayılan bir kullanıcı hesabı paketleri belirtmek için belirli bir sahip değilseniz **RunAsPolicy** tanımlanmış. Bir uygulama tarafından kullanılan hizmet bildiriminde belirtilen kod paketleri çoğunu aynı kullanıcı altında çalıştırmanız gerekiyorsa, uygulamayı yalnızca bu kullanıcı hesabıyla varsayılan bir RunAs ilkesi tanımlayabilirsiniz. Kod paketi yoksa belirleyen aşağıdaki örnekte bir **RunAsPolicy** belirtilen kod paketi altında çalışması gereken **MyDefaultAccount** ilkeleri bölümünde belirtilen.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama modeli anlama](service-fabric-application-model.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Bir uygulamayı dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
