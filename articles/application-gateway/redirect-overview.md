---
title: Azure uygulama ağ geçidi için yeniden yönlendirmeye genel bakış
description: Azure uygulama ağ geçidi yeniden yönlendirme özelliği hakkında bilgi edinin
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/19/2018
ms.author: amsriva
ms.openlocfilehash: f503998668f35ec5bc17db74ee74c4a26465ab04
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="application-gateway-redirect-overview"></a>Uygulama ağ geçidi yeniden yönlendirmeye genel bakış

Birçok web uygulamaları için yaygın bir senaryo, şifrelenmiş bir yolu üzerinden uygulama ve onun kullanıcıları arasındaki tüm iletişimi sağlamak için otomatik HTTP HTTPS yeniden yönlendirmesi için desteklemektir. Geçmişte, müşteriler, HTTPS için http aldığı isteklerini yeniden yönlendirmek için tek amacı olan bir adanmış arka uç havuzu oluşturma gibi teknikler kullandınız.

Uygulama ağ geçidi artık ağ geçidinde trafiği yönlendirmek için özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yönlendirme dahil olmak üzere yeni yeniden yönlendirme senaryoları destekler. Uygulama ağ geçidi yeniden yönlendirme desteği için HTTP sınırlı değildir HTTPS yeniden yönlendirmesi tek başına ->. Uygulama ağ geçidi başka bir dinleyici için bir dinleyici geliş trafik yeniden yönlendirmesi izin veren bir genel yönlendirme mekanizması vardır. Dış site yeniden yönlendirme da desteklenir.

Uygulama ağ geçidi yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

-  **Genel Yönlendirme**

   Ağ geçidi başka bir dinleyici bir dinleyici gelen yönlendirir. Bu, bir sitede HTTPS yeniden yönlendirmesi için HTTP sağlar.
- **Yol tabanlı yeniden yönlendirme**

   Bu tür bir yeniden yönlendirme HTTP HTTPS yeniden yönlendirme yalnızca belirli site alanında, örneğin bir alışveriş sepeti alanı/Sepeti/gösterilen sağlar *.
- **Dış siteye yeniden yönlendirme**

![yeniden yönlendirme](./media/redirect-overview/redirect.png)

Bu değişiklikle müşterileri hedef dinleyici belirten yeni yeniden yönlendirme yapılandırma nesnesi veya yeniden yönlendirme istenen dış site oluşturmanız gerekir. Yapılandırma öğesi de yeniden yönlendirilen URL URI yol ve sorgu dizesini ekleyerek etkinleştirmek için seçeneklerini destekler. Yeniden yönlendirme geçici (HTTP durum kodu 302) ya da kalıcı bir yeniden yönlendirme (HTTP durum kodu 301) olup olmadığını da seçebilirsiniz. Oluşturduktan sonra bu yeniden yönlendirme yapılandırma kaynağı dinleyicisi aracılığıyla yeni bir kural eklenmiş. Temel bir kural kullanırken, yeniden yönlendirme yapılandırma kaynağı dinleyicisi ile ilişkili ve genel bir yeniden yönlendirme. Yol tabanlı bir kural kullanıldığında, yeniden yönlendirme yapılandırması URL yolu haritada tanımlanır. Bu nedenle yalnızca bir sitenin belirli yol alanına uygular.

### <a name="next-steps"></a>Sonraki adımlar

[Bir uygulama ağ geçidinde URL yeniden yönlendirmeyi yapılandırma](tutorial-url-redirect-powershell.md)
