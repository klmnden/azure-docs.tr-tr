---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ea616786d69d41435be2a46e90d4973b21270935
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
1. Sanal ağ geçidinizin sayfasına gidip bu sayfayı açın. Buraya çeşitli yollardan gidebilirsiniz. Örneğimizde "VNet1GW" ağ geçidine gitmek için **TestVNet1 -> Genel bakış -> Bağlı cihazlar -> VNet1GW** yolunu kullandık.
2. VNet1GW sayfasında **Bağlantılar**'a tıklayın. Bağlantılar sayfasının üstündeki **+Ekle**’ye tıklayarak **Bağlantı ekle** sayfasını açın.

    ![Siteden Siteye bağlantısı oluşturma](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. **Bağlantı ekle** sayfasında bağlantınızı oluşturmak için değerleri girin.

  - **Ad:** Bağlantınızı adlandırın. Örneğimizde **VNet1toSite2** kullandık.
  - **Bağlantı türü:** **Siteden siteye (IPSec)** seçeneğini belirleyin.
  - **Sanal ağ geçidi:** Bu ağ geçidinden bağlandığınız için değer sabittir.
  - **Yerel ağ geçidi:** **Bir yerel ağ geçidi seçin**'e tıklayıp kullanmak istediğiniz yerel ağ geçidini seçin. Örneğimizde **Site2** kullandık.
  - **Paylaşılan Anahtar:** Buradaki değer, yerel şirket için VPN cihazınız için kullandığınız değerle eşleşmelidir. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer kullanabilirsiniz (ve kullanmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.
  - **Abonelik**, **Kaynak Grubu** ve **Konum** için kalan değerler sabittir.

4. Bağlantınızı oluşturmak için **Tamam**’a tıklayın. Ekranda yanıp sönen *Bağlantısı Oluşturuluyor* yazısını göreceksiniz.
5. Bağlantıyı sanal ağ geçidinin **Bağlantılar** sayfasında görüntüleyebilirsiniz. *Bilinmiyor* Durumu *Bağlanıyor* olarak ve ardından *Başarılı* olarak değişir.