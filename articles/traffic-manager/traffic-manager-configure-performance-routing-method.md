---
title: Azure Traffic Manager'ı kullanarak performans trafiği yönlendirme yöntemini yapılandırma | Microsoft Docs
description: Bu makalede Traffic Manager uç nokta trafiği için en düşük gecikme süresiyle nasıl yapılandırılacağını açıklar.
services: traffic-manager
manager: twooley
documentationcenter: ''
author: asudbring
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: allensu
ms.openlocfilehash: 5e9b02a4145d86b86ea3ba0d509d06b7c148cc6d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048469"
---
# <a name="configure-the-performance-traffic-routing-method"></a>Performans trafiği yönlendirme yöntemini yapılandırma

Performans trafiği yönlendirme metodunu uç noktaya en düşük gecikme süresiyle istemcinin ağdan doğrudan trafiğe olanak sağlar. Genellikle, en düşük gecikme süresine sahip bir veri merkezinde en yakın coğrafi uzaklığı'dır. Bu trafik yönlendirme yöntemini, gerçek zamanlı değişiklikler ağ yapılandırması için hesap veya yük.

##  <a name="to-configure-performance-routing-method"></a>Performans yönlendirme yöntemini yapılandırma

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portalın arama çubuğunda arama **Traffic Manager profillerini** ve yönlendirme yöntemi için yapılandırmak istediğiniz profil adına tıklayın.
3. İçinde **Traffic Manager profili** dikey penceresinde bulut Hizmetleri ve yapılandırmanızda dahil etmek istediğiniz Web siteleri mevcut olduğunu doğrulayın.
4. İçinde **ayarları** bölümünde **yapılandırma**hem de **yapılandırma** dikey penceresinde, aşağıdaki gibi doldurun:
    1. İçin **trafik yönlendirme yöntemi ayarları**, için **yönlendirme yöntemi** seçin **performans**.
    2. Ayarlama **uç nokta İzleyicisi ayarları** aynı şekilde bu profili içindeki tüm her bir uç noktası için:
        1. Uygun seçin **Protokolü**, belirtin **bağlantı noktası** sayı. 
        2. İçin **yolu** eğik çizgi yazın */* . Uç noktaları izlemek için bir yol ve dosya adı belirtmeniz gerekir. Bir eğik çizgi "/" göreli yolu için geçerli bir giriştir ve dosyasının kök dizininde (varsayılan) olduğunu gösterir.
        3. Sayfanın üst kısmında tıklayın **Kaydet**.
5.  Değişiklikleri yapılandırmanızda şu şekilde test edin:
    1.  Portalın arama çubuğunda, Traffic Manager profil adı için arama yapın ve sonuçları Traffic Manager profiline tıklayın, görüntülenen.
    2.  İçinde **Traffic Manager** profil dikey penceresinde, tıklayın **genel bakış**.
    3.  **Traffic Manager profili** dikey penceresinde, yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) yönlendirme türüne göre belirlenen doğru uç noktaya yönlendirilir. Bu durumda tüm istekler uç noktaya en düşük gecikme süresiyle istemcinin ağ üzerinden yönlendirilir.
6. Traffic Manager profilinizin çalışmaya başladığında, şirketinizin etki alanı adını Traffic Manager etki alanı adına yönlendirme için yetkili DNS sunucunuzdaki DNS kaydı düzenleyin.

![Traffic Manager'ı kullanarak performans trafiği yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar

- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- [Öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md) hakkında bilgi edinin.
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
- Bilgi edinmek için nasıl [Traffic Manager ayarlarını sınama](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png