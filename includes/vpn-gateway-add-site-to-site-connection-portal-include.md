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
ms.openlocfilehash: 0dcb591db5f487a103feccf92ffa577a1cbf8b23
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30921597"
---
1. Sanal ağ geçidinizin sayfasına gidip bu sayfayı açın. Buraya çeşitli yollardan gidebilirsiniz. 'VNet1GW' ağ geçidine gitmek için **TestVNet1 -> Genel bakış -> Bağlı cihazlar -> VNet1GW** seçeneğine gidebilirsiniz.
2. VNet1GW sayfasında **Bağlantılar**'a tıklayın. Bağlantılar sayfasının üstündeki **+Ekle**’ye tıklayarak **Bağlantı ekle** sayfasını açın.

  ![Siteden Siteye bağlantısı oluşturma](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. **Bağlantı ekle** sayfasında, bağlantınız için değerleri yapılandırın.

  - **Ad:** Bağlantınızı adlandırın.
  - **Bağlantı türü:** **Siteden siteye (IPSec)** seçeneğini belirleyin.
  - **Sanal ağ geçidi:** Bu ağ geçidinden bağlandığınız için değer sabittir.
  - **Yerel ağ geçidi:** **Bir yerel ağ geçidi seçin**'e tıklayıp kullanmak istediğiniz yerel ağ geçidini seçin.
  - **Paylaşılan Anahtar:** Buradaki değer, yerel şirket için VPN cihazınız için kullandığınız değerle eşleşmelidir. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer kullanabilirsiniz (ve kullanmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.
  - **Abonelik**, **Kaynak Grubu** ve **Konum** için kalan değerler sabittir.

4. Bağlantınızı oluşturmak için **Tamam**’a tıklayın. Ekranda yanıp sönen *Bağlantısı Oluşturuluyor* yazısını göreceksiniz.
5. Bağlantıyı sanal ağ geçidinin **Bağlantılar** sayfasında görüntüleyebilirsiniz. *Bilinmiyor* Durumu *Bağlanıyor* olarak ve ardından *Başarılı* olarak değişir.