---
title: Bir AD kullanıcı veya grup bir Azure Service Fabric hizmeti çalıştırma | Microsoft Docs
description: Bir hizmet bir Active Directory kullanıcı veya grup bir doku Windows hizmeti tek başına kümede çalışan öğrenin.
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
ms.openlocfilehash: 7dc467d9977d536011e1c3fcf663fc335f90a492
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Bir Active Directory kullanıcı veya grup bir hizmet farklı çalıştır
Bir Windows Server tek başına kümesinde bir hizmet, bir Active Directory kullanıcı veya grup RunAs ilke kullanılarak çalıştırabilirsiniz.  Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesap altında çalışır. Çalışan uygulamalar farklı hesaplar bile paylaşılan bir barındırılan ortamda, bunları birbirinden daha güvenli yapar. Bu Active Directory dahilindeki şirket içi etki alanı ve değil Azure Active Directory (Azure AD) kullandığını unutmayın.  Bir hizmet olarak da çalıştırılabilir bir [Grup yönetilen hizmet hesabı (gMSA)](service-fabric-run-service-as-gmsa.md).

Bir etki alanı kullanıcısı veya grubu kullanarak izinleri verilmiş olması (örneğin, dosya paylaşımları) etki alanındaki diğer kaynaklara erişebilir.

Aşağıdaki örnek, bir Active Directory kullanıcı olarak adlandırılır ve gösterir *TestUser* kendi etki alanı ile bir sertifika kullanılarak şifrelenmiş parola adlı *MyCert*. Kullanabileceğiniz `Invoke-ServiceFabricEncryptText` gizli şifreli metin oluşturmak için PowerShell komutu. Bkz: [Service Fabric uygulamaları parolalarında yönetme](service-fabric-application-secret-management.md) Ayrıntılar için.

Bir bant dışı yöntem kullanarak yerel makineye parolasının şifresi sertifikanın özel anahtarı dağıtmanız gerekir (Azure'da, Azure Resource Manager aracılığıyla budur). Ardından, Service Fabric makineye hizmet paketini dağıtırken, gizli şifresini çözmek ve bu kimlik bilgileri altında çalıştırmak için Active Directory ile (kullanıcı adı ile birlikte) kimlik doğrulaması yapmak kullanabilirsiniz.

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
> Bir hizmet için bir RunAs ilkesi uygulamak ve uç nokta kaynakları HTTP protokolü ile hizmet bildirimi bildirir, de belirtmeniz gerekir bir **SecurityAccessPolicy**.  Daha fazla bilgi için bkz: [HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adım olarak, aşağıdaki makaleler okuyun:
* [Uygulama modeli anlama](service-fabric-application-model.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Bir uygulamayı dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
