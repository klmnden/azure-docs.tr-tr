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
ms.openlocfilehash: d9825ea41937dc9436fe8b465b48b378e13407c1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188243"
---
1. Portalda **Tüm kaynaklar** menüsündeki **+Ekle**’ye tıklayın.
2. İçinde **her şeyi** sayfa arama kutusuna **yerel ağ geçidi**, ardından kaynakları listesini döndürmek için tıklatın. **Yerel ağ geçidi**’ne tıklayarak sayfayı açın, ardından **Oluştur**’a tıklayarak **Yerel ağ geçidi oluştur** sayfasını açın.

   ![yerel ağ geçidi oluşturma](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)
3. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

   - **Adı:** Yerel ağ geçidi nesneniz için bir ad belirtin. Mümkünse, sezgisel gibi bir şey kullanmak **ClassicVNetLocal** veya **TestVNet1Local**. Bu, yerel ağ geçidi portalında belirleyebilmeniz için kolaylaştırır.
   - **IP adresi:** Geçerli bir genel belirtin **IP adresi** bağlanmak istediğiniz VPN cihazı ya da sanal ağ geçidi için.

     * **Bu yerel ağ, şirket içi bir konumu temsil ediyorsa:** Bağlanmak istediğiniz VPN cihazının genel IP adresini belirtin. NAT’nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir.
     * **Bu yerel ağ başka bir Vnet'i temsil ediyorsa:** O VNet için sanal ağ geçidine atanan genel IP adresini belirtin.
     * **IP adresi henüz yoksa:** Geçerli bir yer tutucu IP adresi yapmak ve sonra geri dönün ve bağlanmadan önce bu ayarı değiştirmek.
   - **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun.
   - **BGP ayarları yapılandırın:** Yalnızca BGP'yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
   - **Abonelik:** Doğru aboneliğin gösterildiğini doğrulayın.
   - **Kaynak grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
   - **Konum:** Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.

4. Yerel ağ geçidi oluşturmak için **Oluştur**’a tıklayın.
