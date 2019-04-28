---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 19ad4e39ca4e402c37b2cfa69c7c306b6e5a2766
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60407794"
---
1. Sanal ağ geçidinizin sayfasına gidip bu sayfayı açın. Buraya çeşitli yollardan gidebilirsiniz. 'VNet1GW' ağ geçidine gitmek için **TestVNet1 -> Genel bakış -> Bağlı cihazlar -> VNet1GW** seçeneğine gidebilirsiniz.
2. VNet1GW sayfasında **Bağlantılar**'a tıklayın. Bağlantılar sayfasının üstündeki **+Ekle**’ye tıklayarak **Bağlantı ekle** sayfasını açın.

   ![Siteden Siteye bağlantısı oluşturma](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. **Bağlantı ekle** sayfasında, bağlantınız için değerleri yapılandırın.

   - **Adı:** Bağlantınızı adlandırın.
   - **Bağlantı türü:** Seçin **Site-to-site(IPSec)**.
   - **Sanal ağ geçidi:** Bu ağ geçidinden bağlandığınız için değer sabittir.
   - **Yerel ağ geçidi:** Tıklayın **bir yerel ağ geçidi seçin** ve kullanmak istediğiniz yerel ağ geçidini seçin.
   - **Paylaşılan Anahtar:** Buradaki değer, yerel şirket için VPN cihazınız için kullandığınız değerle eşleşmelidir. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer kullanabilirsiniz (ve kullanmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.
   - **Abonelik**, **Kaynak Grubu** ve **Konum** için kalan değerler sabittir.

4. Bağlantınızı oluşturmak için **Tamam**’a tıklayın. Ekranda yanıp sönen *Bağlantısı Oluşturuluyor* yazısını göreceksiniz.
5. Bağlantıyı sanal ağ geçidinin **Bağlantılar** sayfasında görüntüleyebilirsiniz. *Bilinmiyor* Durumu *Bağlanıyor* olarak ve ardından *Başarılı* olarak değişir.
