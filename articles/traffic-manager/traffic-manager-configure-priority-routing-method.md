---
title: Azure Traffic Manager'ı kullanarak öncelikli trafik yönlendirme yöntemini yapılandırma | Microsoft Docs
description: Bu makalede, trafik Yöneticisi'nde öncelik trafik yönlendirme yöntemini yapılandırma açıklanmaktadır
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
ms.openlocfilehash: 66c5bd9390d6fe0f26af66e18aed22c07a7da3e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884010"
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Trafik Yöneticisi'nde öncelikli trafik yönlendirme yöntemini yapılandırma

Web sitesi modu ne olursa olsun, Azure Web siteleri, zaten bir veri merkezi (bölge olarak da bilinir) içinde Web siteleri için yük devretme işlevsellik sağlar. Traffic Manager yük devretme için farklı veri merkezlerindeki Web sitesi sunar.

Birincil bir hizmete trafiği göndermek ve aynı yedekleme hizmetler için yük devretme kümesi sağlamak için hizmeti yük devretme için yaygın bir düzen olduğunu. Aşağıdaki adımlarda, Azure bulut Hizmetleri ve Web siteleri ile bu öncelikli yük devri yapılandırmak açıklanmaktadır:

## <a name="to-configure-the-priority-traffic-routing-method"></a>Öncelik trafik yönlendirme yöntemini yapılandırma

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portalın arama çubuğunda arama **Traffic Manager profillerini** ve yönlendirme yöntemi için yapılandırmak istediğiniz profil adına tıklayın.
3. İçinde **Traffic Manager profili** dikey penceresinde bulut Hizmetleri ve yapılandırmanızda dahil etmek istediğiniz Web siteleri mevcut olduğunu doğrulayın.
4. İçinde **ayarları** bölümünde **yapılandırma**hem de **yapılandırma** dikey penceresinde, aşağıdaki gibi doldurun:
    1. İçin **trafik yönlendirme yöntemi ayarları**, trafik yönlendirme yöntemini olduğundan emin olun **öncelik**. Yüklü değilse, **öncelik** aşağı açılan listeden.
    2. Ayarlama **uç nokta İzleyicisi ayarları** aynı şekilde bu profili içindeki tüm her bir uç noktası için:
        1. Uygun seçin **Protokolü**, belirtin **bağlantı noktası** sayı. 
        2. İçin **yolu** eğik çizgi yazın */*. Uç noktaları izlemek için bir yol ve dosya adı belirtmeniz gerekir. Bir eğik çizgi "/" göreli yolu için geçerli bir giriştir ve dosyasının kök dizininde (varsayılan) olduğunu gösterir.
        3. Sayfanın üst kısmında tıklayın **Kaydet**.
5. İçinde **ayarları** bölümünde **uç noktaları**.
6. İçinde **uç noktaları** dikey penceresinde, uç noktalarınız için öncelik sırası gözden geçirin. Seçtiğinizde, **öncelik** trafik yönlendirme yöntemini, seçili uç noktalardan önemli olan konuya sırası. Uç noktaları öncelik sırasını doğrulayın.  Üst birincil uç noktadır. Görüntülenme sırasını denetleyin. tüm istekler, ilk uç noktaya yönlendirilir ve iyi durumda olmayan, Traffic Manager algılarsa, trafiği otomatik olarak sonraki uç noktaya devreder. 
7. Uç nokta öncelik sırasını değiştirmek için uç noktaya tıklayın ve **uç nokta** görüntülenir, dikey **Düzenle** değiştirip **öncelik** değer gerektiğinde. 
8. Tıklayın **Kaydet** kaydetme uç nokta ayarlarını değiştirmek için.
9. Yapılandırma Değişikliklerinizi tamamladıktan sonra tıklayın **Kaydet** sayfanın alt kısmındaki.
10. Değişiklikleri yapılandırmanızda şu şekilde test edin:
    1.  Portalın arama çubuğunda, Traffic Manager profil adı için arama yapın ve sonuçları Traffic Manager profiline tıklayın, görüntülenen.
    2.  İçinde **Traffic Manager** profil dikey penceresinde, tıklayın **genel bakış**.
    3.  **Traffic Manager profili** dikey penceresinde, yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) yönlendirme türüne göre belirlenen doğru uç noktaya yönlendirilir. Bu durumda, tüm istekleri ilk uç noktaya yönlendirilir ve iyi durumda olmayan, Traffic Manager algılarsa, trafiği otomatik olarak sonraki uç noktaya devreder.
11. Traffic Manager profilinizin çalışmaya başladığında, şirketinizin etki alanı adını Traffic Manager etki alanı adına yönlendirme için yetkili DNS sunucunuzdaki DNS kaydı düzenleyin.

![Traffic Manager'ı kullanarak öncelikli trafik yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar


- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- Hakkında bilgi edinin [performans yönlendirme yöntemini](traffic-manager-configure-performance-routing-method.md).
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
- Bilgi edinmek için nasıl [Traffic Manager ayarlarını sınama](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png