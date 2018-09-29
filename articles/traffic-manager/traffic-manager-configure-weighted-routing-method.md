---
title: Azure Traffic Manager'ı kullanarak ağırlıklı hepsini bir kez deneme trafik yönlendirme yöntemini yapılandırma | Microsoft Docs
description: Bu makalede, trafik Yöneticisi'nde hepsini bir kez deneme yöntemiyle trafiğin yükünü dengelemenizi açıklanmaktadır
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
ms.openlocfilehash: 6637132481ee33d43ec2b747ba89a56983205ff2
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432452"
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Trafik Yöneticisi'nde ağırlıklı trafik yönlendirme yöntemini yapılandırma

Yaygın trafik yönlendirme yöntemi bulut Hizmetleri ve Web dahil, aynı uç noktaların bir kümesini sağlar ve her bir biçimde hepsini bir kez deneme trafik göndermek için modelidir. Aşağıdaki adımlar, bu tür trafik yönlendirme yöntemini yapılandırma özetlemektedir.

> [!NOTE]
> Azure Web uygulaması zaten hepsini bir kez deneme Yük Dengeleme işlevi (birden çok veri merkezinde içeren) bir Azure bölgesi içinde Web siteleri için sağlar. Traffic Manager, farklı veri merkezlerinde Web siteleri için hepsini bir kez deneme trafik yönlendirme yöntemini belirtmenize olanak sağlar.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>Ağırlıklı trafik yönlendirme yöntemini yapılandırma

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
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
- Hakkında bilgi edinin [coğrafi yönlendirme yöntemini](traffic-manager-configure-geographic-routing-method.md).
- Bilgi edinmek için nasıl [Traffic Manager ayarlarını sınama](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
