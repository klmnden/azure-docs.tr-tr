---
title: Azure trafik Yöneticisi'ni kullanarak ağırlıklı hepsini bir kez deneme trafik yönlendirme yöntemini yapılandırma | Microsoft Docs
description: Bu makalede, trafik Yöneticisi'nde bir hepsini bir kez deneme yöntemiyle Bakiye trafiği Yük Dengelemesi açıklanmaktadır
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 7aa4c9120d44ff1b3e59a57090ea04e3f8021fc4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23876547"
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Trafik Yöneticisi'nde ağırlıklı trafik yönlendirme yöntemini yapılandırma

Bulut Hizmetleri ve Web siteleri içeren aynı uç noktaların kümesi sağlar ve her bir hepsini şekilde trafiği göndermek için genel bir trafik yönlendirme yöntemini desen var. Aşağıdaki adımlar bu tür trafik yönlendirme yöntemini yapılandırma konusunda verilmiştir.

> [!NOTE]
> Azure Web siteleri zaten hepsini bir kez deneme yük dengeleme veri merkezi (bölge olarak da bilinir) içindeki Web siteleri için işlevsellik sağlar. Trafik Yöneticisi, Web siteleri için hepsini bir kez deneme trafik yönlendirme yöntemini farklı veri merkezlerinde belirtmenizi sağlar.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>Ağırlıklı trafik yönlendirme yöntemini yapılandırmak için

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portal'ın arama çubuğunda arama **Traffic Manager profillerini** ve ardından yönlendirme yöntemi için yapılandırmak istediğiniz profil adına tıklayın.
3. İçinde **trafik Yöneticisi profili** dikey penceresinde bulut Hizmetleri ve yapılandırmanızda dahil etmek istediğiniz Web siteleri bulunduğunu doğrulayın.
4. İçinde **ayarları** 'yi tıklatın **yapılandırma**hem de **yapılandırma** dikey penceresinde, aşağıdaki gibi tamamlandı:
    1. İçin **yönlendirme yöntemi ayarları trafiği**, trafik yönlendirme yöntemini olduğundan emin olun **Weighted**. Değilse, **Weighted** aşağı açılan listeden.
    2. Ayarlama **uç nokta izleme ayarları** aynı şekilde bu profili içindeki tüm her uç noktası için:
        1. Uygun seçin **Protokolü**ve belirtin **bağlantı noktası** numarası. 
        2. İçin **yolu** eğik yazın  */* . Uç noktaları izlemek için bir yol ve dosya adı belirtmelisiniz. Bir eğik çizgi "/" göreli yolu için geçerli bir giriş ve dosyasının kök dizininde (varsayılan) olduğunu gösterir.
        3. Sayfanın üstündeki **kaydetmek**.
5. Değişiklikleri yapılandırmanızda gibi test edin:
    1.  Portal'ın arama çubuğunda aramak için trafik Yöneticisi profil adı ve sonuçları trafik Yöneticisi profiline tıklayın, görüntülenen.
    2.  İçinde **trafik Yöneticisi** profil dikey penceresinde, tıklatın **genel bakış**.
    3.  **Trafik Yöneticisi profili** dikey penceresinde, yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) sağ uç noktası olarak yönlendirilen için yönlendirme türü tarafından belirlenir. Bu durumda tüm istekleri yönlendirilir hepsini şekilde her bir uç nokta.
6. Traffic Manager profilinizin çalışma sonra şirketinizin etki alanı adını Traffic Manager etki alanı adına işaret edecek şekilde yetkili DNS sunucunuzdaki DNS kaydını düzenleyin.

![Trafik Yöneticisi'ni kullanarak ağırlıklı trafik yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [öncelik trafik yönlendirme yöntemini](traffic-manager-configure-priority-routing-method.md).
- Hakkında bilgi edinin [performans trafik yönlendirme yöntemini](traffic-manager-configure-performance-routing-method.md).
- Hakkında bilgi edinin [coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md).
- Bilgi edinmek için nasıl [test trafik Yöneticisi Ayarları](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
