---
title: Azure API Yönetimi ile API’nizi dönüştürme ve koruma | Microsoft Docs
description: API’nizi kotalar ve azaltma (hız sınırlama) ilkeleriyle korumayı öğrenin.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: fb56b8489b086b724df9f3c9179f2c3265cd05a7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="transform-and-protect-your-api"></a>API’nizi dönüştürme ve koruma 

Öğreticide, özel bir arka uç bilgisini açığa çıkarmaması için API’nizin nasıl dönüştürüleceği gösterilmektedir. Örneğin, arka uçta çalıştırılan teknoloji yığını hakkındaki bilgileri gizlemek isteyebilirsiniz. API’lerin HTTP yanıt gövdesinde görüntülenen özgün URL’leri gizlemek ve bunları APIM ağ geçidine yeniden yönlendirmek isteyebilirsiniz.

Bu öğreticide size Azure API Yönetimi ile hız sınırı yapılandırarak arka uç API’niz için koruma eklemenin ne kadar kolay olduğu gösterilmektedir. Örneğin, geliştiriciler tarafından aşırı kullanılmaması için API çağrısı sayısını sınırlamak isteyebilirsiniz. Daha fazla bilgi için bkz. [API Yönetimi ilkeleri](api-management-policies.md)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yanıt üst bilgilerini gizlemek için API’yi dönüştürme
> * API yanıt gövdesindeki özgün URL’leri, APIM ağ geçidi URL’leri ile değiştirme
> * Hız sınırı ilkesi ekleyerek (azaltma) bir API’yi koruma
> * Dönüştürmeleri test etme

