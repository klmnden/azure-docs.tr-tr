---
title: Genel bakış, Azure Application Gateway için yeniden yönlendirme | Microsoft Docs
description: Azure Application Gateway'de yeniden yönlendirme özelliği hakkında bilgi edinin
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
origin.date: 07/18/2017
ms.date: 01/15/2019
ms.author: v-junlch
ms.openlocfilehash: d05d509b67fd26c958e0e2fa2bbd877db26e6521
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831760"
---
# <a name="application-gateway-redirect-overview"></a>Application Gateway yeniden yönlendirmeye genel bakış

HTTPS yeniden yönlendirmesi için otomatik HTTP şifrelenmiş bir yolu, kullanıcıların uygulama arasındaki tüm iletişimi sağlamak için birçok web uygulamaları için yaygın bir senaryo desteklemektir. Geçmişte, müşteriler, HTTP, HTTPS için aldığı isteklerini yeniden yönlendirmek için tek amacı olan bir adanmış arka uç havuzu oluşturma gibi teknikler kullandınız.  Application gateway, uygulama ağ geçidinde trafiği yeniden yönlendirme özelliği artık desteklemektedir. Bu uygulama yapılandırmasını basitleştiren, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme de dahil olmak üzere yeni yeniden yönlendirme senaryoları destekler. Uygulama ağ geçidi yeniden yönlendirme desteği için HTTP sınırlı değildir -> yalnızca HTTPS yeniden yönlendirmesi. Uygulama ağ geçidi üzerinde başka bir dinleyici için tek bir dinleyici geliş trafiği yeniden yönlendirme izin veren bir genel yönlendirme mekanizma budur. Ayrıca, bir dış siteye yönlendirmeyi de destekler. Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

1. Ağ geçidi üzerinde başka bir dinleyici için genel yönlendirme gelen bir dinleyici. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
2. Yol tabanlı yönlendirme. Bu tür bir yeniden yönlendirme HTTP yalnızca belirli bir site alanı üzerinde HTTPS yeniden yönlendirmesi/Sepeti/bir alışveriş sepeti alanı örneğin gösterilen sağlar *.
3. Dış siteye yeniden yönlendirme.

![yeniden yönlendirme](./media/application-gateway-redirect-overview/redirect.png)

Bu değişiklik, müşterilere bir hedef dinleyici belirten yeni yeniden yönlendirme yapılandırma nesnesi veya dış siteye yeniden yönlendirme istenildiği gibi oluşturmanız gerekir. Yapılandırma öğesi, URI yolu ve sorgu dizesini URL'ye yeniden yönlendirilen ekleyerek etkinleştirmek için seçeneklerini de destekler. Müşteriler, yeniden yönlendirme (HTTP durum kodu 302) geçici veya kalıcı bir yeniden yönlendirme (HTTP durum kodu 301) olup olmadığını da seçebilirsiniz. Bu yeniden yönlendirme yapılandırması oluşturduktan sonra yeni bir kural aracılığıyla kaynak dinleyicisi eklenir. Temel kural kullanırken, yeniden yönlendirme yapılandırması kaynak dinleyici ile ilişkili ve genel bir yeniden yönlendirme. Yola dayalı kural kullanıldığında, yeniden yönlendirme yapılandırması URL yolu haritada tanımlanır ve bu nedenle yalnızca bir sitenin belirli bir yol alanı için geçerlidir.

### <a name="next-steps"></a>Sonraki adımlar

[Bir uygulama ağ geçidinde HTTPS yeniden yönlendirmesi için HTTP yapılandırın](redirect-http-to-https-portal.md)

<!-- Update_Description: wording update -->