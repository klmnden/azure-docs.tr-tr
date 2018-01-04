---
title: "Uygulama ağ geçidi - Azure portalı için bir yol tabanlı kuralı oluşturun | Microsoft Docs"
description: "Azure portalı kullanarak bir uygulama ağ geçidi için bir yol tabanlı kuralı oluşturmayı öğrenin."
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: b207e7e7bd83e56db68288190c7bedafa8b5b7fa
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-azure-portal"></a>Azure portalı kullanarak bir uygulama ağ geçidi için bir yol tabanlı kuralı oluşturma

> [!div class="op_single_selector"]
> * [Azure portalı](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL yolu tabanlı yönlendirme, HTTP isteklerini URL yola göre yollar ilişkilendirin. Uygulama ağ geçidi listelenen URL için yapılandırılmış bir arka uç sunucu havuzuna bir yolu yoktur ve ardından ağ trafiğini tanımlı havuzuna gönderir olup olmadığını denetler. URL yolu tabanlı yönlendirme için bir ortak Yük Dengeleme isteklerini farklı arka uç sunucu havuzu için farklı içerik türleri için kullanılır.

Uygulama ağ geçidine sahip iki kural türü: basic ve URL yolunu tabanlı kurallar. Temel kural türünü, arka uç havuzları hepsini hizmetidir. Yol tabanlı kuralları, hepsini dağıtım yanı sıra da yol deseni istek URL'si, uygun arka uç havuzu seçerken kullanın.

## <a name="scenario"></a>Senaryo

Aşağıdaki senaryoyu yol tabanlı bir kural, mevcut bir uygulama ağ geçidi oluşturur.
Bu senaryo adımları zaten izlediğinizden varsayar [portal ile bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway-portal.md).

![URL rota][scenario]

## <a name="createrule"></a>Yol tabanlı kuralı oluşturma

Yol tabanlı bir kuralı, kendi dinleyici gerektirir. Kural oluşturmadan önce kullanmak için kullanılabilir bir dinleyici yüklü olduğunu doğrulamak emin olun.

### <a name="step-1"></a>1. Adım

Git [Azure portal](http://portal.azure.com) ve var olan bir uygulama ağ geçidi seçin. Tıklatın **kuralları**.

![Application Gateway’e genel bakış][1]

### <a name="step-2"></a>2. Adım

Tıklatın **yol tabanlı** yol tabanlı yeni bir kural eklemek için düğmeyi.

### <a name="step-3"></a>3. Adım

**Yol tabanlı Kuralı Ekle** dikey iki bölümü vardır. Dinleyici, kural ve varsayılan yol ayarları adını tanımlandığı ilk bölümdür. Özel yol tabanlı rota altında kalan yok rotalar için varsayılan yol ayarları ayarlardır. İkinci bölümü **yol tabanlı Kuralı Ekle** dikey olan yol tabanlı kurallar kendilerini burada tanımlayın.

**Temel ayarlar**

* **Ad**: Portalı'nda erişilebilir olan kural için bir kolay ad.
* **Dinleyici**: kural için kullanılan dinleyicisi.
* **Varsayılan arka uç havuzu**: varsayılan kuralı için kullanılacak arka uç.
* **Varsayılan HTTP ayarları**: varsayılan kuralı için kullanılacak HTTP ayarları.

**Yol tabanlı kural ayarları**

* **Ad**: yol tabanlı kuralı için bir kolay ad.
* **Yollar**: kural zaman arar trafiği iletirken yolu.
* **Arka uç havuzu**: kural için kullanılacak arka uç.
* **HTTP ayarı**: kural için kullanılacak HTTP ayarları.

> [!IMPORTANT]
> **Yolları** eşleştirilecek yol desenleri listesi bir ayardır. Her desen eğik çizgiyle başlamalıdır ve bir yıldız işareti yalnızca sonunda izin verilir. Geçerli örnekler: /xyz, /xyz*ve /xyz/*.  

![Doldurulan bilgilerle Ekle yol tabanlı kural dikey penceresi][2]

Var olan bir uygulama ağ geçidi için bir yol tabanlı kuralın eklenmesi, Azure Portalı aracılığıyla kolay bir işlemdir. Yol tabanlı bir kural oluşturduktan sonra ek kurallar içerecek şekilde düzenleyebilirsiniz. 

![Ek yol tabanlı kuralları ekleme][3]

Bu adım, yol tabanlı rota yapılandırır. İstekleri değil yazılır anlamak önemlidir. İstekler geldikçe gibi uygulama ağ geçidi isteği inceler ve URL deseni temel alınarak, uygun arka uç havuzu isteği gönderir.

## <a name="next-steps"></a>Sonraki adımlar

Azure uygulama ağ geçidi ile SSL boşaltma yapılandırma konusunda bilgi edinmek için [Azure portalını kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi yapılandırma](application-gateway-ssl-portal.md).

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
