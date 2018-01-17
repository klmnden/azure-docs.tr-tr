---
title: "Azure Service Fabric parametrelerini kullanarak bir hizmet bağlantı noktası sayısını belirtmek nasıl | Microsoft Docs"
description: "Service Fabric bir uygulama için bağlantı noktası belirtmek için parametreleri kullanmayı gösterir"
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: aca5b6a476e9526498a5e4834aaa28eb73750562
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-specify-the-port-number-of-a-service-using-parameters-in-service-fabric"></a>Service Fabric parametrelerini kullanarak bir hizmet bağlantı noktası sayısını belirtme

Bu makale, Service Fabric parametrelerini kullanarak bir hizmet bağlantı noktası sayısını belirtmek Visual Studio kullanarak gösterilmiştir.

## <a name="procedure-for-specifying-the-port-number-of-a-service-using-parameters"></a>Yordam parametreleri kullanarak bir hizmet bağlantı noktası sayısını belirtmek için

Bu örnekte, asp.net çekirdek web API kullanarak bir parametre için bağlantı noktası numarasını ayarlayın.

1. Visual Studio'yu açın ve yeni bir Service Fabric uygulaması oluşturma.
1. Durum bilgisiz ASP.NET Core şablonu seçin.
1. Web API'ı seçin.
1. ServiceManifest.xml dosyasını açın.
1. Hizmet için belirtilen uç noktası adını not edin. Varsayılan değer `ServiceEndpoint`.
1. ApplicationManifest.xml dosyasını açın
1. İçinde `ServiceManifestImport` öğesi, yeni bir ekleme `RessourceOverrides` ServiceManifest.xml dosyanızı uç başvuru öğesi.

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

1. İçinde `Endpoint` öğesi, bir parametre kullanarak herhangi bir öznitelik şimdi kılabilirsiniz. Bu örnekte, belirttiğiniz `Port` ve köşeli ayraç - Örneğin, kullanarak bir parametre adı olarak ayarlayın`[MyWebAPI_PortNumber]`

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

1. Hala ApplicationManifest.xml dosyasında sonra parametresinde belirttiğiniz `Parameters` öğesi

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" />
      </Parameters>
    ```

1. Ve tanımlayan bir`DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="8080" />
      </Parameters>
    ```

1. ApplicationParameters klasörünü açın ve `Cloud.xml` dosyası
1. Uzak bir kümeye yayımlarken kullanılacak farklı bir bağlantı noktası belirtmek için bu dosyaya bağlantı noktası numarasıyla parametresini ekleyin.

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="80" />
      </Parameters>
    ```

Profil uygulamanızı Cloud.xml kullanarak Visual Studio'dan yayımlamak yayımladığınızda, hizmetiniz 80 numaralı bağlantı noktasını kullanacak şekilde yapılandırılır. Uygulama MyWebAPI_PortNumber parametresini belirtmeden dağıtırsanız, hizmet bağlantı noktası 8080 kullanır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları hakkında daha fazla bilgi için bkz: [birden çok ortamları makaleler için uygulamaları yönetmek](service-fabric-manage-multiple-environment-app-configuration.md).

Visual Studio'da kullanılabilir olan diğer uygulama yönetim özellikleri hakkında daha fazla bilgi için bkz: [Visual Studio'da, Service Fabric uygulamaları yönetmek](service-fabric-manage-application-in-visual-studio.md).