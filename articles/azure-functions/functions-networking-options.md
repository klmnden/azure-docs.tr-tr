---
title: Azure işlevleri ağ seçenekleri
description: Azure işlevleri'nde kullanılabilir tüm ağ seçeneklerine genel bakış
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: a0bb34f8a43199a5d3a18064bce92ef4bec543af
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050651"
---
# <a name="azure-functions-networking-options"></a>Azure işlevleri ağ seçenekleri

Bu makalede, Azure işlevleri barındırma seçenekleri arasında kullanılabilir ağ özellikleri açıklanmaktadır. Aşağıdaki ağ seçeneklerini tüm kaynaklara internet yönlendirilebilir adresleri kullanmadan veya bir işlev uygulaması için internet erişimi kısıtlamak için bazı yetenekleri sağlar. 

Barındırma modelleri kullanılabilir ağ yalıtımı farklı düzeyleri vardır. Doğru olanı seçerek ağ yalıtımı gereksinimlerinizi karşılamanıza yardımcı olur.

İşlev uygulamaları birkaç farklı şekilde içinde barındırabilirsiniz:

* Bir sanal ağ bağlantısı ve ölçeklendirme seçenekleri çeşitli düzeylerde ile çok kiracılı bir altyapı üzerinde çalışan planı seçenek kümesi vardır:
    * [Tüketim planı](functions-scale.md#consumption-plan), yanıt olarak yüklemek için dinamik olarak ölçeklendirilebilir ve en az bir ağ yalıtım seçenekleri sunar.
    * [Premium planı](functions-scale.md#premium-plan), hangi ayrıca ölçeklendirilebilen dinamik olarak daha kapsamlı ağ yalıtımı sunmaya devam ederken.
    * Azure [App Service planı](functions-scale.md#app-service-plan), sabit bir ölçekte çalışır ve Premium plana benzer ağ yalıtımı sağlar.
* İşlevleri çalıştırabileceğiniz bir [App Service ortamı](../app-service/environment/intro.md). Bu yöntem, sanal ağınıza işlevinizi dağıtır ve tam ağ denetimi ve yalıtım sağlar.

## <a name="matrix-of-networking-features"></a>Matris ağ özellikleri

|                |[Tüketim planı](functions-scale.md#consumption-plan)|[Premium planı (Önizleme)](functions-scale.md#premium-plan)|[App Service planı](functions-scale.md#app-service-plan)|[App Service Ortamı](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Gelen IP kısıtlamaları](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[Giden IP kısıtlamaları](#private-site-access)|❌No| ❌No|❌No|✅Yes|
|[Sanal ağ tümleştirmesi](#virtual-network-integration)|❌No|❌No|✅Yes|✅Yes|
|[Sanal ağ tümleştirmesi Önizleme (Azure ExpressRoute ve hizmet uç noktalarına giden)](#preview-version-of-virtual-network-integration)|❌No|✅Yes|✅Yes|✅Yes|
|[Karma Bağlantılar](#hybrid-connections)|❌No|❌No|✅Yes|✅Yes|
|[Özel site erişimi](#private-site-access)|❌No| ✅Yes|✅Yes|✅Yes|

## <a name="inbound-ip-restrictions"></a>Gelen IP kısıtlamaları

IP kısıtlamaları, izin verilen/uygulamanıza erişim izni verilmeyen IP adresleri öncelik zamana göre sıralı bir listesini tanımlamak için kullanabilirsiniz. IPv4 ve IPv6 adresleri listesi içerebilir. Bir veya daha fazla olduğunda, örtük "Reddet tüm" listesinin sonunda yok. IP kısıtlamaları, tüm işlevi barındırma seçenekleri ile çalışır.

> [!NOTE]
> Azure portal Düzenleyicisi'ni kullanmak için portal, çalışan bir işlev uygulaması doğrudan erişebilir olması gerekir. Ayrıca, portala erişmek için kullanmakta olduğunuz cihaz kendi IP izin verilenler listesinde olmalıdır. Yerinde ağ kısıtlamaları ile herhangi bir özellik üzerinde erişmeye devam edebilirsiniz **Platform özellikleri** sekmesi.

Daha fazla bilgi için bkz. [Azure App Service statik erişim kısıtlamalarını](../app-service/app-service-ip-restrictions.md).

## <a name="outbound-ip-restrictions"></a>Giden IP kısıtlamaları

Giden IP kısıtlamaları, yalnızca bir App Service ortamı için dağıtılan işlevler için kullanılabilir. App Service ortamınızı dağıtıldığı sanal ağı için giden kısıtlamalar yapılandırabilirsiniz.

## <a name="virtual-network-integration"></a>Sanal ağ tümleştirmesi

İşlev uygulamanızı bir sanal ağ içindeki kaynaklara erişmek sanal ağ tümleştirmesi sağlar. Bu özellik hem Premium planı hem de App Service planı içinde kullanılabilir. Uygulamanızı bir App Service Ortamı'nda ise, bir sanal ağda zaten ve aynı sanal ağdaki kaynaklara erişmek için sanal ağ tümleştirmesi kullanımı gerektirmez.

Sanal ağ tümleştirmesi, sanal ağ içindeki kaynaklarla işlevi erişim verir ancak izni olmayan [özel site erişimi](#private-site-access) sanal ağdan işlev uygulamanız için.

Sanal ağ tümleştirmesi, veritabanları ve sanal ağınızda çalışan web hizmetleri uygulamalardan erişimi etkinleştirmek için kullanabilirsiniz. Sanal ağ ile tümleştirme, sanal makinenizin üzerinde uygulamalar için ortak bir uç noktanın kullanıma gerekmez. Bunun yerine, özel, olmayan-internet yönlendirilebilir adreslerini kullanabilirsiniz.

İşlev uygulamaları bir sanal ağa bağlanmak için bir VPN ağ geçidi sanal ağ tümleştirmesi genel kullanıma sunulan sürümünü kullanır. Bir App Service planında barındırılan işlevleri kullanılabilir. Bu özellik yapılandırma konusunda bilgi için bkz: [uygulamanızı bir Azure sanal ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md).

### <a name="preview-version-of-virtual-network-integration"></a>Sanal ağ tümleştirmesi Önizleme sürümü

Sanal ağ tümleştirme özelliği yeni bir sürümü Önizleme aşamasındadır. Bu noktadan siteye VPN'yi bağımlı değildir. ExpressRoute kaynaklara erişmeyi destekler veya hizmet bitiş noktası. Premium planı ve App Service planları için PremiumV2 ölçeği kullanılabilir.

Bu sürümün özelliklerinden bazıları şunlardır:

* Bunu kullanmak için bir ağ geçidi gerekmez.
* ExpressRoute bağlantılı sanal ağı tümleştirme dışında ek yapılandırma gerekmeksizin ExpressRoute bağlantılarından kaynaklara erişebilirsiniz.
* İşlevler'i çalıştırmaya hizmet uç noktası güvenli kaynakları kullanabilir. Bunu yapmak için sanal ağ tümleştirmesi için kullanılan alt ağ üzerindeki hizmet uç noktalarını etkinleştirin.
* Tetikleyiciler hizmetinin uç nokta güvenliği kaynakları kullanmak için yapılandıramazsınız. 
* İşlev uygulaması hem de sanal ağ aynı bölgede olması gerekir.
* Yeni özellik, Azure Resource Manager üzerinden dağıtılan sanal ağdaki kullanılmayan bir alt ağ gerektirir.
* Bu özellik Önizleme aşamasında olduğu sürece, üretim iş yükleri desteklenmez.
* Rota tabloları ve genel eşleme henüz özelliğiyle kullanılabilir değil.
* Bir adres olası her bir işlev uygulaması örneği için kullanılır. Atama sonra alt ağ boyutunu değiştiremezsiniz kolayca maksimum ölçek boyutunuzu destekleyen bir alt ağ kullanın. Örneğin, 80 örneklerine ölçeklendirilebilir bir Premium planı desteklemek için önerilir bir `/25` 126 konak adreslerini sağlayan bir alt ağ.

Sanal ağ tümleştirmesi önizleme sürümünü kullanma hakkında daha fazla bilgi edinmek için [bir işlev uygulaması, bir Azure sanal ağı ile tümleştirme](functions-create-vnet.md).

## <a name="hybrid-connections"></a>Karma Bağlantılar

[Karma bağlantılar](../service-bus-relay/relay-hybrid-connections-protocol.md) diğer ağlara uygulama kaynaklara erişmek için kullanabileceğiniz bir Azure geçişi, bir özelliğidir. Bir uygulama uç noktası uygulamanızdan erişim sağlar. Uygulamanıza erişmek için kullanamazsınız. Karma bağlantılar çalışan işlevler için kullanılabilir bir [App Service planı](functions-scale.md#app-service-plan) ve [App Service ortamı](../app-service/environment/intro.md).

Azure işlevleri'nde kullanılmak üzere tek bir TCP konak ve bağlantı noktası bileşimi her karma bağlantı ilişkilendirir. TCP dinleme bağlantı noktası eriştiğiniz sürece bu, herhangi bir işletim sistemi ve herhangi bir uygulama karma bağlantı uç noktası olabileceği anlamına gelir. Karma bağlantılar özelliği, bilmiyorsanız veya uygulama protokolü nedir ve ne eriştiğiniz dikkat edin. Yalnızca, ağ erişimi sağlar.

Daha fazla bilgi için bkz. [karma bağlantılar için App Service belgeleri](../app-service/app-service-hybrid-connections.md), bir App Service planında işlevleri destekler.

## <a name="private-site-access"></a>Özel site erişimi

Özel site erişimi, uygulamanızı yalnızca özel ağdan gibi bir Azure sanal ağı içinde erişilebilir hale getirmek için ifade eder. 
* Özel site erişimi, Premium ve App Service içinde kullanılabilir olduğunda planlama **hizmet uç noktaları** yapılandırılır. Daha fazla bilgi için [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
    * Hizmet uç noktaları ile işlevinizi hala İnternet'e giden tam erişim bile yapılandırılmış VNET Tümleştirmesi ile olduğunu aklınızda bulundurun.
* Özel site erişimi yalnızca bir iç yük dengeleyici (ILB) ile yapılandırılmış bir App Service ortamı ile kullanılabilir. Daha fazla bilgi için [oluşturma ve kullanma bir App Service ortamı ile iç yük dengeleyici](../app-service/environment/create-ilb-ase.md).

Diğer barındırma Seçenekleri'nde sanal ağ kaynaklarına erişmek için birçok yolu vardır. Ancak, bir App Service ortamı için bir sanal ağ üzerinden gerçekleşmesi bir işlev tetikler izin vermek için tek yoludur.

## <a name="next-steps"></a>Sonraki adımlar
Ağ ve Azure işlevleri hakkında daha fazla bilgi için: 

* [Sanal ağ Tümleştirmesi ile çalışmaya başlama hakkında bir öğretici uygulayın](./functions-create-vnet.md)
* [SSS ağ işlevleri okur](./functions-networking-faq.md)
* [Sanal ağ Tümleştirmesi ile uygulama hizmeti/işlevleri hakkında daha fazla bilgi edinin](../app-service/web-sites-integrate-with-vnet.md)
* [Azure'daki sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [Daha fazla ağ özellikleri ve App Service ortamları ile denetimi etkinleştir](../app-service/environment/intro.md)
* [Karma bağlantılar kullanarak güvenlik duvarı değişiklikleri olmadan tek şirket içi kaynaklara bağlanma](../app-service/app-service-hybrid-connections.md)
