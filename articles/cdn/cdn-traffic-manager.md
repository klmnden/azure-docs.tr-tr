---
title: Birden fazla Azure CDN uç noktası ile Azure Traffic Manager arasında devretmeyi ayarlamak | Microsoft Docs
description: Azure CDN uç noktaları ile Azure Traffic Manager'ı ayarlama hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: afadef8b29927f909af5be1e1204180724258b74
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60324020"
---
# <a name="set-up-failover-across-multiple-azure-cdn-endpoints-with-azure-traffic-manager"></a>Birden fazla Azure CDN uç noktası ile Azure Traffic Manager arasında devretmeyi ayarlayın

Azure Content Delivery Network (CDN) yapılandırdığınızda, ihtiyaçlarınız için en iyi sağlayıcısı ve fiyatlandırma katmanı seçebilirsiniz. Azure CDN, Global olarak dağıtılmış, altyapı ile varsayılan olarak yerel ve coğrafi yedeklilik ve hizmet kullanılabilirliği ve performansı artırmak için genel yük oluşturur. Bir konum içerik sunmak kullanılabilir değilse, istekleri başka bir konuma otomatik olarak yönlendirilir ve en iyi bulunma noktası (istek konumu ve sunucusu yük gibi etkenlere göre), her istemci isteğe hizmet vermek için kullanılır. 
 
Birden çok CDN profiliniz varsa, kullanılabilirliği ve performansı Azure Traffic Manager ile daha da artırabilirsiniz. Azure CDN ile Azure Traffic Manager, yük dengelemek birden çok CDN uç noktaları arasında yük devretme, coğrafi Yük Dengeleme ve diğer senaryolar için kullanabilirsiniz. Bir genel yük devretme senaryosunda, tüm istemci isteklerini ilk birincil CDN profiline yönlendirilmiş; Profil kullanılabilir durumda değilse, birincil CDN profilinizi tekrar çevrimiçi olana kadar istek daha sonra ikincil CDN profiline geçirilir. Bu şekilde Azure Traffic Manager'ı kullanarak web uygulamanızı her zaman kullanılabilir sağlar. 

Bu makalede, rehberlik ve yük devretme ile ayarlamak nasıl bir örnek sağlanmaktadır **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri.

## <a name="set-up-azure-cdn"></a>Azure CDN ' ayarlayın 
Azure CDN profili ve uç noktaları iki veya daha fazla farklı sağlayıcıları ile oluşturun.

1. Oluşturma bir **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** adımları izleyerek profil [yeni bir CDN profili oluşturmak](cdn-create-new-endpoint.md#create-a-new-cdn-profile).
 
   ![CDN birden çok profil](./media/cdn-traffic-manager/cdn-multiple-profiles.png)

2. İçindeki adımları izleyerek her yeni profilleri en az bir uç nokta oluşturma [yeni bir CDN uç noktası](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint).

## <a name="set-up-azure-traffic-manager"></a>Azure Traffic Manager'ı ayarlama
Bir Azure Traffic Manager profili oluşturun ve CDN uç noktalarınızı arasında yük dengelemeyi ayarlama. 

1. İçindeki adımları izleyerek bir Azure Traffic Manager profili oluşturma [Traffic Manager profili oluşturma](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile). 

    İçin **yönlendirme yöntemi**seçin **öncelik**.

2. CDN uç noktalarınızı içindeki adımları izleyerek Traffic Manager profilinize ekleyin [ekleme Traffic Manager uç noktaları](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile#add-traffic-manager-endpoints)

    İçin **türü**seçin **dış uç noktalar**. İçin **öncelik**, bir sayı girin.

    Örneğin, oluşturma *cdndemo101akamai.azureedge.net* önceliği *1* ve *cdndemo101verizon.azureedge.net* önceliği *2*.

   ![CDN traffic manager uç noktası](./media/cdn-traffic-manager/cdn-traffic-manager-endpoints.png)


## <a name="set-up-custom-domain-on-azure-cdn-and-azure-traffic-manager"></a>Azure Traffic Manager ve Azure CDN özel etki alanında ayarlama
CDN ve Traffic Manager profillerinizi ayarladıktan sonra DNS eşlemesi ekleyin ve özel etki alanını CDN uç noktalarına kaydetmek için bu adımları izleyin. Bu örnekte, özel etki alanı adıdır *cdndemo101.dustydogpetcare.online*.

1. GoDaddy gibi özel etki alanınızın etki alanı sağlayıcınız için web sitesine gidin ve iki DNS CNAME girişleri oluşturulamıyor. 

    a. İlk CNAME girişi için cdnverify alt etki alanı, özel etki alanınızı CDN uç noktanıza eşleyin. Bu giriş, 2. adımda eklediğiniz Traffic Manager için CDN uç noktasına özel etki alanı kaydetmek için gerekli bir adımdır.

      Örneğin: 

      `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101akamai.azureedge.net`  

    b. İkinci CNAME girişi için cdnverify alt etki alanı olmadan özel etki alanınızı CDN uç noktanıza eşleyin. Bu giriş, özel etki alanını Traffic Manager eşler. 

      Örneğin: 
      
      `cdndemo101.dustydogpetcare.online  CNAME  cdndemo101.trafficmanager.net`   

    > [!NOTE]
    > Bu adımı, etki alanınız şu anda canlı ve yarıda kesilemez, son uygulayın. Özel DNS etki alanınızı Traffic Manager'a güncelleştirmeden önce CDN uç noktaları ve traffic manager etki alanı dinamik olduğunu doğrulayın.
    >


2.  Azure CDN profilinizi, ilk CDN uç noktası (Akamai) seçin. Seçin **özel etki alanı Ekle** ve giriş *cdndemo101.dustydogpetcare.online*. Özel etki alanını doğrulamak için onay işareti yeşil olduğundan emin olun. 

    Azure CDN kullanan *cdnverify* kayıt işlemini tamamlamak için bir DNS eşlemesi doğrulamak için alt etki alanı. Daha fazla bilgi için [bir CNAME DNS kaydı oluşturma](cdn-map-content-to-custom-domain.md#create-a-cname-dns-record). Bu adım, Azure CDN, böylece kendi isteklerini yanıtlayan özel etki alanı tanımak etkinleştirir.

3.  Özel etki alanınızın etki alanı sağlayıcınız için web sitesine dönün ve böylece özel etki alanı için ikinci bir CDN uç noktanızla eşlendi oluşturduğunuz ilk DNS eşlemesini güncelleştirin.
                             
    Örneğin: 

    `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101verizon.azureedge.net`  

4. Azure CDN profilinizi ikinci CDN uç noktası (Verizon) seçin ve 2. adımı yineleyin. Seçin **özel etki alanı Ekle**ve giriş *cdndemo101.dustydogpetcare.online*.
 
Bu adımları tamamladıktan sonra Yük devretme özellikleriyle çoklu CDN hizmetiniz Azure Traffic Manager ile ayarlanır. Test erişmek mümkün olacaktır özel etki alanınızı URL'lerden. İşlevselliğini test etmek için birincil CDN uç noktası devre dışı bırakın ve istek doğru şekilde ikincil CDN uç noktasına taşınır olduğunu doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Yönlendirme diğer yöntemleri, aşağıdaki gibi ayarlayabilirsiniz coğrafi farklı CDN uç noktaları arasında yük dengelemek için. Daha fazla bilgi için [Traffic Manager'ı kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).



