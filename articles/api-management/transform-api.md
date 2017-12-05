---
title: "Dönüştürme ve API'nizi Azure API Management ile koruma | Microsoft Docs"
description: "API’nizi kotalar ve azaltma (hız sınırlama) ilkeleriyle korumayı öğrenin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 772f3828d85c54e7b8bb44c857e555175b7444cc
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="transform-and-protect-your-api"></a>Dönüştürme ve API'nizi koruma 

Öğretici, özel bir arka uç bilgileri açığa çıkarmadığınızdan şekilde API'nizi dönüştürme gösterilmektedir. Örneğin, arka uçta çalıştıran teknoloji yığınını hakkında bilgi gizlemek isteyebilirsiniz. API'nin HTTP yanıt gövdesinde yer ve bunun yerine APIM ağ geçidine yeniden yönlendirme özgün URL'leri gizlemek isteyebilirsiniz.

Bu öğreticinin Ayrıca, Azure API Management ile hız sınırı yapılandırarak arka uç API'niz için koruma eklemek için ne kadar kolay olduğunu gösterir. Örneğin, API, geliştiriciler tarafından aşırı kullanılmasına olmayan biçimde adlandırılır çağrıları sayısını sınırlamak isteyebilirsiniz. Daha fazla bilgi için bkz: [API Management ilkeleri](api-management-policies.md)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yanıt Üstbilgileri çıkarabilmesi bir API dönüştürme
> * API yanıt gövdesine özgün URL'leri APIM ağ geçidi URL'ler ile değiştir
> * Hız sınırı İlkesi (azaltma) ekleyerek bir API koruma
> * Dönüştürmeleri sınamak

