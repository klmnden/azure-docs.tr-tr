---
title: "Öncelik trafik yönlendirme yöntemini Azure trafik Yöneticisi'ni kullanarak yapılandırma | Microsoft Docs"
description: "Bu makalede, trafik Yöneticisi'nde öncelik trafik yönlendirme yöntemini yapılandırma açıklanmaktadır"
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
ms.openlocfilehash: 0db83cde6facc89b8b8aa72e6419129ec868235c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Trafik Yöneticisi'nde öncelik trafik yönlendirme yöntemini yapılandırma

Web sitesi Modu bağımsız olarak, Azure Web siteleri zaten bir veri merkezi (bölge olarak da bilinir) içindeki Web siteleri için yük devretme işlevsellik sağlar. Trafik Yöneticisi, farklı veri merkezlerinde bulunan Web siteleri için yük devretme sağlar.

Birincil bir hizmete trafiği göndermek ve aynı yedekleme hizmetleri yük devretme kümesi sağlamak için hizmeti yük devretme için genel bir desen var. Aşağıdaki adımlarda bu öncelikli yük devretme Azure bulut hizmetlerine ve Web siteleri ile nasıl yapılandırılacağı açıklanmaktadır:

## <a name="to-configure-the-priority-traffic-routing-method"></a>Öncelik trafik yönlendirme yöntemini yapılandırmak için

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. Portal'ın arama çubuğunda arama **Traffic Manager profillerini** ve ardından yönlendirme yöntemi için yapılandırmak istediğiniz profil adına tıklayın.
3. İçinde **trafik Yöneticisi profili** dikey penceresinde bulut Hizmetleri ve yapılandırmanızda dahil etmek istediğiniz Web siteleri bulunduğunu doğrulayın.
4. İçinde **ayarları** 'yi tıklatın **yapılandırma**hem de **yapılandırma** dikey penceresinde, aşağıdaki gibi tamamlandı:
    1. İçin **yönlendirme yöntemi ayarları trafiği**, trafik yönlendirme yöntemini olduğundan emin olun **öncelik**. Değilse, **öncelik** aşağı açılan listeden.
    2. Ayarlama **uç nokta izleme ayarları** aynı şekilde bu profili içindeki tüm her uç noktası için:
        1. Uygun seçin **Protokolü**ve belirtin **bağlantı noktası** numarası. 
        2. İçin **yolu** eğik yazın  */* . Uç noktaları izlemek için bir yol ve dosya adı belirtmelisiniz. Bir eğik çizgi "/" göreli yolu için geçerli bir giriş ve dosyasının kök dizininde (varsayılan) olduğunu gösterir.
        3. Sayfanın üstündeki **kaydetmek**.
5. İçinde **ayarları** 'yi tıklatın **uç noktaları**.
6. İçinde **uç noktaları** dikey penceresinde, uç noktaları için öncelik sırasını gözden geçirin. Seçtiğinizde, **öncelik** trafik yönlendirme yöntemini, seçili uç noktalarını da sırasını. Uç noktaları öncelik sırasını doğrulayın.  Birincil endpoint üstte olur. Görüntüleniyorsa siparişte denetleyin. tüm istekleri ilk uç noktasına yönlendirilir ve trafik Yöneticisi algılarsa sağlıksız, trafiği otomatik olarak sonraki uç noktasına yöneltilir. 
7. Uç nokta, uç nokta öncelik sırasını değiştirmek için hem de **Endpoint** görüntülenir, dikey tıklayın **Düzenle** değiştirip **öncelik** değeri gerektiği gibi. 
8. Tıklatın **kaydetmek** kaydetme uç noktası ayarlarını değiştirmek için.
9. Yapılandırma değişiklikleri tamamladıktan sonra tıklayın **kaydetmek** sayfanın sonundaki.
10. Değişiklikleri yapılandırmanızda gibi test edin:
    1.  Portal'ın arama çubuğunda aramak için trafik Yöneticisi profil adı ve sonuçları trafik Yöneticisi profiline tıklayın, görüntülenen.
    2.  İçinde **trafik Yöneticisi** profil dikey penceresinde, tıklatın **genel bakış**.
    3.  **Trafik Yöneticisi profili** dikey penceresinde, yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) sağ uç noktası olarak yönlendirilen için yönlendirme türü tarafından belirlenir. Bu durumda, tüm istekleri ilk uç noktasına yönlendirilir ve trafik Yöneticisi algılarsa sağlıksız, trafiği otomatik olarak sonraki uç noktasına yöneltilir.
11. Traffic Manager profilinizin çalışma sonra şirketinizin etki alanı adını Traffic Manager etki alanı adına işaret edecek şekilde yetkili DNS sunucunuzdaki DNS kaydını düzenleyin.

![Trafik Yöneticisi'ni kullanarak öncelik trafik yönlendirme yöntemini yapılandırma][1]

## <a name="next-steps"></a>Sonraki adımlar


- Hakkında bilgi edinin [ağırlıklı trafik yönlendirme yöntemini](traffic-manager-configure-weighted-routing-method.md).
- Hakkında bilgi edinin [performans yönlendirme yöntemini](traffic-manager-configure-performance-routing-method.md).
- Hakkında bilgi edinin [coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md).
- Bilgi edinmek için nasıl [test trafik Yöneticisi Ayarları](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png