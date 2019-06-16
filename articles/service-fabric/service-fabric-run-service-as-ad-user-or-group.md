---
title: Bir Azure Service Fabric hizmeti AD kullanıcısı veya grubu çalıştırın. | Microsoft Docs
description: Service Fabric Windows tek başına küme üzerinde bir hizmet bir Active Directory kullanıcı veya grup çalıştırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: dekapur
ms.openlocfilehash: 3e0bb62609f13430bd2beab2332a31983874eb8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837734"
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Bir Active Directory kullanıcı veya grup bir hizmet çalıştırma
Windows Server tek başına küme üzerinde bir hizmet, bir Active Directory kullanıcı veya grup RunAs ilke kullanılarak olarak çalıştırabilirsiniz.  Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesabın altına çalıştırın. Hatta bir paylaşılan barındırılan ortamda, farklı hesaplar altındaki uygulamaları çalıştıran bunları birbirinden daha güvenli kılacaktır. Active Directory şirket içi etki alanı ve değil Azure Active Directory (Azure AD) içinde kullandığına dikkat edin.  Hizmet olarak da çalıştırabilirsiniz bir [Grup yönetilen hizmet hesabı (gMSA)](service-fabric-run-service-as-gmsa.md).

Bir etki alanı kullanıcısı veya grubu kullanarak (örneğin, dosya paylaşımları) etki alanındaki izninizin olduğu diğer kaynaklara erişebilir.

Aşağıdaki örnekte adlı bir Active Directory kullanıcı *TestUser* kendi etki alanı ile bir sertifika kullanılarak şifrelenmiş parola adlı *My*. Kullanabileceğiniz `Invoke-ServiceFabricEncryptText` gizli şifreli metin oluşturmak için PowerShell komutu. Bkz: [Service Fabric uygulamalarında gizli anahtarları yönetme](service-fabric-application-secret-management.md) Ayrıntılar için.

Bir bant dışı yöntem kullanarak yerel makineye parolasının şifresi sertifika özel anahtarını dağıtmanız gerekir (Azure'da, Azure Resource Manager aracılığıyla budur). Ardından, Service Fabric hizmet paketi için bir makine dağıttığında, parolanın şifresini ve bu kimlik bilgileri altında çalıştırmak için Active Directory ile kimlik doğrulaması (kullanıcı adı ile birlikte).

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```

> [!NOTE] 
> Bir hizmete bir RunAs ilke uygulayın ve hizmet bildirimi uç noktası HTTP protokolü kaynaklarla bildirir, ayrıca belirtmelisiniz bir **SecurityAccessPolicy**.  Daha fazla bilgi için [HTTP ve HTTPS Uç noktalara yönelik güvenlik erişim ilkesi atama](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adım olarak, bu makaleleri okuyun:
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
