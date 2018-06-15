---
title: Azure Service Fabric hizmeti gMSA hesabı altında Çalıştır | Microsoft Docs
description: Hizmet gMSA olarak bir doku Windows hizmeti tek başına kümede çalışan öğrenin.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: mfussell
ms.openlocfilehash: 5f93285061708172b9b6ac40dc97fce08f7b2a86
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206722"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>Grup tarafından Yönetilen Hizmet Hesabı olarak hizmet çalıştırma
Bir Windows Server tek başına kümede bir grup olarak yönetilen hizmet hesabı (gMSA) bir RunAs ilke kullanan bir hizmet çalıştırabilirsiniz.  Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesap altında çalışır. Çalışan uygulamalar farklı hesaplar bile paylaşılan bir barındırılan ortamda, bunları birbirinden daha güvenli yapar. Bu Active Directory dahilindeki şirket içi etki alanı ve değil Azure Active Directory (Azure AD) kullandığını unutmayın. Bir gmsa'yı kullanarak, parola veya yoktur uygulama bildiriminde depolanan şifrelenmiş parola.  Bir hizmet olarak da çalıştırılabilir bir [Active Directory kullanıcı veya grup](service-fabric-run-service-as-ad-user-or-group.md).

Aşağıdaki örnek olarak adlandırılan bir gMSA hesabının nasıl oluşturulacağını gösterir *svc Test$*; Bu yönetilen hizmet hesabı küme düğümlerine; dağıtma ve kullanıcı asıl adı yapılandırma.

Ön koşullar:
- Etki alanı bir KDS kök anahtarı gerekir.
- Etki alanı bir Windows Server 2012 veya sonraki işlev düzeyinde olması gerekir.

1. Bir grup yönetilen hizmet hesabı kullanarak oluşturduğunuz Active Directory etki alanı yöneticisi olan `New-ADServiceAccount` komutunu ve emin `PrincipalsAllowedToRetrieveManagedPassword` service fabric küme düğümlerinin tümü içerir. `AccountName`, `DnsHostName`, ve `ServicePrincipalName` benzersiz olması gerekir.

    ```poweshell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. Her Service Fabric kümesi düğümleri (örneğin, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`) yükleyin ve gMSA test.
    
    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. Kullanıcı asıl adı yapılandırmak ve kullanıcı başvurmak için RunAsPolicy yapılandırın.
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
> Bir hizmet için bir RunAs ilkesi uygulamak ve uç nokta kaynakları HTTP protokolü ile hizmet bildirimi bildirir, belirtmelisiniz bir **SecurityAccessPolicy**.  Daha fazla bilgi için bkz: [HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adım olarak, aşağıdaki makaleler okuyun:
* [Uygulama modeli anlama](service-fabric-application-model.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
