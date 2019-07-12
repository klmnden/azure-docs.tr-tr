---
title: Ortak eşleme Microsoft'a taşıma eşleme - Azure ExpressRoute | Microsoft Docs
description: Bu makalede ExpressRoute eşlemesi, genel eşleme Microsoft'a taşıma adımları gösterilmektedir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 03/12/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 5581e2a5c2fe2ee5e154870f7ffc2ab1c3c0f3b4
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592111"
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Microsoft eşlemesi için genel eşleme Taşı

Bu makalede Microsoft kesinti yaşamadan eşleme için bir ortak eşleme yapılandırmasını taşımanıza yardımcı olur. ExpressRoute, Azure depolama ve Azure SQL Veritabanı gibi Azure PaaS hizmetleri için rota filtreleriyle Microsoft eşlemesinin kullanılmasını sağlar. Artık Microsoft PaaS ve SaaS hizmetlerine erişmek için tek bir yönlendirme etki alanına ihtiyacınız vardır. Kullanmak istediğiniz Azure bölgeleri için PaaS hizmeti ön eklerini tanıtmak için rota filtreleri kullanabilirsiniz.

Azure genel eşdüzey hizmet sağlama için her bir BGP oturumu ilişkili 1 NAT IP adresi vardır. Microsoft eşlemesi yanı sıra kendi NAT ayırmaları yapılandırma seçmeli önek tanıtımları için rota filtreleri kullanın olanak sağlar. Genel eşdüzey hizmet sağlama, Microsoft Azure Hizmetleri için hangi bağlantı kullanarak tek yönlü hizmet her zaman WAN'ınızdan başlatılır ' dir. Microsoft Azure Hizmetleri, ağınızda bu yönlendirme etki alanı aracılığıyla yönelik bağlantıları başlatmasını mümkün olmayacaktır.

Genel eşdüzey hizmet sağlama etkinleştirildikten sonra tüm Azure hizmetlerine bağlanabilirsiniz. Biz size yolları tanıtmak Hizmetleri seçmeli olarak çekmek izin vermiyor. Microsoft eşlemesi yönlü bağlantıyı WAN yanı sıra Microsoft Azure hizmetinden bağlantı burada başlatılabilir olsa da. Yönlendirme etki alanları ve eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute devreleri ve Yönlendirme etki alanları](expressroute-circuit-peerings.md).

## <a name="before"></a>Başlamadan önce

