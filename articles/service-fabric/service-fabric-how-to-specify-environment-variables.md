---
title: Nasıl yapılır, Azure Service Fabric'te Hizmetleri için ortam değişkenlerini belirtin | Microsoft Docs
description: Service Fabric uygulamaları için ortam değişkenlerini kullanma işlemi gösterilmektedir
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: f75de635f08ae06db349387a436c636c149ec9f2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60720238"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Service Fabric'te Hizmetleri için ortam değişkenlerini belirtme

Bu makalede Service Fabric hizmeti veya kapsayıcı için ortam değişkenlerini belirleme gösterilmektedir.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Yordam hizmetler için ortam değişkenlerini belirtme

Bu örnekte, bir kapsayıcı için bir ortam değişkeni ayarlayın. Zaten sahip olduğunuz bir uygulama ve hizmet bildirimi varsayılır.

1. ServiceManifest.xml dosyasını açın.
1. İçinde `CodePackage` öğesi, yeni bir `EnvironmentVariables` öğesi ve bir `EnvironmentVariable` her ortam değişkeni için öğesi.

    ```xml
      <CodePackage Name="MyCode" Version="CodeVersion1">
      ...
        <EnvironmentVariables>
          <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
          <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentVariables>
      </CodePackage>
    ```

    Ortam değişkenleri uygulama bildiriminde geçersiz kılınabilir.

1. Uygulama bildiriminde ortam değişkenlerini geçersiz kılmak için kullanın `EnvironmentOverrides` öğesi.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları hakkında daha fazla bilgi için bkz: [birden çok ortamları makaleler için uygulamaları yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

Visual Studio içinde kullanılabilir olan diğer uygulama yönetim özellikleri hakkında daha fazla bilgi için bkz: [Visual Studio'da Service Fabric uygulamalarınızı yönetmek](service-fabric-manage-application-in-visual-studio.md).