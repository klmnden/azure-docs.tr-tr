---
title: Yapılandırma dosyaları Azure Service fabric'te Parametreleştirme | Microsoft Docs
description: Service Fabric yapılandırma dosyalarında Parametreleştirme öğrenin.
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/09/2018
ms.author: mikhegn
ms.openlocfilehash: 3d03ca5cec2cef67862c2678b3b0a8f17b413787
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60482446"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Service Fabric yapılandırma dosyalarında Parametreleştirme nasıl

Bu makalede bir Service Fabric yapılandırma dosyasında Parametreleştirme gösterilmektedir.  Zaten birden çok ortam için uygulamaları yönetme temel kavramlar hakkında bilgi sahibi değilseniz, okuma [birden çok ortam için uygulamaları yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="procedure-for-parameterizing-configuration-files"></a>Yapılandırma dosyaları kümesini parametreleştirme yordamı

Bu örnekte, uygulama dağıtımınızı parametrelerini kullanarak bir yapılandırma değeri geçersiz.

1. Açık  *\<MyService > \PackageRoot\Config\Settings.xml* hizmet projenizi dosyasında.
1. Bir yapılandırma parametresi ad ve değer, örneğin önbellek boyutu, 25 eşit aşağıdaki XML ekleyerek ayarlayın:

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Dosyayı kaydedin ve kapatın.
1. Açık  *\<MyApplication > \ApplicationPackageRoot\ApplicationManifest.xml* dosya.
1. ApplicationManifest.xml dosyasına bir parametre ve varsayılan değer bildirmek `Parameters` öğesi.  Parametre adı hizmeti (örneğin, "MyService") adını içeren önerilir.

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. İçinde `ServiceManifestImport` bölümü ApplicationManifest.xml dosyasının ekleme bir `ConfigOverride` öğesi, yapılandırma paketi, bölüm ve parametresi başvuruyor.

   ```xml
    <ConfigOverrides>
      <ConfigOverride Name="Config">
          <Settings>
            <Section Name="MyConfigSection">
                <Parameter Name="CacheSize" Value="[MyService_CacheSize]" />
            </Section>
          </Settings>
      </ConfigOverride>
    </ConfigOverrides>
   ```

> [!NOTE]
> Bir ConfigOverride eklediğiniz durumda da, Service Fabric her zaman uygulama parametreleri veya uygulama bildiriminde belirtilen varsayılan değerin seçer.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio içinde kullanılabilir olan diğer uygulama yönetim özellikleri hakkında daha fazla bilgi için bkz: [Visual Studio'da Service Fabric uygulamalarınızı yönetmek](service-fabric-manage-application-in-visual-studio.md).