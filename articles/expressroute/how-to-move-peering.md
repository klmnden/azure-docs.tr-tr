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
ms.openlocfilehash: 6b2bce6b488698db0a72c9a17f67c2555c6afa5b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60883155"
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Microsoft eşlemesi için genel eşleme Taşı

Bu makalede Microsoft kesinti yaşamadan eşleme için bir ortak eşleme yapılandırmasını taşımanıza yardımcı olur. ExpressRoute, Azure depolama ve Azure SQL Veritabanı gibi Azure PaaS hizmetleri için rota filtreleriyle Microsoft eşlemesinin kullanılmasını sağlar. Artık Microsoft PaaS ve SaaS hizmetlerine erişmek için tek bir yönlendirme etki alanına ihtiyacınız vardır. Kullanmak istediğiniz Azure bölgeleri için PaaS hizmeti ön eklerini tanıtmak için rota filtreleri kullanabilirsiniz. Yönlendirme etki alanları ve eşlemeleri hakkında daha fazla bilgi için bkz. [ExpressRoute devreleri ve Yönlendirme etki alanları](expressroute-circuit-peerings.md).

## <a name="before"></a>Başlamadan önce

* Microsoft eşlemesi için bağlanmak için ayarlama ve yönetme NAT gerekir Bağlantı sağlayıcınız ayarlayın ve NAT yönetilen bir hizmet olarak yönetme. Azure PaaS ve Microsoft eşlemesi üzerinde Azure SaaS hizmetlerine erişmek planlıyorsanız, NAT IP havuzu doğru şekilde boyutlandırmak önemlidir. ExpressRoute için NAT hakkında daha fazla bilgi için bkz. [Microsoft eşlemesi için NAT gereksinimleri](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Ortak eşleme kullanıyorsanız ve şu anda kullanılan genel IP adresleri için IP ağ kuralları sahip erişimi [Azure depolama](../storage/common/storage-network-security.md) veya [Azure SQL veritabanı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md), NAT IP havuzu yapılandırdığınızı emin olmanız gerekir Microsoft eşlemesi Azure depolama hesabına veya Azure SQL hesabı için genel IP adresleri listesi dahil edilir.

* Microsoft eşleme kapalı kalma süresi ile taşımak için verildikleri sırada bu makaledeki adımları kullanın.

## <a name="create"></a>1. Microsoft eşlemesi oluşturma

Microsoft eşlemesi oluşturulmadı, Microsoft eşlemesi oluşturmak için aşağıdaki makalelerden birini kullanın. Bağlantı sağlayıcısı tekliflerinizi yönetilen Katman 3 Hizmetleri, Microsoft, bağlantı hattı için eşleme etkinleştirmek için bağlantı sağlayıcısı isteyebilirsiniz.

  * [Azure portalını kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Azure Powershell kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-arm.md#msft)
  * [Azure CLI kullanarak Microsoft eşlemesi oluşturma](howto-routing-cli.md#msft)

## <a name="validate"></a>2. Doğrulama Microsoft eşdüzey hizmet sağlamanın etkin

Microsoft eşdüzey hizmet sağlamanın etkinleştirildiğinden ve tanıtılan genel önekler yapılandırılmış durumda olduğunu doğrulayın.

  * [Azure portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="routefilter"></a>3. Yapılandırma ve bağlantı hattı için bir rota filtresi ekleme

Bağlantı hattı için bir rota filtresinde bağlanana kadar varsayılan olarak, tüm ön ekleri yeni Microsoft eşlemeleri tanıtmıyoruz. Bir rota filtresi kuralı oluşturduğunuzda, aşağıdaki ekran görüntüsünde gösterildiği gibi Azure PaaS Hizmetleri için kullanmak istediğiniz Azure bölgeleri için hizmet toplulukları listesini belirtebilirsiniz:

![Ortak eşleme Birleştir](./media/how-to-move-peering/public.png)

Rota filtreleri aşağıdaki makalelerden birini kullanarak yapılandırın:

  * [Azure portalını kullanarak Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-portal.md)
  * [Azure PowerShell kullanarak Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-powershell.md)
  * [Azure CLI kullanarak Microsoft eşlemesi için rota filtreleri yapılandırma](how-to-routefilter-cli.md)

## <a name="delete"></a>4. Ortak eşleme Sil

Microsoft eşlemesi yapılandırılmış ve Microsoft eşlemesi üzerinde kullanmak istediğiniz ön ekleri doğru tanıtıldığından doğruladıktan sonra genel eşleme sonra silebilirsiniz. Ortak eşlemesini silmek için aşağıdaki makalelerden birini kullanın:

  * [Azure portalını kullanarak Azure ortak eşlemesini Sil](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Azure PowerShell kullanarak Azure ortak eşlemesini Sil](expressroute-howto-routing-arm.md#deletepublic)
  * [CLI kullanarak Azure ortak eşlemesini Sil](howto-routing-cli.md#deletepublic)
  
## <a name="view"></a>5. Görünüm eşlemeleri
  
Tüm ExpressRoute devreleri ve Azure portalında eşlemeleri listesini görebilirsiniz. Daha fazla bilgi için [görünümü Microsoft eşleme ayrıntılarını](expressroute-howto-routing-portal-resource-manager.md#getmsft).

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
