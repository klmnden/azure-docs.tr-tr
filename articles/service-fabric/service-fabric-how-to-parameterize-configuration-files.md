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
ms.openlocfilehash: 9057cdc22e277e4e12e9f439f3fbe0c5a5cda2a2
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900522"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Service Fabric yapılandırma dosyalarında Parametreleştirme nasıl

Bu makalede bir Service Fabric yapılandırma dosyasında Parametreleştirme gösterilmektedir.  Zaten birden çok ortam için uygulamaları yönetme temel kavramlar hakkında bilgi sahibi değilseniz, okuma [birden çok ortam için uygulamaları yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="procedure-for-parameterizing-configuration-files"></a>Yapılandırma dosyaları kümesini parametreleştirme yordamı

Bu örnekte, uygulama dağıtımınızı parametrelerini kullanarak bir yapılandırma değeri geçersiz.

1. Açık  *<MyService>\PackageRoot\Config\Settings.xml* hizmet projenizi dosyasında.
1. Bir yapılandırma parametresi ad ve değer, örneğin önbellek boyutu, 25 eşit aşağıdaki XML ekleyerek ayarlayın:

  ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
  ```

1. Dosyayı kaydedin ve kapatın.
1. Açık  *<MyApplication>\ApplicationPackageRoot\ApplicationManifest.xml* dosya.
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