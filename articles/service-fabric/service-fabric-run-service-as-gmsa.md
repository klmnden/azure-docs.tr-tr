---
title: Bir Azure Service Fabric hizmet gMSA hesabı altında Çalıştır | Microsoft Docs
description: Hizmet gMSA olarak tek başına Service Fabric Windows küme üzerinde çalıştırmayı öğrenin.
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
ms.openlocfilehash: 5c3781c2111fff7483a7fb65bd7b2e69c2011d18
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837751"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>Grup tarafından Yönetilen Hizmet Hesabı olarak hizmet çalıştırma
Windows Server tek başına küme üzerinde bir grup olarak yönetilen hizmet hesabı (gMSA) bir RunAs ilke kullanan bir hizmet çalıştırabilirsiniz.  Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesabın altına çalıştırın. Hatta bir paylaşılan barındırılan ortamda, farklı hesaplar altındaki uygulamaları çalıştıran bunları birbirinden daha güvenli kılacaktır. Active Directory şirket içi etki alanı ve değil Azure Active Directory (Azure AD) içinde kullandığına dikkat edin. Bir gmsa'yı kullanarak, parola veya yoktur uygulama bildiriminde depolanan şifrelenmiş parola.  Hizmet olarak da çalıştırabilirsiniz bir [Active Directory kullanıcı veya grup](service-fabric-run-service-as-ad-user-or-group.md).

Aşağıdaki örnekte adlı bir gMSA hesabının nasıl oluşturulacağını gösterir *svc Test$*; küme düğümlerine; Bu yönetilen hizmet hesabını dağıtma ve yapılandırma kullanıcı asıl adı.

Ön koşullar:
- Etki alanı, bir KDS kök anahtarı gerekir.
- Etki alanı, bir Windows Server 2012 veya üzeri işlev düzeyinde olması gerekir.

1. Bir grup yönetilen hizmet hesabı kullanarak oluşturmak bir Active Directory etki alanı yöneticisi olan `New-ADServiceAccount` komutunu olduğundan emin olun `PrincipalsAllowedToRetrieveManagedPassword` tüm service fabric küme düğümleri içerir. `AccountName`, `DnsHostName`, ve `ServicePrincipalName` benzersiz olması gerekir.

    ```powershell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. Her Service Fabric küme düğümleri (örneğin, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`) yükleyin ve gmsa'yı test.
    
    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. Kullanıcı asıl adı'nı yapılandırma ve kullanıcı başvurmak için RunAsPolicy yapılandırın.
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
          <ConfigOverrides />
          <Policies>
              <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
          </Policies>
        </ServiceManifestImport>
      <Principals>
        <Users>
          <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
        </Users>
      </Principals>
    </ApplicationManifest>
    ```

> [!NOTE] 
> Bir hizmete bir RunAs ilke uygulayın ve hizmet bildirimi uç noktası HTTP protokolü kaynaklarla bildirir, belirtmelisiniz bir **SecurityAccessPolicy**.  Daha fazla bilgi için [HTTP ve HTTPS Uç noktalara yönelik güvenlik erişim ilkesi atama](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adım olarak, bu makaleleri okuyun:
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
