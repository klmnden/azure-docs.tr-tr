---
title: Traffic Manager - Azure App Service kullanan uygulamalar için DNS adlarını yapılandırın
description: Bir özel etki alanı adı için bir Azure App Service, Traffic Manager Yük Dengeleme için içeren bir web uygulaması.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: b207d9e3123245e5cb7dddb625f7488886591ae5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128540"
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Traffic Manager'ı kullanarak Azure App Service içinde bir web uygulaması için özel etki alanı adı yapılandırma
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Bu makalede bir özel etki alanı adı ile kullanmak için genel yönergeler sağlayan bir [App Service](overview.md) ile tümleşik uygulama [Traffic Manager](../traffic-manager/traffic-manager-overview.md) Yük Dengeleme için.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>DNS kayıtlarını anlama
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Standart mod, web uygulamalarını yapılandırma
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Özel etki alanınız için bir DNS kaydı ekleyin
> [!NOTE]
> Azure App Service Web Apps aracılığıyla etki alanı satın aldıysanız sonra aşağıdaki adımları atlayın ve son adımına başvurmak [etki alanı satın almak için Web Apps](manage-custom-dns-buy-domain.md) makalesi.
> 
> 

Özel etki alanınızı Azure App Service'te bir web uygulaması ile ilişkilendirmek için yeni bir giriş özel etki alanınız için DNS tablosunda eklemeniz gerekir. Bunun için etki alanı sağlayıcınızın yönetim araçlarını kullanarak.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Her etki alanı sağlayıcısının özellikleri göstermekle eşlemeniz *gelen* özel etki alanı adınızı (gibi **contoso.com**) *için* Traffic Manager etki alanı adı ( **contoso.trafficmanager.NET**) web uygulamanızı ile tümleşiktir.

> [!NOTE]
> Bir kaydı zaten kullanımda ve uygulamalarınızı sıd'lerde bağlamak gerekiyorsa, ek bir CNAME kaydı oluşturabilirsiniz. Örneğin, sıd'lerde bağlamak için **www\.contoso.com** web uygulamanız için bir CNAME kayıt oluşturma **awverify.www** için **contoso.trafficmanager.net**. Daha sonra ekleyebilirsiniz "www\.contoso.com" "www" CNAME kaydı değiştirmeden Web uygulamanıza. Daha fazla bilgi için [özel bir etki alanı içinde bir web uygulaması için oluşturma DNS kayıtlarını][CREATEDNS].

Etki alanı sağlayıcınız, DNS kayıtlarını değiştirme veya ekleme işlemini tamamladıktan sonra değişiklikleri kaydedin.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Traffic Manager'ı etkinleştir
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/).

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
