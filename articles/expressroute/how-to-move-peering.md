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
ms.date: 03/12/2018
ms.author: cherylmc
ms.openlocfilehash: 02d7c3f587a4cbfb11fc3b6863f75ca30b4d6c51
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Bir ortak Microsoft eşlemesi için eşleme taşıma

Microsoft Azure depolama ve Azure SQL veritabanı gibi Azure PaaS hizmetler için rota filtrelerle eşlemesini ExpressRoute kullanılmasını destekler. Şimdi Microsoft PaaS ve SaaS hizmetlerine erişmek için tek bir yönlendirme etki alanı gerekir. Seçmeli olarak PaaS hizmeti, kullanmak istediğiniz Azure bölgeleri için öneklerini yol filtreleri kullanabilirsiniz.

Bu makalede, kapalı kalma süresi ile eşliği Microsoft ortak eşleme yapılandırmasını taşımanıza yardımcı olur. Yönlendirme etki alanları ve eşlemeleri hakkında daha fazla bilgi için bkz: [ExpressRoute bağlantı hatları ve Yönlendirme etki alanları](expressroute-circuit-peerings.md).

> [!IMPORTANT]
> Microsoft eşlemesi kullanmak üzere ExpressRoute premium eklentisi olması gerekir. Premium eklentisi hakkında daha fazla bilgi için bkz: [ExpressRoute SSS](expressroute-faqs.md#expressroute-premium).

## <a name="before"></a>Başlamadan önce

* Microsoft eşlemesi için bağlanmak için ayarlamak ve NAT'ı yönetmek gerekir Bağlantı sağlayıcınız ayarlayabilir ve NAT yönetilen bir hizmet olarak yönetme. Microsoft eşlemesini Azure SaaS hizmetlerini ve Azure PaaS erişmeyi planlıyorsanız, NAT IP havuzu doğru boyut önemlidir. ExpressRoute NAT hakkında daha fazla bilgi için bkz: [Microsoft eşlemesi için NAT gereksinimleri](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Ortak eşleme kullanıyorsanız ve şu anda kullanılan genel IP adresleri için IP ağ kuralları sahip erişimi [Azure Storage](../storage/common/storage-network-security.md) veya [Azure SQL veritabanı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md), NAT IP havuzu yapılandırılmış emin olmanız gerekir ile Microsoft eşliği Azure depolama hesabı veya Azure SQL hesabı için genel IP adresleri listesi yer alır.

* Microsoft kapalı kalma süresi ile eşlemesini taşımak için verildikleri sırada bu makaledeki adımları kullanın.

## <a name="create"></a>1. Microsoft eşlemesi oluşturma

Microsoft eşlemesi oluşturulmadı, Microsoft eşlemesi oluşturmak için aşağıdaki makalelere birini kullanın. Bağlantı sağlayıcı Teklifleriniz yönetilen Katman 3 Hizmetleri, Microsoft bağlantı hattınız için eşlemesini etkinleştirmek için bağlantı sağlayıcı isteyebilirsiniz.

  * [Azure portalını kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Azure Powershell kullanarak Microsoft eşlemesi oluşturma](expressroute-howto-routing-arm.md#msft)
  * [Azure CLI kullanarak Microsoft eşlemesi oluşturma](howto-routing-cli.md#msft)

## <a name="validate"></a>2. Microsoft doğrulama eşleme etkindir

Microsoft eşlemesi etkin ve yapılandırılmış durumda tanıtılan genel ön ekleri doğrulayın.

  * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="routefilter"></a>3. Yapılandırma ve bağlantı hattı için bir rota filtre ekleme

Bir rota filtre devresine bağlanana kadar varsayılan olarak, tüm ön eklerin yeni Microsoft eşlemeleri tanıtmıyoruz. Bir rota filtre kuralı oluşturduğunuzda, aşağıdaki ekran görüntüsünde gösterildiği gibi Azure PaaS Hizmetleri için kullanmak istediğiniz Azure bölgeleri için hizmet topluluklarına listesi belirtebilirsiniz:

![Ortak eşlemeyi Birleştir](.\media\how-to-move-peering\public.png)

Yol filtreleri aşağıdaki makalelerde birini kullanarak yapılandırın:

  * [Azure portalını kullanarak Microsoft eşliği için rota filtreleri yapılandırma](how-to-routefilter-portal.md)
  * [Azure PowerShell'i kullanarak Microsoft eşliği için rota filtreleri yapılandırma](how-to-routefilter-powershell.md)
  * [Azure CLI kullanarak Microsoft eşliği için rota filtreleri yapılandırma](how-to-routefilter-cli.md)

## <a name="delete"></a>4. Ortak eşleme Sil

Microsoft eşlemesi yapılandırılmış ve Microsoft eşlemesi üzerinde kullanmak istediğiniz ön eklerin doğru bildirildiğini doğruladıktan sonra ortak eşleme sonra silebilirsiniz. Ortak eşlemesini silmek için aşağıdaki makalelere birini kullanın:

  * [Azure portalını kullanarak Azure ortak eşlemesini silmek](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Azure PowerShell kullanarak Azure ortak eşlemesini silmek](expressroute-howto-routing-arm.md#deletepublic)
  * [CLI kullanarak Azure ortak eşlemesini silmek](howto-routing-cli.md#deletepublic)
  
## <a name="view"></a>5. Görünüm eşlemeleri
  
Tüm ExpressRoute bağlantı hatları ve Azure portalında eşlemeleri listesini görebilirsiniz. Daha fazla bilgi için bkz: [görünüm Microsoft eşleme ayrıntılarını](expressroute-howto-routing-portal-resource-manager.md#getmsft).

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
