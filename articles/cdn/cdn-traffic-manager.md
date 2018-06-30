---
title: Birden çok Azure CDN uç Azure Traffic Manager ile yük devretmeyi ayarlamak | Microsoft Docs
description: Azure Traffic Manager ile Azure CDN uç ayarlama hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: b52cad1f32cc3d16cf70bb81640dcb1d9f8614bf
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133125"
---
# <a name="set-up-failover-across-multiple-azure-cdn-endpoints-with-azure-traffic-manager"></a>Birden çok Azure CDN uç Azure Traffic Manager ile yük devretmeyi ayarlayın

Azure içerik teslim ağı (CDN) yapılandırdığınızda, gereksinimleriniz için en iyi sağlayıcısı ve fiyatlandırma katmanı seçebilirsiniz. Azure CDN Genel dağıtılmış altyapısıyla varsayılan olarak yerel ve coğrafi artıklık ve hizmet kullanılabilirliği ve performansı artırmak için genel yükü oluşturur. Bir konum içerik sunmak kullanılabilir değilse, istekleri otomatik olarak başka bir konuma yönlendirilir ve (istek konumu ve sunucusu yükleme gibi etkenlere bağlı olarak) en iyi POP her istemci isteği sunmak için kullanılır. 
 
Birden çok CDN profili varsa, daha fazla kullanılabilirlik ve performans Azure Traffic Manager ile artırabilir. Birden çok CDN uç noktaları arasında yük devretme, coğrafi Yük Dengeleme ve diğer senaryolar için dengelemek üzere Azure CDN ile Azure trafik Yöneticisi'ni kullanabilirsiniz. Normal yük devretme senaryoda, tüm istemci isteklerini ilk birincil CDN profili yönlendirilir; Profil kullanılabilir durumda değilse, birincil CDN profilinizi tekrar çevrimiçi olana kadar istekleri sonra ikincil CDN profili geçirilir. Bu şekilde Azure trafik Yöneticisi'ni kullanarak web uygulamanızın her zaman kullanılabilir sağlar. 

Bu makalede, kılavuz ve yük devretme kümelemesiyle ayarlama konusunda bir örnek sağlar **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri.

## <a name="set-up-azure-cdn"></a>Azure CDN ayarlayın 
İki veya daha fazla Azure CDN profili ve uç noktaları farklı sağlayıcıları ile oluşturun.

1. Create bir **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** içindeki adımları izleyerek profili [yeni bir CDN profili oluşturma](cdn-create-new-endpoint.md#create-a-new-cdn-profile).
 
   ![CDN birden çok profil](./media/cdn-traffic-manager/cdn-multiple-profiles.png)

2. İçindeki adımları izleyerek her yeni profilleri en az bir uç noktası oluşturma [yeni bir CDN uç noktası oluşturma](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint).

## <a name="set-up-azure-traffic-manager"></a>Azure trafik Yöneticisi ayarlama
Bir Azure Traffic Manager profili oluşturma ve Yük Dengeleme arasında CDN uç noktalarınızı ayarlayın. 

1. İçindeki adımları izleyerek bir Azure Traffic Manager profili oluşturma [bir Traffic Manager profili oluşturma](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile#create-a-traffic-manager-profile-1). 

    İçin **yönlendirme yöntemi**seçin **öncelik**.

2. İçindeki adımları izleyerek CDN uç noktalarınızı trafik Yöneticisi profilinize ekleyin [eklemek trafik Yöneticisi uç noktaları](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-create-profile#add-traffic-manager-endpoints)

    İçin **türü**seçin **dış uç noktalar**. İçin **öncelik**, bir sayı girin.

    Örneğin, oluşturma *cdndemo101akamai.azureedge.net* önceliği *1* ve *cdndemo101verizon.azureedge.net* önceliği *2*.

   ![CDN trafik Yöneticisi uç noktası](./media/cdn-traffic-manager/cdn-traffic-manager-endpoints.png)


## <a name="set-up-custom-domain-on-azure-cdn-and-azure-traffic-manager"></a>Azure CDN ve Azure trafik Yöneticisi özel etki alanı ayarlama
CDN ve trafik Yöneticisi profillerinizi ayarladıktan sonra DNS eşlemeyi ekleyin ve CDN uç noktası için özel etki alanı kaydetmek için aşağıdaki adımları izleyin. Bu örnekte, özel etki alanı adı, *cdndemo101.dustydogpetcare.online*.

1. GoDaddy gibi özel bir etki alanının etki alanı sağlayıcısının web sitesine gidin ve iki DNS CNAME girişleri oluşturun. 

    a. İlk CNAME girişi için cdnverify alt etki alanı ile özel etki alanınız CDN uç noktanızı eşleyin. Bu giriş, özel etki alanı trafik Yöneticisi için 2. adımda eklediğiniz CDN uç noktası kaydetmek için gerekli bir adımdır.

      Örneğin: 

      `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101akamai.azureedge.net`  

    b. İkinci CNAME girişi için cdnverify alt etki alanı olmayan özel etki alanınız CDN uç noktanızı eşleyin. Bu giriş, özel etki alanı trafik Yöneticisi eşler. 

      Örneğin: 
      
      `cdndemo101.dustydogpetcare.online  CNAME  cdndemo101.trafficmanager.net`   

    > [!NOTE]
    > Etki alanınızı şu anda dinamik ise ve yarıda kesilemez, bu adımı son yapın. Trafik Yöneticisi için özel etki alanı DNS güncelleştirmeden önce CDN uç noktası ve trafik yöneticisi etki alanı dinamik olduğundan emin olun.
    >


2.  Azure CDN profilinizden ilk CDN uç noktası (Akamai) seçin. Seçin **özel etki alanı Ekle** ve giriş *cdndemo101akamai.azureedge.net*. Özel etki alanı doğrulamak için onay işaretine yeşil olduğundan emin olun. 

    Azure CDN kullanan *cdnverify* bu kayıt işlemini tamamlamak için DNS eşlemesi doğrulamak için bir alt etki alanı. Daha fazla bilgi için bkz: [bir CNAME DNS kaydı oluşturma](cdn-map-content-to-custom-domain.md#create-a-cname-dns-record). Bu adım, Azure CDN özel etki alanı kendi isteklerine yanıt vermesini sağlayabilirsiniz tanımak etkinleştirir.

3.  Web sitesine özel etki alanınız için etki alanı sağlayıcısı dönün ve böylece özel etki alanı için ikinci bir CDN uç noktanız eşlendi oluşturduğunuz ilk DNS eşlemesi güncelleştirin.
                             
    Örneğin: 

    `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101verizon.azureedge.net`  

4. Azure CDN profilinizi ikinci CDN uç noktası (Verizon) seçin ve 2. adımı yineleyin. Seçin **özel etki alanı Ekle**ve giriş *cdndemo101akamai.azureedge.net*.
 
Bu adımları tamamladıktan sonra Yük devretme yetenekleri ile çoklu CDN hizmetinizi Azure Traffic Manager ile ayarlanır. Test erişmek kullanabileceksiniz özel etki alanınızı URL'lerden. İşlevselliğini sınamak için birincil CDN uç noktası devre dışı bırakın ve istek doğru ikincil CDN uç noktasına taşınır olduğunu doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Yönlendirme diğer yöntemleri, aşağıdaki gibi ayarlayabilirsiniz coğrafi farklı CDN uç noktalar arasında yük dengelemesi için. Daha fazla bilgi için bkz: [trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).



