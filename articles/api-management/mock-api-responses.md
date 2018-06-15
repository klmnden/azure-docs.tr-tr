---
title: Azure portalı ile sahte API yanıtları | Microsoft Docs
description: Bu öğreticide API Management (APIM) kullanarak bir API üzerinde sahte yanıt döndürmesini sağlayacak bir ilke ayarlama işlemi gösterilmektedir. Bu yöntem, arka ucun gerçek yanıtlar göndermek için kullanılamadığı durumlarda geliştiricilerin API Management örneğinde uygulama ve test işlemlerine devam etmesini sağlar.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4383ce3788f6fade5299d69ef99b80221c58d9e7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33936992"
---
# <a name="mock-api-responses"></a>Sahne API yanıtları

Arka uç API'leri bir APIM API’sine aktarılabilir veya el ile oluşturulup yönetilebilir. Bu öğreticideki adımlar, APIM kullanarak boş bir API oluşturma ve el ile yönetme işlemini gösterir. Bu öğreticide bir API üzerinde sahte yanıt döndürmesini sağlayacak bir ilke ayarlama işlemi gösterilmektedir. Bu yöntem, arka ucun gerçek yanıtlar göndermek için kullanılamadığı durumlarda bile geliştiricilerin APIM örneğinde uygulama ve test işlemlerine devam etmesini sağlar. Sahte yanıtlar oluşturabilmek birkaç senaryoda yararlı olabilir:

+ İlk olarak API cephesi tasarlanıp arka uç uygulaması daha sonra geldiğinde. Veya arka uç paralel olarak geliştirildiğinde.
+ Arka uç geçici olarak çalışır durumda olmadığında veya ölçeklenemediğinde.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Test API’si oluşturma 
> * Test API’sine işlem ekleme
> * Sahte yanıt vermeyi etkinleştirme
> * Sahte API’yi test etme

![Sahte işlem yanıtı](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Ön koşullar

Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="create-a-test-api"></a>Test API’si oluşturma 

Bu bölümdeki adımlar arka uç olmadan boş bir API oluşturmayı gösterir. Ayrıca, API’ye bir işlem eklemeyi gösterir. Bu bölümdeki adımları tamamladıktan sonra işlem çağrılırsa bir hata oluşur. "Sahte yanıt vermeyi etkinleştirme" bölümündeki adımları tamamladıktan sonra herhangi bir hata almazsınız.

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. Soldaki menüden **+ API Ekle**'yi seçin.
3. Listeden **Boş API**’yi seçin.
4. **Görünen ad** için "*Test API*" yazın.
5. **Ürünler** için "*Sınırsız*" yazın.
6. **Oluştur**’u seçin.

## <a name="add-an-operation-to-the-test-api"></a>Test API’sine işlem ekleme

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **+ İşlem Ekle**’ye tıklayın.

    ![Sahte işlem yanıtı](./media/mock-api-responses/mock-api-responses02.png)

    |Ayar|Değer|Açıklama|
    |---|---|---|
    |**URL** (HTTP fiili)|GET|Önceden tanımlanmış HTTP fiillerinden birini seçebilirsiniz.|
    |**URL** |*/test*|API için bir URL yolu. |
    |**Görünen ad**|*Test çağrısı*|**Geliştirici portalında** görüntülenen ad.|
    |**Açıklama**||**Geliştirici portalında** bu API’yi kullanan geliştiricilere belge sağlamak için kullanılan işlemin bir açıklamasını girin.|
    |**Sorgu** sekmesi||Sorgu parametreleri ekleyebilirsiniz. Bir ad ve açıklama sağlamaya ek olarak bu parametreye atanabilecek değerleri belirtebilirsiniz. Varsayılan olarak işaretlenebilecek değerlerde biri (isteğe bağlı).|
    |**İstek** sekmesi||İstek içerik türleri, örnekler ve şemalar tanımlayabilirsiniz. |
    |**Yanıt** sekmesi|Bu tablodan sonraki adımlara bakın.|Yanıt durum kodları, içerik türleri, örnekler ve şemalar tanımlayın.|

3. URL, Görünen ad ve Açıklama alanlarının altında bulunan **Yanıt** sekmesini seçin.
4. **+ Yanıt ekle**’ye tıklayın.
5. Listeden **200 Tamam**’ı seçin.
6. Sağ taraftaki **Gösterimler** başlığının altında **+ Gösterim ekle**’yi seçin.
7. Arama kutusuna "*application/json*" yazın ve **application/json** içerik türünü seçin.
8. **Örnek** metin kutusuna "*{ 'sampleField' : 'test' }*" girin.
9. **Kaydet**’i seçin.

## <a name="enable-response-mocking"></a>Sahte yanıt vermeyi etkinleştirme

1. "Test API’si oluşturma" adımında oluşturduğunuz API’yi seçin.
2. Eklediğiniz test işlemini seçin.
2. Sağdaki pencerede **Tasarım** sekmesine tıklayın.
3. **Gelen işlem** penceresinde kalem simgesine tıklayın.
4. **Sahte** sekmesinde **Sahte işlem davranışı** için **Statik yanıtlar**’ı seçin.
5. **API Management şu yanıtı döndürür:** metin kutusuna **200 Tamam, application/json** yazın. Bu seçim, API’nizin önceki bölümde tanımladığınız yanıt örneğini döndürmesi gerektiğini gösterir.
6. **Kaydet**’i seçin.

## <a name="test-the-mocked-api"></a>Sahte API’yi test etme

1. "Test API’si oluşturma" adımında oluşturduğunuz API’yi seçin.
2. **Test** sekmesini açın.
3. **Test çağrısı** API’sinin seçili olduğundan emin olun.

    > [!TIP]
    > **Sahte işlem etkin** metnini içeren sarı çubuk, API Management’tan döndürülen yanıtların bir sahte işlem ilkesi gönderdiğini ve gerçek bir arka uç yanıtı göndermediğini gösterir.

3. Bir test çağrısı yapmak için **Gönder**’i seçin.
4. **HTTP yanıtı**, öğreticinin ilk bölümde örnek olarak sağlanan JSON’u görüntüler.

## <a name="video"></a>Video

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Test API’si oluşturma
> * Test API’sine işlem ekleme
> * Sahte yanıt vermeyi etkinleştirme
> * Sahte API’yi test etme

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)
