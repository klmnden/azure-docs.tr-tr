---
title: Azure Service Fabric'te parametrelerini kullanarak bir hizmet bağlantı noktası numarasını belirtme | Microsoft Docs
description: Service Fabric'te uygulama bağlantı noktasını belirtmek için parametreleri kullanmayı gösterir
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
ms.openlocfilehash: d69e02126564388bf045693b9960e6e574307641
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60720255"
---
# <a name="how-to-specify-the-port-number-of-a-service-using-parameters-in-service-fabric"></a>Service Fabric'te parametrelerini kullanarak bir hizmet bağlantı noktası numarasını belirtme

Bu makalede, Visual Studio kullanarak Service Fabric'te parametrelerini kullanarak bir hizmet bağlantı noktası numarasını belirtin işlemini göstermektedir.

## <a name="procedure-for-specifying-the-port-number-of-a-service-using-parameters"></a>Yordam parametreleri kullanarak bir hizmet bağlantı noktası sayısını belirtmek için

Bu örnekte, asp.net core web API'niz bir parametre kullanarak bağlantı noktası numarasını ayarlayın.

1. Visual Studio'yu açın ve yeni bir Service Fabric uygulaması oluşturma.
1. Durum bilgisi olmayan ASP.NET Core şablonu seçin.
1. Web API'nizi seçin.
1. ServiceManifest.xml dosyasını açın.
1. Hizmetiniz için belirtilen bitiş noktası adını not edin. `ServiceEndpoint` varsayılan değerdir.
1. ApplicationManifest.xml dosyasını açın
1. İçinde `ServiceManifestImport` öğesi, yeni bir `RessourceOverrides` öğeyle uç ServiceManifest.xml dosyanıza bir başvuru.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. İçinde `Endpoint` öğesi, bir parametre kullanarak herhangi bir öznitelik şimdi geçersiz. Bu örnekte, belirttiğiniz `Port` kullanarak köşeli ayraç - Örneğin, bir parametre adı ayarlayın `[MyWebAPI_PortNumber]`

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint" Port="[MyWebAPI_PortNumber]"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. ApplicationManifest.xml dosyasının hala, ardından parametresinde belirttiğiniz `Parameters` öğesi

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" />
      </Parameters>
    ```

1. Ve tanımlayan bir `DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="8080" />
      </Parameters>
    ```

1. ApplicationParameters klasörü açın ve `Cloud.xml` dosyası
1. Uzak bir kümeye yayımlama sırasında kullanılacak farklı bir bağlantı noktası belirtmek için bu dosyaya bağlantı noktası numarası ile parametre ekleyin.

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" Value="80" />
      </Parameters>
    ```

Profili Visual Studio'dan myfirstcontainer kullanarak uygulamanızı yayımlama yayımlanması sırasında hizmetiniz 80 numaralı bağlantı noktasını kullanmak üzere yapılandırılır. MyWebAPI_PortNumber parametresini belirtmeden bir uygulamayı dağıtırsanız, hizmet bağlantı noktası 8080 kullanır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları hakkında daha fazla bilgi için bkz: [birden çok ortamları makaleler için uygulamaları yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

Visual Studio içinde kullanılabilir olan diğer uygulama yönetim özellikleri hakkında daha fazla bilgi için bkz: [Visual Studio'da Service Fabric uygulamalarınızı yönetmek](service-fabric-manage-application-in-visual-studio.md).
