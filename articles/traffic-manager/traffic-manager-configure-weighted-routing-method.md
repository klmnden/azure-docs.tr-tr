---
title: Azure Traffic Manager'ı kullanarak ağırlıklı hepsini bir kez deneme trafik yönlendirme yöntemini yapılandırma | Microsoft Docs
description: Bu makalede, trafik Yöneticisi'nde hepsini bir kez deneme yöntemiyle trafiğin yükünü dengelemenizi açıklanmaktadır
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: ef39c09d4fc411937fdd6f4b5b5aec491efd0c5f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62113296"
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Trafik Yöneticisi'nde ağırlıklı trafik yönlendirme yöntemini yapılandırma

Bir ortak trafik yönlendirme yöntemi bulut Hizmetleri ve Web dahil, aynı uç noktaların bir kümesini sağlar ve trafiği her birine eşit gönderme modelidir. Aşağıdaki adımlar, bu tür trafik yönlendirme yöntemini yapılandırma özetlemektedir.

> [!NOTE]
> Azure Web uygulaması zaten hepsini bir kez deneme Yük Dengeleme işlevi (Bu, birden çok veri merkezinde oluşturan) bir Azure bölgesi içinde Web siteleri için sağlar. Traffic Manager, farklı veri merkezlerindeki Web sitesi üzerinden trafiği dağıtmak sağlar.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>Ağırlıklı trafik yönlendirme yöntemini yapılandırma

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portalın arama çubuğunda arama **Traffic Manager profillerini** ve yönlendirme yöntemi için yapılandırmak istediğiniz profil adına tıklayın.
3. İçinde **Traffic Manager profili** dikey penceresinde bulut Hizmetleri ve yapılandırmanızda dahil etmek istediğiniz Web siteleri mevcut olduğunu doğrulayın.
4. İçinde **ayarları** bölümünde **yapılandırma**hem de **yapılandırma** dikey penceresinde, aşağıdaki gibi doldurun:
    1. İçin **trafik yönlendirme yöntemi ayarları**, trafik yönlendirme yöntemini olduğundan emin olun **ağırlıklı**. Yüklü değilse, **ağırlıklı** aşağı açılan listeden.
    2. Ayarlama **uç nokta İzleyicisi ayarları** aynı şekilde bu profili içindeki tüm her bir uç noktası için:
        1. Uygun seçin **Protokolü**, belirtin **bağlantı noktası** sayı. 
        2. İçin **yolu** eğik çizgi yazın */*. Uç noktaları izlemek için bir yol ve dosya adı belirtmeniz gerekir. Bir eğik çizgi "/" göreli yolu için geçerli bir giriştir ve dosyasının kök dizininde (varsayılan) olduğunu gösterir.
        3. Sayfanın üst kısmında tıklayın **Kaydet**.
5. Değişiklikleri yapılandırmanızda şu şekilde test edin:
    1.  Portalın arama çubuğunda, Traffic Manager profil adı için arama yapın ve sonuçları Traffic Manager profiline tıklayın, görüntülenen.
    2.  İçinde **Traffic Manager** profil dikey penceresinde, tıklayın **genel bakış**.
    3.  **Traffic Manager profili** dikey penceresinde, yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) yönlendirme türüne göre belirlenen doğru uç noktaya yönlendirilir. Bu durumda tüm istekler yönlendirilir hepsini bir biçimde her bir uç nokta.
6. Traffic Manager profilinizin çalışmaya başladığında, şirketinizin etki alanı adını Traffic Manager etki alanı adına yönlendirme için yetkili DNS sunucunuzdaki DNS kaydı düzenleyin.

![Traffic Manager'ı kullanarak ağırlıklı trafik yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [öncelik trafik yönlendirme yöntemini](traffic-manager-configure-priority-routing-method.md).
- Hakkında bilgi edinin [performans trafiği yönlendirme yöntemini](traffic-manager-configure-performance-routing-method.md).
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
- Bilgi edinmek için nasıl [Traffic Manager ayarlarını sınama](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
