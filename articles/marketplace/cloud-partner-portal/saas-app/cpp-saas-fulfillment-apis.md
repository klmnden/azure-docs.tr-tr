---
title: SaaS yerine getirme API'leri | Azure Market
description: İle Azure marketi, SaaS tümleştirmenize olanak sağlayan API'ler sunar yerine getirme sürümlerini sunar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: pabutler
ms.openlocfilehash: ec206c2d637f9fb2727d72cf17087050a765672c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64941959"
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
