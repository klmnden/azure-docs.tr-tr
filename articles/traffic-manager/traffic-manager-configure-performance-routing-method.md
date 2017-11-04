---
title: "Azure trafik Yöneticisi'ni kullanarak performans trafik yönlendirme yöntemini yapılandırma | Microsoft Docs"
description: "Bu makalede trafik Yöneticisi uç noktası trafiğini yönlendirmek için en düşük gecikme süresi ile nasıl yapılandırılacağını açıklar"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 014aa646459cd64fca7c697419324caa3edaeeea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-the-performance-traffic-routing-method"></a>Performans trafik yönlendirme yöntemini yapılandırma

Performans trafik yönlendirme yöntemini, uç nokta için en düşük gecikme süresine sahip istemcinin ağdan doğrudan trafiğe olanak sağlar. Genellikle, en düşük gecikme ile veri merkezi en yakın coğrafi uzaklığı ifade eder. Bu trafik yönlendirme yöntemini ağ yapılandırmasında gerçek zamanlı değişiklikleri hesaba veya yük.

##  <a name="to-configure-performance-routing-method"></a>Performans yönlendirme yöntemini yapılandırmak için

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portal'ın arama çubuğunda arama **Traffic Manager profillerini** ve ardından yönlendirme yöntemi için yapılandırmak istediğiniz profil adına tıklayın.
3. İçinde **trafik Yöneticisi profili** dikey penceresinde bulut Hizmetleri ve yapılandırmanızda dahil etmek istediğiniz Web siteleri bulunduğunu doğrulayın.
4. İçinde **ayarları** 'yi tıklatın **yapılandırma**hem de **yapılandırma** dikey penceresinde, aşağıdaki gibi tamamlandı:
    1. İçin **yönlendirme yöntemi ayarları trafiği**, için **yönlendirme yöntemi** seçin **performans**.
    2. Ayarlama **uç nokta izleme ayarları** aynı şekilde bu profili içindeki tüm her uç noktası için:
        1. Uygun seçin **Protokolü**ve belirtin **bağlantı noktası** numarası. 
        2. İçin **yolu** eğik yazın  */* . Uç noktaları izlemek için bir yol ve dosya adı belirtmelisiniz. Bir eğik çizgi "/" göreli yolu için geçerli bir giriş ve dosyasının kök dizininde (varsayılan) olduğunu gösterir.
        3. Sayfanın üstündeki **kaydetmek**.
5.  Değişiklikleri yapılandırmanızda gibi test edin:
    1.  Portal'ın arama çubuğunda aramak için trafik Yöneticisi profil adı ve sonuçları trafik Yöneticisi profiline tıklayın, görüntülenen.
    2.  İçinde **trafik Yöneticisi** profil dikey penceresinde, tıklatın **genel bakış**.
    3.  **Trafik Yöneticisi profili** dikey penceresinde, yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) sağ uç noktası olarak yönlendirilen için yönlendirme türü tarafından belirlenir. Bu durumda tüm istekleri uç nokta için en düşük gecikme süresine sahip istemcinin ağ üzerinden yönlendirilir.
6. Traffic Manager profilinizin çalışma sonra şirketinizin etki alanı adını Traffic Manager etki alanı adına işaret edecek şekilde yetkili DNS sunucunuzdaki DNS kaydını düzenleyin.

![Trafik Yöneticisi'ni kullanarak performans trafik yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [ağırlıklı trafik yönlendirme yöntemini](traffic-manager-configure-weighted-routing-method.md).
- Hakkında bilgi edinin [öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md).
- Hakkında bilgi edinin [coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md).
- Bilgi edinmek için nasıl [test trafik Yöneticisi Ayarları](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png