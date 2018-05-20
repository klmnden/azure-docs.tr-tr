---
title: Azure Service Fabric birden çok ortamlarda uygulamalarını yönetme | Microsoft Docs
description: Azure Service Fabric uygulamaları kümelerinde bu aralık boyutu bir makineden makineler binlerce için çalıştırılabilir. Bazı durumlarda, uygulamanız bu çeşitli ortamlar için farklı yapılandırma isteyeceksiniz. Bu makalede, ortam başına farklı uygulama parametrelerini tanımlamak alınmaktadır.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 15ad606578970290cef440ec4efdd967ca0c0b32
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manage-applications-for-multiple-environments"></a>Birden çok ortam için uygulamaları yönetme

Azure Service Fabric kümeleri, birçok binlerce birinden herhangi bir yerde kullanarak kümeleri oluşturmak etkinleştir makineler. Çoğu durumda, kendiniz arasında birden çok küme yapılandırmaları uygulamanızı dağıtmak zorunda bulduğunuz: yerel geliştirme kümenizi, kümelenmiş bir paylaşılan geliştirme ve üretim kümenizi. Tüm bu kümeleri farklı ortamlarda kodunuzu çalıştırmak için sahip olarak kabul edilir. Uygulama ikili dosyaları değişiklik yapmadan bu arasında paylaşılabilen çok çalıştırılabilir, ancak genellikle farklı uygulama yapılandırmak istediğiniz.

İki basit örnekler göz önünde bulundurun:
  - Hizmetinizi tanımlı bir bağlantı noktasını dinler, ancak bu bağlantı noktasını ortamlar genelinde farklı olması gerekir
  - ortamlar genelinde bir veritabanı için farklı bağlama kimlik bilgilerini sağlamanız gerekir

## <a name="specifying-configuration"></a>Yapılandırma belirtme

Sağladığınız yapılandırma içinde iki kategoriye ayrılabilir:

- Hizmetlerinizi nasıl çalıştırmayı geçerlidir yapılandırma
  - Örneğin, bağlantı noktası numarası bir uç nokta veya bir hizmet örneklerinin sayısı
  - Bu yapılandırma uygulama veya hizmet bildirim dosyası belirtilen
- Uygulama kodunuz geçerlidir yapılandırma
  - Örneğin, bir veritabanı için bağlama bilgileri
  - Bu yapılandırma yapılandırma dosyalarının veya ortam değişkenleri yoluyla sağlanabilir

> [!NOTE]
> Uygulama ve hizmet tüm öznitelikleri dosya destek parametrelerini bildirimi.
> Bu durumda, dizeleri dağıtım akışınızın bir parçası değiştirerek yararlanmayı sahip. Visual Studio Team Services içinde uzantı Değiştir belirteçleri gibi kullanabilirsiniz: https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens veya Jenkins değerleri değiştirmek için bir komut dosyası görev çalıştırabilirsiniz.
>

## <a name="specifying-parameters-during-application-creation"></a>Uygulama oluşturma sırasında parametrelerini belirtme

Adlandırılmış uygulama örnekleri Service Fabric oluştururken, parametreleri geçirin seçeneğiniz vardır. Bunu yolu, Uygulama örneğinin nasıl oluşturacağınız bağlıdır.

  - PowerShell'de, [ `New-ServiceFabricApplication` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet karma tablosu olarak uygulama parametreleri alır.
  - Sfctl, kullanarak [ `sfctl application create` ](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-create) komut parametreleri JSON dize olarak alır. İnstall.sh komut dosyası sfctl kullanır.
  - Visual Studio uygulama projesi parametreleri klasöründe parametre dosyaları kümesini sağlar. Bu parametre dosyaları Visual Visual Studio Team hizmeti veya Team Foundation Server kullanarak Studio'dan yayımlama sırasında kullanılır. Visual Studio'da parametre dosyaları dağıtma FabricApplication.ps1 komut dosyasına geçirilen.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde açıklanan kavramları bazıları burada nasıl kullanıldığı gösterilmektedir:

- [Service Fabric Hizmetleri için ortam değişkenleri belirtme](service-fabric-how-to-specify-environment-variables.md)
- [Service Fabric parametrelerini kullanarak bir hizmet bağlantı noktası sayısını belirtme](service-fabric-how-to-specify-port-number-using-parameters.md)
- [Yapılandırma dosyaları Parametreleştirme nasıl](service-fabric-how-to-parameterize-configuration-files.md)

- [Ortam değişkeni başvurusu](service-fabric-environment-variables-reference.md)
