---
title: Azure işlevleri ağ seçenekleri
description: Azure işlevleri'nde kullanılabilir tüm ağ seçeneklerine genel bakış
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: conceptual
ms.date: 1/14/2019
ms.author: alkarche
ms.openlocfilehash: 10d7daa6da45c56e20c622fcbca9ee288e737dab
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358171"
---
# <a name="azure-functions-networking-options"></a>Azure işlevleri ağ seçenekleri

Bu belgede, Azure işlevleri barındırma seçenekleri arasında kullanılabilir ağ özellikleri paketini açıklanmaktadır. Aşağıdaki ağ seçeneklerini tüm kaynaklara internet yönlendirilebilir adresleri kullanmadan veya bir işlev uygulaması için internet erişimi kısıtlamak için bazı yetenekleri sağlar. Kullanılabilir ağ yalıtımı farklı düzeylerde tüm barındırma modelleri sahiptir ve doğru seçme, ağ yalıtımı gereksinimlerinizi karşılayacak şekilde izin verir.

İşlev uygulamaları birkaç farklı yolla barındırılabilir.

* Bir sanal AĞA bağlantı ve ölçeklendirme seçenekleri çeşitli düzeylerde ile çok kiracılı altyapısında çalışan planı seçenek kümesi vardır.
    1. Yanıt olarak yüklemek için dinamik olarak ölçeklendirilebilir ve en az bir ağ yalıtım seçenekleri sunar tüketim planı.
    1. Premium olarak da dinamik olarak daha kapsamlı ağ yalıtımı sunmaya devam ederken ölçeklenen planlayın.
    1. App Service, sabit bir ölçekte çalışır ve benzer ağ yalıtımı için Premium planı sunar planı.
