---
title: "Azure portalını kullanarak el ile bir API ekleme | Microsoft Docs"
description: "Bu öğreticide bir API el ile eklemek için API Management (APIM) kullanmayı gösterir."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 9426839f88daece1bb688a2079b7854ccaebdc57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="add-an-api-manually"></a>Bir API el ile Ekle 

Bu makaledeki adımları Azure portalında bir API API Management (APIM) örneğine el ile eklemek için nasıl kullanılacağını gösterir. API mock istediğinizde boş bir API oluşturmak ve el ile tanımlamak istersiniz, yaygın bir senaryo sağlar. Bir API mocking hakkında daha fazla bilgi için bkz [Mock API yanıtları](mock-api-responses.md).

Varolan bir API almak istiyorsanız, bkz: [ilgili konular](#related-topics) bölümü.

Bu makalede, biz boş bir API oluşturun ve belirtin [httpbin.org](http://httpbin.org) (bir ortak test etme hizmeti) bir arka uç API'si olarak.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-an-api"></a>Bir API oluşturma

1. Seçin **API'leri** gelen altında **API MANAGEMENT**.
2. Sol menüden seçin **+ API ekleme**.
3. Seçin **boş API** listeden.

    ![Boş API](media/add-api-manually/blank-api.png)
4. API ayarlarını girin.

    ![Ayarlar](media/add-api-manually/settings.png)

    |**Ad**|**Değer**|**Açıklama**|
    |---|---|---|
    |**Görünen ad**|"*Boş API*" |Bu ad, Geliştirici Portalı'nda görüntülenir.|
    |**Web hizmeti URL'si** (isteğe bağlı)| "*http://httpbin.org*"| Bir API mock istiyorsanız, yapmamanız. <br/>Bu durumda, biz girin [http://httpbin.org](http://httpbin.org). Bu bir genel test hizmetidir. <br/>Bir arka uç için otomatik olarak eşlenmiş bir API almak istiyorsanız, içinde konulardan birine bakın [ilgili konular](#related-topics) bölümü.|
    |**URL şeması**|"*HTTPS*"|Bu durumda, arka uç güvenli olmayan HTTP erişimi olmasına rağmen biz arka uç güvenli bir HTTPS APIM erişim belirtin. <br/>Bu tür bir senaryo (HTTPS HTTP), HTTPS sonlandırma adı verilir. API'nizi (burada HTTPS kullanılmıyor olsa bile erişim güvenli olduğunu bildiğiniz) bir sanal ağ içindeki varsa bunu yapmanız. <br/>"HTTPS sonlandırma" kullanmak isteyebilirsiniz bazı CPU döngülerini kaydetmek için.|
    |**URL soneki**|"*hbin*"| Bu APIM örnekte belirli bu API tanımlayan bir ad sonekidir. Bu APIM örneğinde benzersiz olması gerekir.|
    |**Ürünler**|"*Sınırsız*" |Bir ürün API ilişkilendirerek API yayımlayacak. Yayımlanması ve geliştiricileri için kullanılabilir olması API istiyorsanız, bir ürün ekleyin. API oluşturma sırasında yapın ya da daha sonra ayarlayın.<br/><br/>Ürün bir veya daha fazla API'leri ilişkilendirmelerini değil. Geliştiriciler Geliştirici Portalı aracılığıyla sunar ve API sayısını içerir. <br/>Geliştiriciler ilk API erişmek için bir ürüne abone olması gerekir. Bunlar abone olduğunuzda, bunlar herhangi bir API'yi bu ürün için iyi bir abonelik anahtarı alın. APIM örneği oluşturduysanız, varsayılan olarak her ürüne abone için zaten yönetici olduğunuz.<br/><br/> Varsayılan olarak, her API Management örneği iki örnek ürün ile gelir: **Starter** ve **sınırsız**.| 
5. **Oluştur**’u seçin.

Bu noktada, arka uç API'niz işlemlerinde Eşle hiçbir APIM işlemlerinde sahip. Arka uç aracılığıyla ancak APIM üzerinden değil sunulan bir işlem çağırma olursa, bir **404**. 

>[!NOTE] 
> Bazı arka uç hizmetine bağlı olsa bile bir API eklediğinizde, varsayılan olarak, APIM herhangi bir işlem, beyaz liste kadar açığa çıkarır değil bunları. Bir işlemi arka uç hizmetinizin beyaz liste ile arka uç işlemi eşleyen bir APIM işlem oluşturun.
>

## <a name="add-and-test-an-operation"></a>Ve bir işlem test ekleyin

Bu bölümde, arka uç "http://httpbin.org/get" işlemi eşlemek için ekleme "/ Al" işlemi gösterilmektedir.

### <a name="add-the-operation"></a>Ekleme işlemi

1. Önceki adımda oluşturduğunuz API seçin.
2. Tıklatın **+ ekleme işlemi**.
3. İçinde **URL**seçin **almak** ve girin "*/get*" kaynak.
4. Girin "*parçalar*" için **görünen adı**.
5. **Kaydet**’i seçin.

### <a name="test-the-operation"></a>Test işlemi

İşlemi Azure portalında sınayın. İçinde test alternatif olarak, **Geliştirici Portalı**.

1. Seçin **Test** sekmesi.
2. Seçin **parçalar**.
3. Tuşuna **Gönder**.

"Http://httpbin.org/get" işlemi oluşturur yanıt görüntülenir. İşlemlerinizi dönüştürmek istiyorsanız, bkz: [dönüştürme ve API'nizi koruma](transform-api.md).

## <a name="add-and-test-a-parameterized-operation"></a>Ve parametreli işlemi test ekleyin

Bu bölümde, bir parametre alan bir işlem ekleme gösterilmektedir. Bu durumda, "http://httpbin.org/status/200" işlemi eşleyin.

### <a name="add-the-operation"></a>Ekleme işlemi

1. Önceki adımda oluşturduğunuz API seçin.
2. Tıklatın **+ ekleme işlemi**.
3. İçinde **URL**seçin **almak** ve girin "*/status/ {code}*" kaynak. İsteğe bağlı olarak, bu parametre ile ilişkili bazı bilgiler sağlayabilir. Örneğin, "*numarası*" için **türü**, "*200*" için (varsayılan) **değerleri**.
4. "GetStatus" girin **görünen adı**.
5. **Kaydet**’i seçin.

### <a name="test-the-operation"></a>Test işlemi 

İşlemi Azure portalında sınayın.  İçinde test alternatif olarak, **Geliştirici Portalı**.

1. Seçin **Test** sekmesi.
2. Seçin **GetStatus**. Varsayılan olarak kod değeri ayarlamak "*200*". Diğer değerleri test etmek için değiştirebilirsiniz. Örneğin, "*418*".
3. Tuşuna **Gönder**.

    "Http://httpbin.org/status/200" işlemi oluşturur yanıt görüntülenir. İşlemlerinizi dönüştürmek istiyorsanız, bkz: [dönüştürme ve API'nizi koruma](transform-api.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dönüştürme ve yayımlanan bir API koruyun](transform-api.md)