![İlkeler](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Ön koşullar

+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Yanıt üst bilgilerini gizlemek için API’yi dönüştürme

Bu bölümde, kullanıcılarınıza göstermek istemediğiniz HTTP üst bilgilerinin nasıl gizleneceği gösterilmektedir. Bu örnekte, aşağıdaki üst bilgiler HTTP yanıtında silinir:

* **X-Destekleyen:**
* **X-AspNet-Sürümü**

### <a name="test-the-original-response"></a>Özgün yanıtı test etme

Özgün yanıtı görmek için:

1. **API** sekmesini seçin.
2. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
3. **GetSpeakers** işlemini seçin.
4. Ekranın üst kısmındaki **Test** sekmesine tıklayın.
5. Ekranın alt kısmındaki **Gönder** düğmesine basın. 

    Gördüğünüz gibi özgün yanıt şuna benzer:

    ![İlkeler](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Dönüştürme ilkesi ayarlama

1. APIM örneğinize göz atın.
2. **API** sekmesini seçin.
3. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
4. **Tüm işlemler**’i seçin.
5. Ekranın üst kısmında **Tasarım** sekmesini seçin.
6. **Gelen işlem** penceresinde üçgene (kalemin yanındaki) tıklayın.
7. **Kod düzenleyicisi**’ni seçin.
    
     ![İlkeyi düzenleme](./media/set-edit-policies/set-edit-policies01.png)
9. İmleci **&lt;giden&gt;** öğesinin içine konumlandırın.
10. Sağ pencerede **Dönüştürme ilkeleri** bölümünde **+ HTTP üst bilgisini ayarla** seçeneğine iki defa tıklayın (iki ilke kod parçacığı eklemek için).

    ![İlkeler](./media/transform-api/transform-api.png)
11. **<outbound>** kodunuzu aşağıdaki gibi görünecek şekilde değiştirin:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>API yanıt gövdesindeki özgün URL’leri, APIM ağ geçidi URL’leri ile değiştirme

Bu bölümde, API’lerin HTTP yanıt gövdesinde görüntülenen özgün URL’lerin nasıl gizleneceği ve bunların APIM ağ geçidine nasıl yeniden yönlendirileceği gösterilmektedir.

### <a name="test-the-original-response"></a>Özgün yanıtı test etme

Özgün yanıtı görmek için:

1. **API** sekmesini seçin.
2. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
3. **GetSpeakers** işlemini seçin.
4. Ekranın üst kısmındaki **Test** sekmesine tıklayın.
5. Ekranın alt kısmındaki **Gönder** düğmesine basın. 

    Gördüğünüz gibi özgün yanıt şuna benzer:

    ![İlkeler](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Dönüştürme ilkesi ayarlama

1. APIM örneğinize göz atın.
2. **API** sekmesini seçin.
3. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
4. **Tüm işlemler**’i seçin.
5. Ekranın üst kısmında **Tasarım** sekmesini seçin.
6. **Gelen işlem** penceresinde üçgene (kalemin yanındaki) tıklayın.
7. **Kod düzenleyicisi**’ni seçin.
8. İmleci **&lt;giden&gt;** öğesinin içine konumlandırın.
9. Sağ pencerede **Dönüştürme ilkeleri** bölümünde **+ Gövdedeki dizeyi bul ve değiştir** seçeneğine tıklayın.
10. URL’yi API ağ geçidinizle eşleşecek şekilde değiştirmek için **<find-and-replace** kodunuzu (**<outbound>** öğesinde) değiştirin. Örnek:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Hız sınırı ilkesi ekleyerek (azaltma) bir API’yi koruma

Bu bölümde, hız sınırları yapılandırılarak arka uç API’niz için nasıl koruma ekleneceği gösterilmektedir. Örneğin, geliştiriciler tarafından aşırı kullanılmaması için API çağrısı sayısını sınırlamak isteyebilirsiniz. Bu örnekte, her bir abonelik kimliği için sınır 15 saniyede 3 çağrıya ayarlanmıştır. 15 saniye sonra geliştirici, API’yi çağırmayı yeniden deneyebilir.

1. APIM örneğinize göz atın.
2. **API** sekmesini seçin.
3. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
4. **Tüm işlemler**’i seçin.
5. Ekranın üst kısmında **Tasarım** sekmesini seçin.
6. **Gelen işlem** penceresinde (kalemin yanındaki) üçgene tıklayın.
7. **Kod düzenleyicisi**’ni seçin.
8. İmleci **&lt;gelen&gt;** öğesinin içine konumlandırın.
9. Sağ pencerede **Erişim kısıtlama ilkeleri** bölümünde **+ Anahtar başına çağrıyı sınırla** seçeneğine tıklayın.
10. **<rate-limit-by-key** kodunuzu (**<inbound>** öğesinde) aşağıdaki kodla değiştirin:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Dönüştürmeleri test etme
        
Bu noktada ilkelerinizin kodu şöyle görünür:

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

Bu bölümün geri kalanında, bu makalede ayarladığınız ilke dönüştürmeleri test edilmektedir.

### <a name="test-the-stripped-response-headers"></a>Gizlenmiş yanıt üst bilgilerini test etme

1. APIM örneğinize göz atın.
2. **API** sekmesini seçin.
3. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
4. **GetSpeakers** işlemine tıklayın.
5. **Test** sekmesini seçin.
6. **Gönder**’e basın.

    Gördüğünüz gibi üst bilgiler gizlenmiştir:

    ![İlkeler](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Değiştirilen URL’yi test etme

1. APIM örneğinize göz atın.
2. **API** sekmesini seçin.
3. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
4. **GetSpeakers** işlemine tıklayın.
5. **Test** sekmesini seçin.
6. **Gönder**’e basın.

    Gördüğünüz gibi URL değiştirilmiştir.

    ![İlkeler](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Hız sınırını test etme (azaltma)

1. APIM örneğinize göz atın.
2. **API** sekmesini seçin.
3. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
4. **GetSpeakers** işlemine tıklayın.
5. **Test** sekmesini seçin.
6. Art arda üç defa **Gönder** düğmesine basın.

    İsteği 3 defa gönderdikten sonra **429 Çok sayıda istek var** yanıtını alırsınız.
7. 15 saniye kadar bekleyin ve tekrar **Gönder** düğmesine basın. Bu defa **200 OK** yanıtını almanız gerekir.

    ![Azaltma](./media/transform-api/test-throttling.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yanıt üst bilgilerini gizlemek için API’yi dönüştürme
> * API yanıt gövdesindeki özgün URL’leri, APIM ağ geçidi URL’leri ile değiştirme
> * Hız sınırı ilkesi ekleyerek (azaltma) bir API’yi koruma
> * Dönüştürmeleri test etme

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [API’nizi izleme](api-management-howto-use-azure-monitor.md)
