---
title: "Azure portal ile API yanıtları mock | Microsoft Docs"
description: "Bu öğretici, API Management (APIM) mocked yanıt döndürecek şekilde bir API üzerine bir ilke ayarlamak için nasıl kullanılacağını gösterir. Uygulama ve API Management örneği durumda arka uç test ile devam etmek için bu yöntemi endables geliştiriciler gerçek yanıtları göndermek kullanılabilir değil."
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
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: e485071b026c52eb23532639546ad475fc92cde3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="mock-api-responses"></a>Sahte API yanıtları

Arka uç API'leri APIM API içeri veya oluşturulabilir ve el ile yönetilebilir. Bu öğreticideki adımlardan APIM boş bir API oluşturmak ve el ile yönetmek için nasıl kullanılacağını gösterir. Öğretici mocked yanıt döndürecek şekilde bir ilke bir API kümesi gösterilmektedir. Bu yöntemi uygulaması ve arka uç gerçek yanıtları göndermek kullanılabilir olsa bile APIM örneği test ile devam etmek geliştiricilere sağlar. Yanıtları temizleyip mock yeteneği çeşitli senaryoları yararlı olabilir:

+ Ne zaman API cephesi ilk tasarlanmıştır ve arka uç uygulama daha sonra gelir. Veya, arka uç paralel olarak geliştirilmiştir.
+ Ne zaman arka uç geçici olarak işletimsel veya ölçeklendirmek için değil.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Test API'si oluşturma 
> * API testine işlem ekleme
> * Yanıt mocking etkinleştir
> * Mocked API testi

![Mocked işlemi yanıtı](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-a-test-api"></a>Test API'si oluşturma 

Bu bölümdeki adımları hiçbir arka ucuyla boş bir API oluşturmayı gösterir. Ayrıca, bir işlem için API eklemek nasıl gösterir. Bu bölümdeki adımları tamamladıktan sonra işlem çağrılırken bir hata oluşturur. "Yanıt mocking etkinleştir" bölümündeki adımları tamamladıktan sonra herhangi bir hata alırsınız.

1. Seçin **API'leri** gelen altında **API MANAGEMENT**.
2. Sol menüden seçin **+ API ekleme**.
3. Seçin **boş API** listeden.
4. Girin "*Test API*" için **görünen adı**.
5. Girin "*sınırsız*" için **ürünleri**.
6. **Oluştur**’u seçin.

## <a name="add-an-operation-to-the-test-api"></a>API testine işlem ekleme

1. Önceki adımda oluşturduğunuz API seçin.
2. Tıklatın **+ ekleme işlemi**.

    ![Mocked işlemi yanıtı](./media/mock-api-responses/mock-api-responses02.png)

    |Ayar|Değer|Açıklama|
    |---|---|---|
    |**URL** (HTTP fiili)|GET|Önceden tanımlanmış HTTP fiilleri birinden seçebilirsiniz.|
    |**URL** |*/ Test*|API için bir URL yolu. |
    |**Görünen ad**|*Test çağrısı*|Görüntülenen adı **Geliştirici Portalı**.|
    |**Açıklama**||Bu API kullanan geliştiriciler için belgeleri sağlamak için kullanılan işlem bir açıklama belirtin **Geliştirici Portalı**.|
    |**Sorgu** sekmesi||Sorgu parametreleri ekleyebilirsiniz. Bir ad ve açıklama sağlamanın yanı sıra, bu parametre için atanan değerler sağlayabilirsiniz. Değerlerden biri varsayılan olarak (isteğe bağlı) işaretlenebilir.|
    |**İstek** sekmesi||İstek içerik türleri, örnekler ve şemalar tanımlayabilirsiniz. |
    |**Yanıt** sekmesi|Bu tablo izleyin adımlara bakın.|Yanıt durum kodları, içerik türleri, örnekler ve şemalar tanımlayın.|

3. Seçin **yanıt** URL altında bulunan sekmesinde, görünen ad ve açıklama alanları.
4. Tıklatın **+ yanıtı ekleme**.
5. Seçin **200 Tamam** listeden.
6. Altında **Beyanları** sağ tarafta başlığını seçin **+ gösterimi eklemek**.
7. Girin "*uygulama/json*" seçin ve arama kutusu içine **uygulama/json** içerik türü.
8. İçinde **örnek** metin kutusuna "*{'sampleField': 'test'}*".
9. **Kaydet**’i seçin.

## <a name="enable-response-mocking"></a>Yanıt mocking etkinleştir

1. "Testi API'si oluşturma" adımda oluşturduğunuz API seçin.
2. Eklediğiniz test işlemi seçin.
2. Sağdaki penceresinde **tasarım** sekmesi.
3. İçinde **gelen işleme** penceresinde kalem simgesine tıklayın.
4. İçinde **Mocking** sekmesine **statik yanıtlar** için **davranışı Mocking**.
5. İçinde **API Management aşağıdaki yanıtı döndürür:** metin kutusunda, **200 Tamam, uygulama/json**. Bu seçim, API önceki bölümde tanımlanan yanıtlama örnek döndürmesi gerektiğini gösterir.
6. **Kaydet**’i seçin.

## <a name="test-the-mocked-api"></a>Mocked API testi

1. "Testi API'si oluşturma" adımda oluşturduğunuz API seçin.
2. Açık **Test** sekmesi.
3. Olun **Test çağrısı** API seçilidir.

    > [!TIP]
    > Sarı çubuğu metniyle **Mocking etkin** API Yönetimi'nden döndürülen yanıt mocking bir ilkeyi ve bir gerçek arka uç yanıt gönderir gösterir.

3. Seçin **Gönder** testine çağrı yapma.
4. **HTTP yanıtı** öğreticinin ilk bölümdeki örnek olarak sağlanan JSON görüntüler.

## <a name="video"></a>Video

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Test API'si oluşturma
> * API testine işlem ekleme
> * Yanıt mocking etkinleştir
> * Mocked API testi

Sonraki öğretici ilerleyin:

> [!div class="nextstepaction"]
> [Dönüştürme ve yayımlanan bir API koruyun](transform-api.md)