Microsoft eşlemesi için bağlanmak için ayarlama ve yönetme NAT gerekir Bağlantı sağlayıcınız ayarlayın ve NAT yönetilen bir hizmet olarak yönetme. Azure PaaS ve Microsoft eşlemesi üzerinde Azure SaaS hizmetlerine erişmek planlıyorsanız, NAT IP havuzu doğru şekilde boyutlandırmak önemlidir. ExpressRoute için NAT hakkında daha fazla bilgi için bkz. [Microsoft eşlemesi için NAT gereksinimleri](expressroute-nat.md#nat-requirements-for-microsoft-peering). Azure ExpressRoute(Microsoft peering) üzerinden Microsoft'a bağlandığınızda, Microsoft'a birden çok bağlantıları vardır. Bağlantıların biri, var olan İnternet bağlantınız diğeri de ExpressRoute aracılığıyla olan bağlantı. Microsoft’a yönelik trafiğin bir kısmı İnternet üzerinden gidip ExpressRoute üzerinden dönebilir ya da tam tersi gerçekleşebilir.

![Çift yönlü bağlantı](./media/how-to-move-peering/bidirectional-connectivity.jpg)

> [!Warning]
> Microsoft'a tanıtılan NAT IP havuzu İnternet'e tanıtılmamalıdır. Bu, diğer Microsoft hizmetlerine bağlantıyı keser.

Başvurmak [birden çok ağ yolu ile asimetrik yönlendirme](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing) Microsoft eşlemesi yapılandırmadan önce asimetrik yönlendirme uyarılar için.

* Ortak eşleme kullanıyorsanız ve şu anda kullanılan genel IP adresleri için IP ağ kuralları sahip erişimi [Azure depolama](../storage/common/storage-network-security.md) veya [Azure SQL veritabanı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md), NAT IP havuzu yapılandırdığınızı emin olmanız gerekir Microsoft eşlemesi Azure depolama hesabına veya Azure SQL hesabı için genel IP adresleri listesi dahil edilir.<br>
* Microsoft eşleme kapalı kalma süresi ile taşımak için verildikleri sırada bu makaledeki adımları kullanın.

## <a name="create"></a>1. Microsoft eşlemesi oluşturma

Microsoft eşlemesi oluşturulmadı, Microsoft eşlemesi oluşturmak için aşağıdaki makalelerden birini kullanın. Bağlantı sağlayıcısı tekliflerinizi yönetilen Katman 3 Hizmetleri, Microsoft, bağlantı hattı için eşleme etkinleştirmek için bağlantı sağlayıcısı isteyebilirsiniz.

Sizin tarafınızdan yönetilen Katman 3, devam etmeden önce aşağıdaki bilgiler gereklidir:

* Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft, yönlendirici için ikinci kalmayacak IP kullandığından bu alt ağından yönlendiriciniz için ilk kullanılabilir IP adresi atar.<br>
* İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft, yönlendirici için ikinci kalmayacak IP kullandığından bu alt ağından yönlendiriciniz için ilk kullanılabilir IP adresi atar.<br>
* Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Birincil ve ikincil bağlantı için aynı VLAN kimliğini kullanmanız gerekir<br>
* Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.<br>
* Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm ön eklerin listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Ön ek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu ön ekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.<br>
* Yönlendirme kayıt defteri adı: Belirtebileceğiniz RIR / IRR'yi AS numarası ve öneklerinin kaydedildiği rır.

* **İsteğe bağlı** -müşteri ASN'si: Eşleme AS numarasına kayıtlı olmayan önekler tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz.<br>
* **İsteğe bağlı** -kullanmayı seçerseniz bir MD5 karma değeri.

Microsoft eşlemesi etkinleştirmek için ayrıntılı yönergeler aşağıdaki makalelerde bulunabilir:

* [Azure portalını kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-portal-resource-manager.md#msft)<br>
* [Azure Powershell kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-arm.md#msft)<br>
* [Azure CLI kullanarak Microsoft eşlemesi oluşturma](howto-routing-cli.md#msft)

## <a name="validate"></a>2. Doğrulama Microsoft eşdüzey hizmet sağlamanın etkin

Microsoft eşdüzey hizmet sağlamanın etkinleştirildiğinden ve tanıtılan genel önekler yapılandırılmış durumda olduğunu doğrulayın.

* [Azure portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)<br>
* [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)<br>
* [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="routefilter"></a>3. Yapılandırma ve bağlantı hattı için bir rota filtresi ekleme

Varsayılan olarak, yeni Microsoft eşlemesi kullanıma sunmuyor tüm ön ekleri bağlantı hattına bir rota filtresinde bağlanana kadar. Bir rota filtresi kuralı oluşturduğunuzda, hizmet toplulukları Azure PaaS Hizmetleri için kullanmak istediğiniz Azure bölgelerinde listesini belirtebilirsiniz. Bu, yollar, ihtiyacınıza göre filtrelemek için esneklik aşağıdaki ekran görüntüsünde gösterildiği gibi sağlar:

![Ortak eşleme Birleştir](./media/how-to-move-peering/routefilter.jpg)

Rota filtreleri aşağıdaki makalelerden birini kullanarak yapılandırın:

* [Azure portalını kullanarak Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-portal.md)<br>
* [Azure PowerShell kullanarak Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-powershell.md)<br>
* [Azure CLI kullanarak Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-cli.md)

## <a name="delete"></a>4. Ortak eşleme Sil

Microsoft eşlemesi yapılandırılmış ve Microsoft eşlemesi üzerinde kullanmak istediğiniz ön ekleri doğru tanıtıldığından doğruladıktan sonra genel eşleme sonra silebilirsiniz. Ortak eşlemesini silmek için aşağıdaki makalelerden birini kullanın:

* [Azure portalını kullanarak Azure ortak eşlemesini Sil](expressroute-howto-routing-portal-resource-manager.md#deletepublic)<br>
* [Azure PowerShell kullanarak Azure ortak eşlemesini Sil](expressroute-howto-routing-arm.md#deletepublic)<br>
* [CLI kullanarak Azure ortak eşlemesini Sil](howto-routing-cli.md#deletepublic)
  
## <a name="view"></a>5. Görünüm eşlemeleri
  
Tüm ExpressRoute devreleri ve Azure portalında eşlemeleri listesini görebilirsiniz. Daha fazla bilgi için [görünümü Microsoft eşleme ayrıntılarını](expressroute-howto-routing-portal-resource-manager.md#getmsft).

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
