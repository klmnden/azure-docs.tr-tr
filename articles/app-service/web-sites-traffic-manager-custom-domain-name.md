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
ms.openlocfilehash: 5f099201d9018a6f8577cb3daf127d09560fb94b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak Azure App Service içinde bir web uygulaması için bir özel etki alanı adı yapılandırma
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Bu makalede Yük Dengeleme için trafik Yöneticisi kullanan bir özel etki alanı adı Azure App Service ile kullanmak için genel yönergeler sağlar.

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

Özel etki alanınızı Azure App Service'in web uygulamasında ilişkilendirmek için yeni bir giriş DNS tabloda özel etki alanınız için etki alanı adınızı satın alınan etki alanı kayıt şirketi tarafından sağlanan araçları kullanarak eklemelisiniz. Bulun ve DNS araçları kullanmak için aşağıdaki adımları kullanın.

1. Etki alanı kayıt şirketinizdeki hesabınızda oturum açın ve DNS kayıtlarını yönetme için bir sayfa arayın. Bağlantıları veya olarak etiketli sitesinin alanları arayın **etki alanı adı**, **DNS**, veya **adı sunucu yönetimi**. Bu sayfaya bir bağlantı genellikle bulunabilir hesap bilgilerinizi görüntülemek ve bir bağlantı gibi arayan **My etki alanları**.
2. Etki alanı adınız için Yönetim sayfasında bulduktan sonra DNS kaydını düzenlemek izin veren bir bağlantı için bakın. Bu olarak listelenebilir bir **bölge dosyası**, **DNS kayıtlarını**, ya da farklı bir **Gelişmiş** yapılandırma bağlantısı.
   
   * Sayfa büyük olasılıkla zaten oluşturulmuş, bir giriş ilişkilendirme gibi birkaç kayıtları vardır '**@**'veya'\*' bir 'etki alanı park' sayfasıyla. Ayrıca ortak alt etki alanları için kayıtları gibi içerebilir **www**.
   * Sayfa Bahsediyor **CNAME kayıtları**, veya bir kayıt türü seçmek için aşağı açılan sağlayın. Bu ayrıca diğer kayıtları gibi bahsedebilir **A kayıtlarını** ve **MX kayıtları**. Bazı durumlarda, CNAME kayıtları diğer adlarına göre gibi çağrılacak bir **diğer ad kaydı**.
   * Sayfa ayrıca izin alanları olacaktır **harita** gelen bir **ana bilgisayar adı** veya **etki alanı adı** başka bir etki alanı adı.
3. Her kayıt şirketi ayrıntılarını değişir, ancak genel eşledikten *gelen* özel etki alanı adınızı (gibi **contoso.com**,) *için* trafik yöneticisi etki alanı adı (**contoso.trafficmanager.net**) web uygulamanız için kullanılır.
   
   > [!NOTE]
   > Alternatif olarak, bir kayıt zaten kullanımda ve uygulamalarınızı erken önlem bağlamak gerekiyorsa, ek bir CNAME kaydı oluşturabilirsiniz. Örneğin, erken önlem bağlamak için **www.contoso.com** web uygulamanız için bir CNAME kayıt oluşturma **awverify.www** için **contoso.trafficmanager.net**. Ardından "www.contoso.com", "www" CNAME kaydı değiştirmeden, Web uygulamanızın ekleyebilirsiniz. Daha fazla bilgi için bkz: [özel bir etki alanındaki bir web uygulaması oluşturma DNS kayıtlarını][CREATEDNS].
   > 
   > 
4. Ekleme veya kayıt şirketinizdeki DNS kayıtlarını değiştirme tamamladıktan sonra değişiklikleri kaydedin.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Trafik Yöneticisi'ni etkinleştir
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](/develop/nodejs/).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