![İlkeler](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Ön koşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, aşağıdaki öğreticiye: [alma ve ilk API'nizi yayımlama](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Yanıt Üstbilgileri çıkarabilmesi bir API dönüştürme

Bu bölümde, Göster kullanıcılarınıza istemediğiniz HTTP üstbilgilerini gizleme gösterilmektedir. Bu örnekte, aşağıdaki üst bilgiler HTTP yanıt olarak silindi:

* **X-gücü-tarafından**
* **X AspNet sürümü**

### <a name="test-the-original-response"></a>Özgün yanıt sınama

Özgün yanıtı görmek için:

1. Seçin **API** sekmesi.
2. Tıklatın **Demo konferans API** API listenizden.
3. Seçin **GetSpeakers** işlemi.
4. Tıklatın **Test** ekranın üst kısmındaki sekme.
5. Tuşuna **Gönder** ekranın altındaki düğmesi. 

    Gördüğünüz gibi özgün yanıtı şuna benzer:

    ![İlkeler](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Dönüştürme ilkesi ayarlama

1. APIM örneğine göz atın.
2. Seçin **API** sekmesi.
3. Tıklatın **Demo konferans API** API listenizden.
4. Seçin **tüm işlemleri**.
5. Ekranın üst kısmında seçin **tasarım** sekmesi.
6. İçinde **giden işlem** penceresinde üçgen (yanındaki kalem) tıklayın.
7. Seçin **Kod düzenleyicisinde**.
    
     ![İlkeyi düzenle](./media/set-edit-policies/set-edit-policies01.png)
9. İç imleç Konumlandır  **<outbound>**  öğesi.
10. Sağ penceresinde altında **dönüştürme ilkelerini**, tıklatın **+ kümesi HTTP üstbilgisi** iki kez (iki ilke parçacıkları eklemek için).

    ![İlkeler](./media/transform-api/transform-api.png)
11. Değiştirme,  **<outbound>**  kod şöyle görünür:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>API yanıt gövdesine özgün URL'leri APIM ağ geçidi URL'ler ile değiştir

Bu bölümde, API'nin HTTP yanıt gövdesinde yer ve bunun yerine APIM ağ geçidine yeniden yönlendirme özgün URL'leri gizleme gösterilmektedir.

### <a name="test-the-original-response"></a>Özgün yanıt sınama

Özgün yanıtı görmek için:

1. Seçin **API** sekmesi.
2. Tıklatın **Demo konferans API** API listenizden.
3. Seçin **GetSpeakers** işlemi.
4. Tıklatın **Test** ekranın üst kısmındaki sekme.
5. Tuşuna **Gönder** ekranın altındaki düğmesi. 

    Gördüğünüz gibi özgün yanıtı şuna benzer:

    ![İlkeler](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Dönüştürme ilkesi ayarlama

1. APIM örneğine göz atın.
2. Seçin **API** sekmesi.
3. Tıklatın **Demo konferans API** API listenizden.
4. Seçin **tüm işlemleri**.
5. Ekranın üst kısmında seçin **tasarım** sekmesi.
6. İçinde **giden işlem** penceresinde üçgen (yanındaki kalem) tıklayın.
7. Seçin **Kod düzenleyicisinde**.
8. İç imleç Konumlandır  **<outbound>**  öğesi.
9. Sağ penceresinde altında **dönüştürme ilkelerini**, tıklatın **+ bulun ve gövde dizesinde**.
10. Değiştirme, **< Bul ve Değiştir** kod (içinde  **<outbound>**  öğesi) APIM ağ geçidiniz eşleşecek şekilde URL'sini değiştirmek için. Örneğin:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Hız sınırı İlkesi (azaltma) ekleyerek bir API koruma

Bu bölümde, oran sınırları yapılandırarak arka uç API'niz için koruma ekleyin gösterilmektedir. Örneğin, API, geliştiriciler tarafından aşırı kullanılmasına olmayan biçimde adlandırılır çağrıları sayısını sınırlamak isteyebilirsiniz. Bu örnekte, her abonelik kimliği için 15 saniye başına 3 çağrı sınırı ayarlanır 15 saniye sonra API'yi çağıran bir geliştirici yeniden deneyebilirsiniz.

1. APIM örneğine göz atın.
2. Seçin **API** sekmesi.
3. Tıklatın **Demo konferans API** API listenizden.
4. Seçin **tüm işlemleri**.
5. Ekranın üst kısmında seçin **tasarım** sekmesi.
6. İçinde **gelen işleme** penceresinde üçgen (yanındaki kalem) tıklayın.
7. Seçin **Kod düzenleyicisinde**.
8. İç imleç Konumlandır  **<inbound>**  öğesi.
9. Sağ penceresinde altında **erişim kısıtlama ilkeleri**, tıklatın **+ anahtar başına çağrı hızını sınırla**.
10. Değiştirme, **< anahtar tarafından hızı sınırı** kod (içinde  **<inbound>**  öğesi) aşağıdaki kodu için:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Dönüştürmeleri sınamak
        
Bu noktada, bu kod görülüyor ilkeler:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

Bu bölümün geri kalanında bu makalede ayarladığınız İlkesi dönüşümleri sınar.

### <a name="test-the-stripped-response-headers"></a>Kırpılmış yanıt üstbilgileri test

1. APIM örneğine göz atın.
2. Seçin **API** sekmesi.
3. Tıklatın **Demo konferans API** API listenizden.
4. Tıklatın **GetSpeakers** işlemi.
5. Seçin **Test** sekmesi.
6. Tuşuna **Gönder**.

    Üstbilgileri atılmış görebileceğiniz gibi:

    ![İlkeler](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Değiştirilen URL test etme

1. APIM örneğine göz atın.
2. Seçin **API** sekmesi.
3. Tıklatın **Demo konferans API** API listenizden.
4. Tıklatın **GetSpeakers** işlemi.
5. Seçin **Test** sekmesi.
6. Tuşuna **Gönder**.

    Gördüğünüz gibi URL değiştirilmiştir.

    ![İlkeler](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>(Azaltma) Hız sınırını test etme

1. APIM örneğine göz atın.
2. Seçin **API** sekmesi.
3. Tıklatın **Demo konferans API** API listenizden.
4. Tıklatın **GetSpeakers** işlemi.
5. Seçin **Test** sekmesi.
6. Tuşuna **Gönder** üç kez.

    3 kez İsteği gönderdikten sonra get **429 çok fazla istek** yanıt.
7. 15 saniye veya bunu bekleyin ve basın **Gönder** yeniden. Almanız bu süre bir **200 Tamam** yanıt.

    ![Azaltma](./media/transform-api/test-throttling.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yanıt Üstbilgileri çıkarabilmesi bir API dönüştürme
> * API yanıt gövdesine özgün URL'leri APIM ağ geçidi URL'ler ile değiştir
> * Hız sınırı İlkesi (azaltma) ekleyerek bir API koruma
> * Dönüştürmeleri sınamak

Sonraki öğretici ilerleyin:

> [!div class="nextstepaction"]
> [API'nizi izleme](api-management-howto-use-azure-monitor.md)
