---
title: Azure uygulama ağ geçidi için genel bakış yönlendirmek | Microsoft Docs
description: Azure uygulama ağ geçidi yeniden yönlendirme özelliği hakkında bilgi edinin
services: application-gateway
documentationcenter: na
author: amsriva
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: amsriva
ms.openlocfilehash: e6352873ea055965b433fbf3e6e46162890e5fec
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="application-gateway-redirect-overview"></a>Uygulama ağ geçidi yeniden yönlendirmeye genel bakış

Birçok web uygulamaları için yaygın bir senaryo, şifrelenmiş bir yolu üzerinden uygulama ve onun kullanıcıları arasındaki tüm iletişimi sağlamak için otomatik HTTP HTTPS yeniden yönlendirmesi için desteklemektir. Geçmişte, müşteriler, HTTPS için http aldığı isteklerini yeniden yönlendirmek için tek amacı olan bir adanmış arka uç havuzu oluşturma gibi teknikler kullandınız.  Uygulama ağ geçidi artık uygulama ağ geçidi trafiği yönlendirmek için özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yönlendirme dahil olmak üzere yeni yeniden yönlendirme senaryoları destekler. Uygulama ağ geçidi yeniden yönlendirme desteği için HTTP sınırlı değildir HTTPS yeniden yönlendirmesi tek başına ->. Uygulama ağ geçidi başka bir dinleyici için bir dinleyici adresindeki alınan trafik yeniden yönlendirilmesine izin veren bir genel yönlendirme mekanizması budur. Ayrıca, yeniden yönlendirme bir dış siteye de destekler. Uygulama ağ geçidi yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

1. Ağ geçidi başka bir dinleyici için genel yönlendirmesi bir dinleyici gelen. Bu, bir sitede HTTPS yeniden yönlendirmesi için HTTP sağlar.
2. Yol tabanlı yönlendirmesi. Bu tür bir yeniden yönlendirme HTTP HTTPS yeniden yönlendirme yalnızca belirli site alanında, örneğin bir alışveriş sepeti alanı/Sepeti/gösterilen sağlar *.
3. Dış sitesine yeniden yönlendirebilir.

![yeniden yönlendirme](./media/application-gateway-redirect-overview/redirect.png)

Bu değişiklikle, müşteriler hedef dinleyici belirten yeni yeniden yönlendirme yapılandırma nesnesi veya yeniden yönlendirme istenen dış site oluşturmanız gerekir. Yapılandırma öğesi de yeniden yönlendirilen URL URI yol ve sorgu dizesini ekleyerek etkinleştirmek için seçeneklerini destekler. Müşteriler, yeniden yönlendirme geçici (HTTP durum kodu 302) ya da kalıcı bir yeniden yönlendirme (HTTP durum kodu 301) olup olmadığını da tercih edebilirsiniz. Bu yeniden yönlendirme yapılandırması oluşturulduktan sonra yeni bir kural aracılığıyla kaynak dinleyicisi bağlı. Temel bir kural kullanırken, yeniden yönlendirme yapılandırma kaynağı dinleyicisi ile ilişkili ve genel bir yeniden yönlendirme. Yol tabanlı bir kural kullanıldığında, yeniden yönlendirme yapılandırması URL yolu haritada tanımlanır ve bu nedenle yalnızca bir sitenin belirli yol alanına uygular.

### <a name="next-steps"></a>Sonraki adımlar

[Bir uygulama ağ geçidinde URL yeniden yönlendirmeyi yapılandırma](application-gateway-configure-redirect-powershell.md)
