---
title: "Microsoft eşlemesi için eşleme, ortak bir Azure ExpressRoute taşıma | Microsoft Docs"
description: "Bu makalede expressroute bağlantı hattı üzerinde eşliği, ortak eşleme Microsoft'a taşımak için adımlar gösterilmektedir."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/07/2017
ms.author: cherylmc
ms.openlocfilehash: 311e1de3200cd684bbec1329ebd5217b4fb3a2e2
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Bir ortak Microsoft eşlemesi için eşleme taşıma

ExpressRoute, Azure depolama ve Azure SQL veritabanı, Microsoft ile yol filtreleri eşlemesini kullanarak gibi Azure PaaS Hizmetleri artık destekler. Şimdi Microsoft PaaS ve SaaS hizmetlerine erişmek için tek bir yönlendirme etki alanı gerekir. Yol filtreleri seçerek PaaS hizmeti, kullanmak istediğiniz Azure bölgeleri için öneklerini avantajından yararlanabilirsiniz.

Bu makalede, kapalı kalma süresi ile eşliği Microsoft ortak eşleme yapılandırmasını taşımanıza yardımcı olur. Yönlendirme etki alanları ve eşlemeleri hakkında daha fazla bilgi için bkz: [ExpressRoute bağlantı hatları ve Yönlendirme etki alanları](expressroute-circuit-peerings.md).

> [!IMPORTANT]
> Microsoft eşlemesi kullanmak üzere ExpressRoute premium eklentisi olması gerekir. Premium eklentisi hakkında daha fazla bilgi için bkz: [ExpressRoute SSS](expressroute-faqs.md#expressroute-premium).

## <a name="before-you-begin"></a>Başlamadan önce

* Microsoft eşlemesi için bağlanmak için ayarlamak ve NAT'ı yönetmek gerekir Bağlantı sağlayıcınız ayarlayabilir ve NAT yönetilen bir hizmet olarak yönetme. Microsoft eşlemesini Azure SaaS hizmetlerini ve Azure PaaS erişmeyi planlıyorsanız, NAT IP havuzu doğru boyut önemlidir. ExpressRoute NAT hakkında daha fazla bilgi için bkz: [Microsoft eşlemesi için NAT gereksinimleri](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Şu anda Azure PaaS hizmet uç noktalarına bir ağ erişim denetimi listesi (ACL) NAT IP havuzu ile yapılandırılmış emin olmak için gereken Azure ortak eşleme kullanırken varsa Microsoft eşlemesi hizmet için yapılandırılan erişim denetimi listesi bulunmaktadır uç noktası.

Microsoft kapalı kalma süresi ile eşlemesini taşımak için verildikleri sırada bu makaledeki adımları kullanmanız gerekir.

## <a name="1-create-microsoft-peering"></a>1. Microsoft eşlemesi oluşturma

Microsoft eşlemesi oluşturulmadı, Microsoft eşlemesi oluşturmak için aşağıdaki makalelere birini kullanın. Bağlantı sağlayıcı Teklifleriniz yönetilen Katman 3 Hizmetleri, Microsoft bağlantı hattınız için eşlemesini etkinleştirmek için bağlantı sağlayıcı isteyebilirsiniz.

  * [Azure portalını kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Azure Powershell kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-arm.md#msft)
  * [Azure CLI kullanarak Microsoft eşlemesi oluşturma](howto-routing-cli.md#msft)

## <a name="2-validate-microsoft-peering-is-enabled"></a>2. Microsoft doğrulama eşleme etkindir

Microsoft eşlemesi etkin ve yapılandırılmış durumda tanıtılan genel ön ekleri doğrulayın.

  * [Azure portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="3-configure-and-attach-a-route-filter-to-the-circuit"></a>3. Yapılandırma ve bağlantı hattı için bir rota filtre ekleme

Bir rota filtre devresine bağlanana kadar varsayılan olarak, tüm ön eklerin yeni Microsoft eşlemeleri tanıtmıyoruz. Bir rota filtre kuralı oluşturduğunuzda, aşağıdaki ekran görüntüsünde gösterildiği gibi Azure PaaS Hizmetleri için kullanmak istediğiniz Azure bölgeleri için hizmet topluluklarına listesi belirtebilirsiniz:

![Ortak eşlemeyi Birleştir](.\media\how-to-move-peering\public.png)

Yol filtrelerini yapılandırmak için aşağıdaki makalelere birini kullanın.

  * [Azure portalını kullanarak Microsoft eşliği için rota filtreleri yapılandırma](how-to-routefilter-portal.md)
  * [Azure PowerShell'i kullanarak Microsoft eşliği için rota filtreleri yapılandırma](how-to-routefilter-powershell.md)
  * [Azure CLI kullanarak Microsoft eşliği için rota filtreleri yapılandırma](how-to-routefilter-cli.md)

## <a name="4-delete-the-public-peering"></a>4. Ortak eşleme Sil

Microsoft eşlemesi yapılandırılmış ve Microsoft eşlemesi üzerinde kullanmak istediğiniz ön eklerin doğru bildirildiğini doğruladıktan sonra ortak eşleme sonra silebilirsiniz. Ortak eşlemesini silmek için aşağıdaki makalelere birini kullanın:

  * [Azure portalını kullanarak Azure ortak eşlemesini silmek](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Azure PowerShell kullanarak Azure ortak eşlemesini silmek](expressroute-howto-routing-arm.md#deletepublic)
  * [CLI kullanarak Azure ortak eşlemesini silmek](howto-routing-cli.md#deletepublic)

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).