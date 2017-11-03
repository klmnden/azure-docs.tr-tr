---
title: "Azure CDN gerçek zamanlı uyarılar | Microsoft Docs"
description: "Microsoft Azure CDN gerçek zamanlı uyarılar. Gerçek zamanlı uyarılar CDN profilinizi uç noktalardan performansını hakkında bildirimler sağlayın."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 51dce1680be5f5f4387c2ba02827195bcdbe9b48
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Microsoft Azure cdn'de gerçek zamanlı uyarılar
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Bu belgede, Microsoft Azure cdn'de gerçek zamanlı uyarılar açıklanmaktadır. Bu işlevselliği performansı CDN profilinize uç noktaları ile ilgili gerçek zamanlı bildirimler sağlar.  E-posta veya temel HTTP uyarılar ayarlayabilirsiniz:

* Bant Genişliği
* Durum kodları
* Önbellek durumları
* Bağlantılar

## <a name="creating-a-real-time-alert"></a>Gerçek zamanlı uyarı oluşturma
1. İçinde [Azure portal](https://portal.azure.com), CDN profilinize gidin.
   
    ![CDN profili](./media/cdn-real-time-alerts/cdn-profile-blade.png)
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili Yönet düğmesi](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
3. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **gerçek zamanlı İstatistikler** çıkma.  Tıklayın **gerçek zamanlı uyarılar**.
   
    ![CDN Yönetim Portalı](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    Var olan uyarı yapılandırmaları (varsa) listesi görüntülenir.
4. Tıklatın **eklemek uyarı** düğmesi.
   
    ![Uyarı düğme ekleme](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Yeni bir uyarı oluşturmak için bir form görüntülenir.
   
    ![Yeni uyarı formu](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. Bu uyarı etkin olmasını istiyorsanız tıkladığınızda **kaydetmek**, kontrol **uyarı etkin** onay kutusu.
6. Uyarınız içinde için açıklayıcı bir ad girin **adı** alan.
7. İçinde **medya türü** açılan listesinde, select **HTTP büyük nesne**.
   
    ![HTTP büyük seçili nesne medya türüyle](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > Seçmelisiniz **HTTP büyük nesne** olarak **medya türü**.  Ve diğer seçenekleri tarafından kullanılmaz **verizon'dan Azure CDN**.  Seçilecek hatası **HTTP büyük nesne** Uyarınız asla tetiklenmez neden olur.
   > 
   > 
8. Oluşturma bir **ifade** seçerek izlemek için bir **ölçüm**, **işleci**, ve **tetikleyen değer**.
   
   * İçin **ölçüm**, izlenen istediğiniz koşulu seçin.  **Bant genişliği Mbps** saniye başına megabit olarak bant genişliği kullanım miktarı.  **Toplam Bağlantı** kenar sunucularımızda eşzamanlı HTTP bağlantı sayısıdır.  Çeşitli önbellek durumları ve durum kodları tanımları için bkz: [Azure CDN önbellek durum kodları](https://msdn.microsoft.com/library/mt759237.aspx) ve [Azure CDN HTTP durum kodları](https://msdn.microsoft.com/library/mt759238.aspx)
   * **İşleç** ölçüm ve tetikleyici değer arasında bir ilişki kurar matematiksel işlecidir.
   * **Tetikleyen değer** bir bildirim gönderilmeden önce karşılanması gereken eşik değeridir.
     
     Aşağıdaki örnekte, 404 durum kodları sayısı 25'den büyük olduğunda bir bildirim gönderilir oluşturulan ifadeyi belirtir.
     
     ![Gerçek zamanlı uyarı örnek bir ifade](./media/cdn-real-time-alerts/cdn-expression.png)
9. İçin **aralığı**, hesaplanan ifadenin ne sıklıkta istediğiniz girin.
10. İçinde **bildirim** açılan listesinde, ne zaman ifade doğruysa bildirilmesini istediğinizi seçin.
    
    * **Koşul başlangıç** belirtilen koşulu ilk algılandığında bir bildirim gönderildiğini gösterir.
    * **Koşul son** belirtilen koşulu artık algılandığında bir bildirim gönderildiğini gösterir. Bu bildirim, yalnızca belirtilen koşulu oluştuğunu bizim ağ sistem izleme saptadıktan sonra tetiklenebilir.
    * **Sürekli** bir bildirim her zaman ağ sistem izleme belirtilen koşulu algılar gönderilen gösterir. Ağ Sistem izleme için belirtilen koşul aralık başına yalnızca bir kez denetler aklınızda bulundurun.
    * **Koşul başlangıç ve bitiş** bir bildirim ilk kez belirtilen koşulu algılanır ve bir kez daha zaman koşul artık algılandığında gönderildiğini gösterir.
1. E-posta ile bildirim almak istiyorsanız, denetleme **e-posta ile bildir** onay kutusu.  
    
    ![E-posta formu tarafından bildir](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    İçinde **için** alanında, istediğiniz yere bildirimleri gönderdiğiniz e-posta adresi girin. İçin **konu** ve **gövde**, varsayılan bırakabilir veya kullanarak iletiyi özelleştirebilir **kullanılabilir anahtar sözcükleri** ileti gönderildiğinde uyarı verileri dinamik olarak eklemek için liste.
    
    > [!NOTE]
    > E-posta bildirimi tıklatarak sınayabilirsiniz **Test bildirimi** uyarı yapılandırma kaydedildikten sonra ancak yalnızca düğme.
    > 
    > 
12. Bir web sunucusuna gönderilen bildirimler istiyorsanız işaretleyin **HTTP Post tarafından bildirim** onay kutusu.
    
    ![HTTP Post formuyla bildir](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    İçinde **Url** alan, HTTP iletisi istediğiniz gönderilen URL'yi girin. İçinde **üstbilgileri** metin kutusuna, istekte gönderilmek üzere HTTP üst bilgilerini girin.  İçin **gövde**, kullanarak iletiyi özelleştirebilir **kullanılabilir anahtar sözcükleri** ileti gönderildiğinde uyarı verileri dinamik olarak eklemek için liste.  **Üstbilgileri** ve **gövde** varsayılan olarak aşağıdaki örneğe benzer bir XML yükü:
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > HTTP Post bildirim tıklatarak sınayabilirsiniz **Test bildirimi** uyarı yapılandırma kaydedildikten sonra ancak yalnızca düğme.
    > 
    > 
13. Tıklatın **kaydetmek** uyarı yapılandırmanızı kaydetmek için düğmesini.  İşaretlendiğinde, **uyarı etkin** 5. adımda Uyarınız şimdi etkindir.

## <a name="next-steps"></a>Sonraki Adımlar
* Analiz [Azure CDN içindeki gerçek zamanlı İstatistikler](cdn-real-time-stats.md)
* Dıg ile daha derin [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
* Analiz [kullanım desenleri](cdn-analyze-usage-patterns.md)

