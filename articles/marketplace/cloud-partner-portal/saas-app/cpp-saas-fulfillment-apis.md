---
title: SaaS yerine getirme API - Azure Marketi | Microsoft Docs
description: İle Azure marketi, SaaS tümleştirmenize olanak sağlayan API'ler sunar yerine getirme sürümlerini sunar.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: pbutlerm
ms.openlocfilehash: c7da46984d592abc6ed97d7490fde732bf26b0ba
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62101362"
---
# <a name="saas-fulfillment-apis"></a>SaaS gerçekleştirme API'leri

SaaS yerine getirme API'leri, bağımsız yazılım satıcılarına (ISV) Azure Market ile SaaS uygulamalarını tümleştirme sağlar. Bu API'leri ISV uygulamaları tüm ticari etkin kanallarında katılmayı etkinleştir: doğrudan, iş ortağı destekli (satıcı) ve alan öncülüğünde.  Bir gereksinim transactable SaaS tekliflerini Azure Marketi'nde listeleme değildirler.

> [!WARNING]
> Bu API'ın geçerli sürümü, sürüm 2 olan tüm yeni SaaS için kullanılmalıdır sunar.  API sürümü 1 kullanım dışıdır ve mevcut teklifler desteklemek üzere saklanır.


## <a name="business-model-support"></a>İş modeli desteği

Bu API aşağıdaki iş modeli özelliklerini destekler; Şunları yapabilirsiniz:

* Bir teklif için birden çok planları belirtin. Bu planları farklı işlevselliğe sahip ve farklı fiyatlandırılır.
* Teklif sağlayan bir site başına veya bir model başına faturalama kullanıcı başına.
* Aylık ve yıllık (önceden ödenen) sağlayan faturalandırma seçenekleriyle.
* Üzerinde anlaşılan iş anlaşmasına göre bir müşteriye özel ücretlendirme yapılır.


## <a name="next-steps"></a>Sonraki adımlar

Zaten yapmadıysanız, SaaS uygulamanızı kaydetme [Azure portalında](https://ms.portal.azure.com) açıklandığı şekilde [bir Azure AD uygulaması kaydedin](./cpp-saas-registration.md).  Ardından, geliştirme için en güncel sürümü bu arabirimin kullanın: [SaaS yerine getirme API sürüm 2](./cpp-saas-fulfillment-api-v2.md).
