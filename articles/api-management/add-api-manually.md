---
title: Azure portalını kullanarak el ile API ekleme  | Microsoft Docs
description: Bu öğreticide, el ile API eklemek için API Management’ın (APIM) nasıl kullanılacağı gösterilir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: ef7cfa0f30eaaa426c312b21ce0a73aa4409d2ec
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="add-an-api-manually"></a>El ile API ekleme 

Bu makaledeki adımlar, Azure portalını kullanarak API Management (APIM) örneğine nasıl el ile API ekleneceğini gösterir. Boş bir API oluşturup el ile tanımlamanızı gerektirecek yaygın bir senaryo, API’nin sahte bir kopyasını oluşturmak istediğiniz durumlardır. Bir API’nin sahte bir kopyasını oluşturma hakkında ayrıntılı bilgi için bkz. [Sahte API yanıtları](mock-api-responses.md).

Mevcut bir API’yi içeri aktarmak istiyorsanız [ilgili konular](#related-topics) bölümüne bakın.

Bu makalede boş bir API oluşturacak ve [httpbin.org](http://httpbin.org)’u (genel bir test hizmeti) bir arka uç API’si olarak ekleyeceğiz.

## <a name="prerequisites"></a>Ön koşullar

Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-an-api"></a>Bir API oluşturma

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. Soldaki menüden **+ API Ekle**'yi seçin.
3. Listeden **Boş API**’yi seçin.

    ![Boş API](media/add-api-manually/blank-api.png)
4. API için ayarları girin.

    ![Ayarlar](media/add-api-manually/settings.png)

    |**Ad**|**Değer**|**Açıklama**|
    |---|---|---|
    |**Görünen ad**|"*Boş API*" |Bu ad, Geliştirici portalında görüntülenir.|
    |**Web Hizmeti URL’si** (isteğe bağlı)| "*http://httpbin.org*"| Bir API’nin sahte bir kopyasını oluşturmak istiyorsanız hiçbir şey girmeyebilirsiniz. <br/>Bu durumda [http://httpbin.org](http://httpbin.org) değerini gireriz. Bu, genel bir test hizmetidir. <br/>Bir arka uçla otomatik olarak eşlenen bir API’yi içeri aktarmak istiyorsanız [ilgili konular](#related-topics) bölümündeki konulardan birine bakın.|
    |**URL düzeni**|"*HTTPS*"|Bu durumda, arka ucun HTTP erişimi güvenli olmasa da arka uç için güvenli bir HTTPS APIM erişimi belirtiriz. <br/>Bu tür senaryolara (HTTPS’den HTTP’ye) is HTTPS sonlandırma denir. API’niz HTTPS kullanılmasa bile erişimin güvenli olduğunu bildiğiniz bir sanal ağın içindeyse bunu yapabilirsiniz. <br/>Bazı CPU döngülerinde tasarruf sağlamak için "HTTPS sonlandırma" kullanmak isteyebilirsiniz.|
    |**URL son eki**|"*hbin*"| Son ek, bu APIM örneğinde özel olarak bu API’yi tanımlayan bir addır. Son ekin bu APIM örneğinde benzersiz olması gerekir.|
    |**Ürünler**|"*Unlimited*" |API’yi bir ürünle ilişkilendirerek yayımlayın. API’nin yayımlanmasını ve geliştiricilerin kullanımına sunulmasını istiyorsanız API’yi bir ürüne ekleyin. Bunu API oluşturması sırasında yapabilir ya da daha sonra ayarlayabilirsiniz.<br/><br/>Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. <br/>Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve varsayılan olarak tüm ürünlere abone olmuşsunuz demektir.<br/><br/> Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir: **Başlangıç** ve **Sınırsız**.| 
5. **Oluştur**’u seçin.

Bu noktada, APIM üzerinde arka uç API’nizdeki işlemlerle eşleşen herhangi bir işlem yoktur. Arka uç aracılığıyla kullanıma sunulup APIM ile sunulmamış bir işleme çağrı yaparsanız **404** yanıtını alırsınız. 

>[!NOTE] 
> Varsayılan olarak, bir API eklediğinizde bu API bir arka uç hizmetine bağlı olsa bile sizin tarafınızdan beyaz listeye eklenene kadar APIM tarafından herhangi bir işlem sunulmaz. Arka uç hizmetinizin bir işlemini beyaz listeye eklemek için arka uç işlemiyle eşlenen bir APIM işlemi oluşturun.
>

## <a name="add-and-test-an-operation"></a>İşlem ekleme ve test etme

Bu bölümde, arka uçtaki "http://httpbin.org/get" işlemiyle eşlemek üzere nasıl bir "/get" işlemi ekleneceği gösterilir.

### <a name="add-the-operation"></a>İşlemi ekleme

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **+ İşlem Ekle**’ye tıklayın.
3. **URL** bölümünde **GET** seçeneğini belirleyip kaynak bölümüne "*/get*" değerini girin.
4. **Görünen ad** alanına "*FetchData*" değerini girin.
5. **Kaydet**’i seçin.

### <a name="test-the-operation"></a>İşlemi test etme

Azure portalında işlemi test edin. Alternatif olarak, **Geliştirici portalında** da test edebilirsiniz.

1. **Test** sekmesini seçin.
2. **FetchData** seçeneğini belirleyin.
3. **Gönder**’e basın.

"http://httpbin.org/get" işleminin oluşturduğu yanıt görünür. İşlemlerinizi dönüştürmek istiyorsanız bkz. [API'nizi dönüştürme ve koruma](transform-api.md).

## <a name="add-and-test-a-parameterized-operation"></a>Parametreli işlem ekleme ve test etme

Bu bölümde, nasıl parametre alan bir işlem ekleneceği gösterilir. Bu durumda, işlemi "http://httpbin.org/status/200" ile eşliyoruz.

### <a name="add-the-operation"></a>İşlemi ekleme

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **+ İşlem Ekle**’ye tıklayın.
3. **URL** bölümünde **GET** seçeneğini belirleyip kaynak bölümüne "*/status/{code}*" değerini girin. İsteğe bağlı olarak, bu parametre ile ilişkili bazı bilgiler sağlayabilirsiniz. Örneğin, **TYPE** alanına "*Number*" değerini, **VALUES** alanına "*200*" (varsayılan) değerini girin.
4. **Görünen ad** alanına "GetStatus" değerini girin.
5. **Kaydet**’i seçin.

### <a name="test-the-operation"></a>İşlemi test etme 

Azure portalında işlemi test edin.  Alternatif olarak, **Geliştirici portalında** da test edebilirsiniz.

1. **Test** sekmesini seçin.
2. **GetStatus** seçeneğini belirleyin. Varsayılan olarak, kod değeri "*200*" olarak ayarlanır. Diğer değerleri test etmek için değiştirebilirsiniz. Örneğin, "*418*" yazın.
3. **Gönder**’e basın.

    "http://httpbin.org/status/200" işleminin oluşturduğu yanıt görünür. İşlemlerinizi dönüştürmek istiyorsanız bkz. [API'nizi dönüştürme ve koruma](transform-api.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)
