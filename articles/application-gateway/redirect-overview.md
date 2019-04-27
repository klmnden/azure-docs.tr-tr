---
title: Azure Application Gateway için yeniden yönlendirmeye genel bakış
description: Azure Application Gateway'de yeniden yönlendirme özelliği hakkında bilgi edinin
services: application-gateway
documentationcenter: na
author: amsriva
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/19/2018
ms.date: 04/17/2019
ms.author: v-junlch
ms.openlocfilehash: 8e88e0e11b3ccab7cc2c68b2617df2d588680780
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60715811"
---
# <a name="application-gateway-redirect-overview"></a>Application Gateway yeniden yönlendirmeye genel bakış

Uygulama ağ geçidi trafiği yönlendirmek için kullanabilirsiniz.  Veren başka bir dinleyici veya dış bir siteye bir dinleyici geliş trafiği yönlendirmek için genel yönlendirme mekanizması var. Bu uygulama yapılandırmasını basitleştiren, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme de dahil olmak üzere yeni yeniden yönlendirme senaryoları destekler.

HTTPS yeniden yönlendirmesi için otomatik HTTP şifrelenmiş bir yolu, kullanıcıların uygulama arasındaki tüm iletişimi sağlamak için birçok web uygulamaları için ortak bir yeniden yönlendirme senaryosu desteklemektir. Geçmişte, müşteriler, HTTP, HTTPS için aldığı isteklerini yeniden yönlendirmek için tek amacı olan bir adanmış arka uç havuzu oluşturma gibi teknikler kullandınız. Application Gateway'de yeniden yönlendirme desteği sayesinde, bunu powershell'inizi yazarak yeni bir yeniden yönlendirme yapılandırması için yönlendirme kuralı ekleme ve hedef dinleyici olarak HTTPS protokolüne sahip başka bir dinleyici belirterek gerçekleştirebilirsiniz.

Yeniden yönlendirme aşağıdaki türleri desteklenir:

- 301 kalıcı bir yeniden yönlendirme
- 302 Bulundu
- 303 diğerini gör
- 307 geçici yeniden yönlendirme

Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

-  **Genel Yönlendirme**

   Tek bir dinleyici alanından başka bir ağ geçidi Dinleyicide yeniden yönlendirir. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
- **Yol tabanlı yeniden yönlendirme**

   Bu tür bir yeniden yönlendirme HTTP yalnızca belirli bir site alanı üzerinde HTTPS yeniden yönlendirmesi/Sepeti/bir alışveriş sepeti alanı örneğin gösterilen sağlar *.
- **Dış siteye yeniden yönlendirme**

![yeniden yönlendirme](./media/redirect-overview/redirect.png)

Bu değişiklik, müşterilere bir hedef dinleyici belirten yeni yeniden yönlendirme yapılandırma nesnesi veya dış siteye yeniden yönlendirme istenildiği gibi oluşturmanız gerekir. Yapılandırma öğesi, URI yolu ve sorgu dizesini URL'ye yeniden yönlendirilen ekleyerek etkinleştirmek için seçeneklerini de destekler. Yeniden yönlendirme türünü de seçebilirsiniz. Oluşturulduktan sonra yeni bir kural aracılığıyla kaynak dinleyicisi bu yeniden yönlendirme yapılandırması eklenir. Temel kural kullanırken, yeniden yönlendirme yapılandırması kaynak dinleyici ile ilişkili ve genel bir yeniden yönlendirme. Yola dayalı kural kullanıldığında, yeniden yönlendirme yapılandırması URL yolu haritada tanımlanır. Bu nedenle yalnızca bir sitenin belirli bir yol alanı için geçerlidir.

### <a name="next-steps"></a>Sonraki adımlar

[Bir uygulama ağ geçidinde URL yeniden yönlendirmesini yapılandırma](tutorial-url-redirect-powershell.md)

<!-- Update_Description: wording update -->