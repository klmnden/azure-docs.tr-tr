---
title: "Azure Service Fabric yapılandırma dosyalarında Parametreleştirme nasıl | Microsoft Docs"
description: "Service Fabric yapılandırma dosyalarında Parametreleştirme gösterilmektedir"
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: 1e7d59ecb231440711b8ed3dc0b27a2b105890c4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Service Fabric yapılandırma dosyalarında Parametreleştirme nasıl

Bu makalede Service Fabric yapılandırma dosyasında Parametreleştirme gösterilmiştir.

## <a name="procedure-for-parameterizing-configuration-files"></a>Yapılandırma dosyaları kümesini parametreleştirme yordamı

Bu örnekte, uygulama dağıtımınızı parametrelerini kullanarak bir yapılandırma değeri geçersiz.

1. Config\Settings.xml dosyasını açın.
1. Bir yapılandırma parametresi aşağıdaki XML ekleyerek ayarlayın:

    ```xml
      <Section Name="MyConfigSection">
        <Parameter Name="CacheSize" Value="25" />
      </Section>
    ```

1. Dosyasını kaydedin ve kapatın.
1. `ApplicationManifest.xml` dosyasını açın.
1. Ekleme bir `ConfigOverride` öğesi, bir yapılandırma paketi, bölüm ve parametresi başvuruyor.

      ```xml
        <ConfigOverrides>
          <ConfigOverride Name="Config">
              <Settings>
                <Section Name="MyConfigSection">
                    <Parameter Name="CacheSize" Value="[Stateless1_CacheSize]" />
                </Section>
              </Settings>
          </ConfigOverride>
        </ConfigOverrides>
      ```

1. Hala ApplicationManifest.xml dosyasında sonra parametresinde belirttiğiniz `Parameters` öğesi

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" />
      </Parameters>
    ```

1. Ve tanımlayan bir`DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" DefaultValue="80" />
      </Parameters>
    ```

> [!NOTE]
> Bir ConfigOverride eklediğiniz durumda Service Fabric uygulama parametreleri ya da uygulama bildiriminde belirtilen varsayılan değer her zaman seçer.
>
>

Profil uygulamanızı Cloud.xml kullanarak Visual Studio'dan yayımlamak yayımladığınızda, hizmetiniz 80 numaralı bağlantı noktasını kullanacak şekilde yapılandırılır. Uygulama MyWebAPI_PortNumber parametresini belirtmeden dağıtırsanız, hizmet bağlantı noktası 8080 kullanır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları hakkında daha fazla bilgi için bkz: [birden çok ortamları makaleler için uygulamaları yönetmek](service-fabric-manage-multiple-environment-app-configuration.md).

Visual Studio'da kullanılabilir olan diğer uygulama yönetim özellikleri hakkında daha fazla bilgi için bkz: [Visual Studio'da, Service Fabric uygulamaları yönetmek](service-fabric-manage-application-in-visual-studio.md).