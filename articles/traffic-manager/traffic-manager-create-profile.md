---
title: "Bir Traffic Manager profilini oluşturma | Microsoft Docs"
description: "Bu makalede bir Traffic Manager profilini oluşturma"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: kumud
ms.openlocfilehash: 1994c8374244b62e65b1a54234775d9a39f72bb3
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Bu makalede bir profille nasıl **öncelik** yönlendirme türü, iki Azure Web Apps uç nokta için rota kullanıcılara oluşturulabilir. Kullanarak **öncelik** yedek olarak ikinci tutarken türü yönlendirme, tüm trafik ilk uç noktasına yönlendirilir. Sonuç olarak, ilk bitiş noktasının bozulursa kullanıcıların ikinci uç noktasına yönlendirilebilir.

Bu makalede, iki önceden oluşturulmuş Azure Web uygulaması uç nokta Traffic Manager profilini yeni oluşturulan bu ilişkilendirilir. Azure Web uygulaması uç noktaları oluşturma hakkında daha fazla bilgi için ziyaret [Azure Web Apps belge sayfasının](https://docs.microsoft.com/azure/app-service/). Bir DNS adı ve genel internet üzerinden erişilebilen herhangi bir uç nokta ekleme yapabilir ve biz örnek olarak Azure Web Apps uç noktaları kullanıyoruz.

### <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma
1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Zaten bir hesabınız yoksa, kaydolmak için yapabileceğiniz bir [ücretsiz bir aylık deneme](https://azure.microsoft.com/free/). 
2. Tıklatın **kaynak oluşturma** > **ağ** > **trafik Yöneticisi profili** > **oluşturma**.
4. İçinde **oluşturma trafik Yöneticisi profili**, aşağıdaki gibi tamamlandı:
    1. **Ad** alanında profiliniz için bir ad belirtin. Bu ad trafficmanager.net bölge içinde benzersiz olmalıdır ve sonuçları DNS adıyla <name>, Traffic Manager profilinizin erişmek için kullanılan trafficmanager.net.
    2. **Yönlendirme yöntemi** alanında **Öncelik** yönlendirme yöntemini seçin.
    3. **Abonelik** alanında bu profili hangi abonelik altında oluşturacağınızı seçin
    4. **Kaynak Grubu** alanında bu profilin yerleştirileceği yeni bir kaynak grubu oluşturun.
    5. **Kaynak grubu konumu** alanında kaynak grubunun konumunu seçin. Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılacak Traffic Manager profilini etkilemez.
    6. **Oluştur**’a tıklayın.
    7. Traffic Manager profilinizin genel dağıtımı, tamamlandığında ilgili kaynak grubunda kaynaklardan biri olarak listelenir.

    ![Traffic Manager profili oluşturma](./media/traffic-manager-create-profile/Create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Trafik Yöneticisi uç noktaları ekleme

1. Portal'ın arama çubuğunda arama **trafik Yöneticisi profili** önceki oluşturulan adı bölümünde ve trafik Yöneticisi profili sonuçlarında'ı tıklatın, görüntülenen.
2. İçinde **trafik Yöneticisi profili**, **ayarları** 'yi tıklatın **uç noktaları**.
3. **Ekle**'ye tıklayın.
4. Aşağıdaki gibi tamamlayın:
    1. **Tür** için **Azure uç noktası**’na tıklayın.
    2. Bu uç noktayı tanımak istediğiniz bir **Ad** belirtin.
    3. İçin **hedef kaynak türünün**, tıklatın **uygulama hizmeti**.
    4. İçin **hedef kaynak**, tıklatın **uygulama hizmeti seçin** aynı abonelik altında Web uygulamaları listesi göstermek için. İçinde **kaynak**, ilk uç noktası olarak eklemek istediğiniz uygulama hizmeti seçin.
    5. **Öncelik** için **1** seçin. Bu durum, sağlıksız olması durumunda tüm trafiğin bu uç noktaya gitmesiyle sonuçlanır.
    6. **Devre dışı olarak ekle** seçeneğini işaretsiz bırakın.
    7. **Tamam**’a tıklayın.
5.  Sonraki Azure Web Apps uç noktası için 3 ve 4 numaralı adımları tekrarlayın. Uç noktayı eklerken **Öncelik** değerinin **2** olarak ayarlandığından emin olun.
6.  Her iki uç noktaları eklenmesi tamamlandığında görüntülenir **trafik Yöneticisi profili** izleme durumlarını birlikte **çevrimiçi**.

    ![Trafik Yöneticisi uç noktası ekleme](./media/traffic-manager-create-profile/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Trafik Yöneticisi profili kullan
1.  Portal'ın arama çubuğunda arama **trafik Yöneticisi profili** önceki bölümde oluşturduğunuz adı. Görüntülenen sonuçlarında trafik Yöneticisi profili tıklayın.
2. Tıklatın **genel bakış**.
3. **Trafik Yöneticisi profili** yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) sağ uç noktası olarak yönlendirilen için yönlendirme türü tarafından belirlenir. Bu durumda, tüm istekleri ilk uç noktasına yönlendirilir ve trafik Yöneticisi algılarsa sağlıksız, trafiği otomatik olarak sonraki uç noktasına yöneltilir.

## <a name="delete-the-traffic-manager-profile"></a>Trafik Yöneticisi profilini Sil
Artık gerekli olduğunda, kaynak grubu ve oluşturduğunuz trafik Yöneticisi profilini silin. Bunu yapmak için kaynak grubundan seçin **trafik Yöneticisi profili** tıklatıp **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yönlendirme türleri](traffic-manager-routing-methods.md).
- Uç nokta türleri hakkında daha fazla bilgi [uç nokta türleri](traffic-manager-endpoint-types.md).
- Daha fazla bilgi edinmek [uç nokta izleme](traffic-manager-monitoring.md).