* İşlevleri, bir App Service ortamı (işlevinizi Vnet'inizi dağıtır ve tam ağ denetimi ve yalıtım sağlar. ASE üzerinde) da çalıştırılabilir.

## <a name="networking-feature-matrix"></a>Özellik matrisi ağ

|                |[Tüketim Planı](functions-scale.md#consumption-plan)|⚠ [Premium planı](functions-scale.md##premium-plan-public-preview)|[Uygulama Hizmeti Planı](functions-scale.md#app-service-plan)|[App Service Ortamı](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[**Gelen IP kısıtlamaları**](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[**Sanal Ağ Tümleştirmesi**](#vnet-integration)|❌No|⚠ Evet|✅Yes|✅Yes|
|[**Sanal ağ tümleştirmesi Önizleme (Express Route ve hizmet uç noktaları)**](#preview-vnet-integration)|❌No|⚠ Evet|⚠ Evet|✅Yes|
|[**Karma Bağlantılar**](#hybrid-connections)|❌No|❌No|✅Yes|✅Yes|
|[**Özel Site erişimi**](#private-site-access)|❌No| ❌No|❌No|✅Yes|

⚠ Üretim kullanımı için önizleme özelliği

## <a name="inbound-ip-restrictions"></a>Gelen IP kısıtlamaları

IP kısıtlamaları sıralı uygulamanıza erişmek için izin verilen IP adreslerini izin verme/reddetme listesi öncelikli tanımlamanızı sağlar. IPv4 ve IPv6 adresleri izin verilenler listesine ekleyebilirsiniz. Bir veya daha fazla olduğunda, örtük olarak reddetmek sonra listenin en sonunda bulunan tüm yoktur. IP kısıtlamaları özelliği, barındırma seçenekleri tüm işlev ile çalışır.

> [!NOTE]
> Azure portal Düzenleyicisi'ni kullanabilmek için portal, çalışan bir işlev uygulaması doğrudan erişebilir olmalıdır ve portala erişmek için kullandığınız cihazla kendi IP izin verilenler listesinde olmalıdır. Yerinde ağ kısıtlamaları ile herhangi bir özellik erişmeye devam edebilirsiniz **Platform özellikleri** sekmesi.

[Buradan daha fazla bilgi edinin](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)

## <a name="vnet-integration"></a>Sanal Ağ Tümleştirmesi

VNET tümleştirmesi, bir sanal ağ içindeki kaynaklara erişmek işlev uygulamanızı sağlar. VNET tümleştirmesi Premium planı ve App Service planı içinde kullanılabilir. Uygulamanızı bir App Service Ortamı'nda ise, bir sanal ağda zaten ve aynı sanal ağ kaynaklarına ulaşmak için VNet tümleştirmesi özelliğinin gerektirmez.

VNet tümleştirmesi, sanal ağ içindeki kaynaklarla işlevi erişim verir ancak izni olmayan [özel site erişimi](#private-site-access) sanal ağdan işlev uygulamanız için.

VNet tümleştirmesi, genellikle bir veritabanlarına uygulamalardan erişime izin verin ve web Hizmetleri, sanal ağda çalışan için kullanılır. VNet Tümleştirmesi ile VM ancak can uygulamaları özel dışı Internet yönlendirilebilir adreslerini kullanmanız için genel bir uç nokta kullanıma sunmak gerekmez.

İşlev uygulamaları bir sanal ağa bağlanmak için bir VPN ağ geçidi VNET tümleştirmesi genel kullanıma sunulan sürümünü kullanır. Bir app service planında barındırılan işlevleri kullanılabilir. Bu özellik yapılandırma konusunda bilgi için bkz: [aynı özelliği için App Service belge](../app-service/web-sites-integrate-with-vnet.md#enabling-vnet-integration).

### <a name="preview-vnet-integration"></a>Önizleme VNET tümleştirmesi

Yeni bir sürümü önizlemede olan VNet tümleştirme özelliği yoktur. Noktadan siteye VPN'yi bağımlı değildir ve kaynaklara erişmeyi ExpressRoute veya hizmet uç noktaları arasında da destekler. Bu özellik, Premium planı ve App Service planları için PremiumV2 ölçeği kullanılabilir.

Şu anda önizlemede olan VNet tümleştirmesi yeni sürümünü aşağıdaki avantajları sağlar:

* Ağ geçidi yok yeni VNet tümleştirme özelliğini kullanmak için gereklidir
* ExpressRoute ile tümleştirme ötesinde herhangi bir ek yapılandırma, sanal ağa bağlı olmayan ExpressRoute bağlantılar üzerinden kaynaklara erişebilir.
* Hizmet uç kaynakları işlevleri çalıştırılmasının güvenli kullanabilir. Bunu yapmak için sanal ağ tümleştirmesi için kullanılan alt ağ üzerindeki hizmet uç noktalarını etkinleştirin.
* Yeni VNet tümleştirme özelliğini kullanarak kaynakları güvenli hizmet uç kullanılacak Tetikleyicileri yapılandıramazsınız. 
* İşlev uygulaması hem de sanal ağ aynı bölgede olması gerekir.
* Yeni özellik, Resource Manager sanal ağınızdaki kullanılmayan bir alt ağ gerektiriyor.
* Önizleme aşamasında olduğu sürece, üretim iş yükleri yeni VNet tümleştirmesi sürümünde desteklenmez.
* Rota tabloları ve genel eşleme henüz yeni VNet Tümleştirmesi ile kullanılamaz.
* Bir adres, her olası bir işlev uygulaması örneği için kullanılır. Alt ağ boyutu atamasından sonra değiştirilemediğinden kolayca maksimum ölçek boyutunuzu destekleyen bir alt ağ kullanın. Örneğin, 80 örneklerine ölçeklendirilebilir bir Premium planı desteklemek için önerilir bir `/25` 126 konak adreslerini sağlayan bir alt ağ.

Sanal ağ tümleştirmesi Önizleme kullanma hakkında daha fazla bilgi edinmek için [bir işlev uygulaması bir Azure sanal ağ ile tümleştirme](functions-create-vnet.md).

## <a name="hybrid-connections"></a>Karma Bağlantılar

[Karma bağlantılar](../service-bus-relay/relay-hybrid-connections-protocol.md) Azure geçişi, uygulama kaynaklarında başka ağlara erişim için kullanılan bir özelliğidir. Bir uygulama uç noktası uygulamanızdan erişim sağlar. Uygulamanıza erişmek için kullanılamaz. Karma bağlantılar çalışan işlevler için kullanılabilir bir [App Service planı](functions-scale.md#app-service-plan) ve [App Service ortamı](../app-service/environment/intro.md).

İşlevlerde kullanılan gibi tek bir TCP konak ve bağlantı noktası bileşimi her karma bağlantı ilişkilendirir. Bu karma bağlantı uç noktasını tüm işletim sistemlerinde olabilir ve size sağlanan herhangi bir uygulama, TCP dinleme bağlantı noktası eriştiğiniz anlamına gelir. Karma bağlantılar özelliği, bilmiyorsanız veya uygulama protokolü nedir ve ne eriştiğiniz dikkat edin. Ayrıca, ağ erişimini yalnızca sağlamaktır.

Daha fazla bilgi için bkz. [karma bağlantılar için App Service belgeleri](../app-service/app-service-hybrid-connections.md), hem işlev hem de Web uygulamalarını destekler.

## <a name="private-site-access"></a>Özel Site erişimi

Özel site erişimi, uygulamanızı yalnızca özel ağdan gibi bir Azure sanal ağı içinde erişilebilir hale getirmek için ifade eder. Özel site erişimi yalnızca bir iç yük dengeleyici (ILB) ile yapılandırılmış bir ASE ile kullanılabilir. ILB ASE kullanma hakkında daha fazla bilgi için bkz [oluşturma ve ILB ASE kullanır](../app-service/environment/create-ilb-ase.md).

Diğer barındırma seçenekleri VNET kaynaklara erişmek için birçok yolu vardır, ancak bir ASE Tetikleyiciler bir VNET üzerinde gerçekleşmesi bir işlev için izin vermek için tek yoludur.
