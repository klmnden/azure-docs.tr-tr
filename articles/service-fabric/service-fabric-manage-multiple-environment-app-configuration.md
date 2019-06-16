---
title: Azure Service fabric'te birden çok ortam için uygulamaları yönetme | Microsoft Docs
description: Azure Service Fabric uygulamaları kümelerinde, aralığı boyutu bir makineden binlerce makineye kadar çalıştırabilirsiniz. Bazı durumlarda, uygulamanız çeşitli ortamlar için farklı şekilde yapılandırmak isteyebilirsiniz. Bu makalede, her ortam farklı uygulama parametrelerini tanımlayın ele alınmaktadır.
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
ms.openlocfilehash: dac96ef6fce38a0557444e181fa6eccb649cfb9a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60719235"
---
# <a name="manage-applications-for-multiple-environments"></a>Birden çok ortam için uygulamaları yönetme

Azure Service Fabric kümeleri, birden çok binlerce herhangi bir yerde kullanarak kümeleri oluşturmak etkinleştirme makineler. Çoğu durumda kendiniz uygulamanız birden çok küme yapılandırmaları dağıtmak zorunda bulduğunuz: yerel geliştirme kümenizin, paylaşılan bir geliştirme kümesi ve üretim kümenizi. Tüm bu kümeleri farklı ortamlarda kodunuzun çalıştırmak için sahip olarak kabul edilir. Uygulama ikili dosyalarını bu arasında paylaşılabilen çok hiçbir değişikliğe gerek olmadan çalıştırabilirsiniz ancak genellikle uygulamanın farklı bir şekilde yapılandırmak isteyebilirsiniz.

İki basit örneğe göz önünde bulundurun:
  - Hizmetinizi tanımlı bir bağlantı noktasında dinler, ancak ortamlar genelinde farklı olması için bu bağlantı noktası gerekiyor
  - farklı bağlama kimlik bilgileri için bir veritabanı ortamlarında sağlamanız gerekir

## <a name="specifying-configuration"></a>Yapılandırma belirtme

Sağladığınız yapılandırma içinde iki kategoriye ayrılabilir:

- Hizmetlerinizi nasıl çalıştığını için geçerli yapılandırma
  - Örneğin, bir uç nokta veya bir hizmetin örneklerine sayısı için bağlantı noktası numarası
  - Bu yapılandırma, uygulama veya hizmet bildirimi dosyası belirtilmedi
- Uygulama kodunuz için geçerli yapılandırma
  - Örneğin, bir veritabanı için bağlama bilgileri
  - Bu yapılandırma yapılandırma dosyaları ya da ortam değişkenlerini aracılığıyla sağlanabilir

> [!NOTE]
> Tüm öznitelikleri uygulama ve hizmet dosya desteği parametrelerini bildirimi.
> Bu gibi durumlarda, dizeleri dağıtım akışınızın bir parçası değiştirerek üzerinde zorunda değilsiniz. Azure DevOps bir uzantısı gibi değiştirin belirteçleri kullanabilirsiniz: https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens veya Jenkins değerleri değiştirmek için bir betik görev çalıştırabilirsiniz.
>

## <a name="specifying-parameters-during-application-creation"></a>Uygulama oluşturma sırasında parametrelerini belirtme

Adlandırılmış uygulama örnekleri Service Fabric'te oluştururken parametreleri geçirmek için seçeneğiniz vardır. Uygulama örneğini nasıl oluşturacağınızı, yaptığınız gibi bağlıdır.

  - PowerShell'de, [ `New-ServiceFabricApplication` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet, karma tablosu olarak uygulama parametreleri alır.
  - Sfctl, kullanarak [ `sfctl application create` ](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-create) komutu, bir JSON dizesi parametreleri alır. Sfctl install.sh betiği kullanır.
  - Visual Studio uygulama projesini parametreleri klasöründe parametre dosyaları kümesi sağlar. Bu parametre dosyaları, Azure DevOps Services veya Team Foundation Server kullanarak Visual Studio'dan, yayımlama sırasında kullanılır. Visual Studio'da parametre dosyaları dağıtma FabricApplication.ps1 betiğe geçirilen.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri açıklanan kavramları bazıları burada kullanma işlemini gösterir:

- [Service Fabric'te Hizmetleri için ortam değişkenlerini belirtme](service-fabric-how-to-specify-environment-variables.md)
- [Service Fabric'te parametrelerini kullanarak bir hizmet bağlantı noktası numarasını belirtme](service-fabric-how-to-specify-port-number-using-parameters.md)
- [Yapılandırma dosyalarını Parametreleştirme nasıl](service-fabric-how-to-parameterize-configuration-files.md)

- [Ortam değişkeni başvurusu](service-fabric-environment-variables-reference.md)
