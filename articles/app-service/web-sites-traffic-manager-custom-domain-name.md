---
title: "Azure App Service'te, Yük Dengeleme için trafik Yöneticisi kullanan bir web uygulaması için bir özel etki alanı adı yapılandırın."
description: "Bir özel etki alanı adı için bir web uygulamasını Azure App Service'te, Yük Dengeleme için trafik Yöneticisi'ni içerir."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: c78fb7883559e46ebaa1d8dab59a15c55fb76fdf
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak Azure App Service içinde bir web uygulaması için bir özel etki alanı adı yapılandırma
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Bu makalede bir özel etki alanı adı ile kullanmak için genel yönergeler sağlar bir [uygulama hizmeti](app-service-web-overview.md) ile tümleşik uygulama [trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) Yük Dengeleme için.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>DNS kayıtlarını anlama
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Web uygulamalarınızı Standart mod için yapılandırma
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Özel etki alanınız için DNS kaydı ekleyin
> [!NOTE]
> Azure App Service Web Apps aracılığıyla etki alanı satın aldıktan sonra aşağıdaki adımları atlayın ve son adımına başvurmak [Web uygulamaları için etki alanı satın](custom-dns-web-site-buydomains-web-app.md) makalesi.
> 
> 

Özel etki alanınızı Azure App Service'in web uygulamasında ilişkilendirmek için yeni bir giriş özel etki alanınız için DNS tabloda eklemeniz gerekir. Bunun için etki alanı sağlayıcınızdan yönetim araçlarını kullanarak.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Her etki alanı sağlayıcısının özellikleri değişir, ancak eşledikten *gelen* özel etki alanı adınızı (gibi **contoso.com**) *için* trafik yöneticisi etki alanı adı ( **contoso.trafficmanager.NET**) web uygulamanızı ile tümleşiktir.
   
> [!NOTE]
> Bir kayıt zaten kullanımda ve uygulamalarınızı erken önlem bağlamak gerekiyorsa, ek bir CNAME kaydı oluşturabilirsiniz. Örneğin, erken önlem bağlamak için **www.contoso.com** web uygulamanız için bir CNAME kayıt oluşturma **awverify.www** için **contoso.trafficmanager.net**. Ardından "www.contoso.com", "www" CNAME kaydı değiştirmeden, Web uygulamanızın ekleyebilirsiniz. Daha fazla bilgi için bkz: [özel bir etki alanındaki bir web uygulaması oluşturma DNS kayıtlarını][CREATEDNS].
> 
> 

Ekleme veya etki alanı Sağlayıcınızdaki DNS kayıtlarını değiştirme tamamladıktan sonra değişiklikleri kaydedin.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Trafik Yöneticisi'ni etkinleştir
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](/develop/nodejs/).

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
