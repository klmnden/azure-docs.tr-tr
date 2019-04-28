---
title: Özel listeler ve API Konsolu - Content Moderator ile orta görüntüleri
titlesuffix: Azure Content Moderator
description: Özel görüntüleri listesi oluşturmak için Azure Content Moderator listesi yönetim API'sini kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 7efa5114a903ba88010ec44f2f1038331df62948
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097989"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>Özel görüntü listeleriyle API konsolundaki Orta

Kullandığınız [listesi Yönetimi API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) özel görüntüleri listesi oluşturmak için Azure Content Moderator içinde. Görüntü denetimi API'si ile özel görüntüleri listesini kullanın. Görüntü denetimi işlemi görüntünüzü değerlendirir. Özel listeler oluşturursanız, işlemi de listelerinizin özel görüntüleri karşılaştırır. Görüntü veya özel listeleri kullanabilirsiniz.

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

Aşağıdaki görevleri gerçekleştirmek için listesi yönetim API'sini kullanın:

- Liste oluşturma.
- Görüntüleri bir listesine ekleyin.
- Ekran görüntüleri karşı bir liste görüntüler.
- Görüntüleri bir listeden silin.
- Listeyi silme.
- Liste bilgileri düzenleme.
- Listede yapılan değişikliklerin yeni taramaya eklenmesi için dizini yenileyin.

## <a name="use-the-api-console"></a>API Konsolu
Çevrimiçi konsolunda API'yi test sürüşü önce abonelik anahtarınızı gerekir. Bu dosya çubuğunda bulunur **ayarları** sekmesinde **Ocp-Apim-Subscription-Key** kutusu. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="refresh-search-index"></a>Arama dizini Yenile

Görüntü listesi için değişiklikleri yaptıktan sonra değişiklikler gelecekteki taramalarında dahil edilecek dizinini yenilemeniz gerekir. Bu adım, nasıl bir arama motoru (etkinse) masaüstü veya bir web arama motoru sürekli olarak yeni dosyalar veya sayfalar eklemek için dizinini yeniler için benzerdir.

1. İçinde [görüntü listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), sol menüde **görüntü listeleri**ve ardından **Yenile arama dizini**.

   **Görüntü listeleri - yenileme arama dizini** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 
 
    ![Görüntü listeleri - yenileme arama dizin sayfası kayıt seçimi](images/test-drive-region.png)

    **Görüntü listeleri - yenileme arama dizini** API konsolu açılır.

3. İçinde **ListId** kutusunda, liste kimliğini girin. Abonelik anahtarınızı girin ve ardından **Gönder**.

   ![Görüntü listeleri - yenileme arama dizin Konsolu yanıt içerik kutusu](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Görüntü listesi oluşturma

1. Git [görüntü listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672).

   **Görüntü listesi -** sayfası açılır. 

3. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin.

   ![Görüntü listeleri - kayıt seçimi sayfası oluşturma](images/test-drive-region.png)

   **Görüntü listesi -** API konsolu açılır.
 
4. İçinde **Ocp-Apim-Subscription-Key** kutusuna, abonelik anahtarınızı girin.

5. İçinde **istek gövdesi** için değerleri girin, kutusunda **adı** (örneğin, MyList) ve **açıklama**.

   ![Görüntü listeleri - konsol istek gövdesi ad ve açıklama oluşturma](images/try-terms-list-create-1.png)

6. Anahtar-değer çifti yer tutucuları daha açıklayıcı meta verileri listenize atamak için kullanın.

       {
          "Name": "MyExclusionList",
          "Description": "MyListDescription",
          "Metadata": 
          {
            "Category": "Competitors",
            "Type": "Exclude"
          }
       }

   Liste meta verilerini, anahtar-değer çiftleri ve değil gerçek görüntüleri ekleyin.
 
7. **Gönder**’i seçin. Listenize oluşturulur. Not **kimliği** yeni listesiyle ilişkili değer. Diğer resim listesi Yönetimi işlevleri için bu kodu ihtiyacınız var.

   ![Görüntü listeleri - konsol yanıt kutusu liste kimliği gösterir içerik oluşturma](images/try-terms-list-create-2.png)
 
8. Ardından, görüntüleri için MyList ekleyin. Sol menüde **görüntü**ve ardından **görüntüsü Ekle**.

   **- Görüntü ekleme resim** sayfası açılır. 

9. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin.

   ![Görüntü - görüntüsü sayfa bölge seçimini ekleme](images/test-drive-region.png)

   **- Görüntü ekleme resim** API konsolu açılır.
 
10. İçinde **ListId** kutusuna oluşturduğunuz liste kimliği girin ve ardından eklemek istediğiniz görüntü URL'sini girin. Abonelik anahtarınızı girin ve ardından **Gönder**.

11. Görüntü soldaki menüde listesine eklendiğini doğrulamak için **görüntü**ve ardından **tüm resim kimlikleri alma**.

    **Image - tüm resim kimlikleri alma** API konsolu açılır.
  
12. İçinde **ListId** kutusunda, liste Kimliğini girin ve sonra abonelik anahtarınızı girin. **Gönder**’i seçin.

    ![Görüntü - Get tüm resim kimlikleri içerik kutusu girdiğiniz görüntüleri listeler yanıt Konsolu](images/try-image-list-create-11.png)
 
10. Birkaç daha fazla görüntü ekleyin. Özel bir görüntü listesi oluşturduğunuza göre deneyin [görüntüleri değerlendirmek](try-image-api.md) kullanarak özel bir görüntü listesi. 

## <a name="delete-images-and-lists"></a>Görüntüleri ve listeleri sil

Bir görüntü veya ayrılmış bir listeyi silmek oldukça basittir. Aşağıdaki görevleri gerçekleştirmek için API'yi kullanabilirsiniz:

- Görüntüyü silin. (**Görüntü - Sil**)
- Listenin silmeden bir listedeki tüm görüntüleri silin. (**Görüntü - tüm görüntüleri silin**)
- Listeyi ve tüm içeriğini silme. (**Görüntü listeleri - silme**)

Bu örnek, tek bir görüntüyü siler:

1. İçinde [görüntü listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), soldaki menüde **görüntü**ve ardından **Sil**. 

   **Görüntü - Sil** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Görüntü - silme sayfası kayıt seçimi](images/test-drive-region.png)
 
   **Görüntü - Sil** API konsolu açılır.
 
3. İçinde **ListId** kutusunda, bir görüntüden silmek listesinin Kimliğini girin.  Bu iade numarasıdır **Image - tüm resim kimlikleri alma** MyList Konsolu. Ardından, girin **ImageId** görüntünün silinemedi. 

Bizim örneğimizde liste kimliğidir **58953**, değeri **ContentSource**. Görüntü Kimliği **59021**, değeri **ContentIds**.

1. Abonelik anahtarınızı girin ve ardından **Gönder**.

1. Görüntü silinip silinmediğini doğrulamak için **Image - tüm resim kimlikleri alma** Konsolu.
 
## <a name="change-list-information"></a>Liste bilgilerini değiştirme

Bir listenin adını ve açıklamasını düzenlemek ve meta veri öğeleri ekleyin.

1. İçinde [görüntü listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), soldaki menüde **görüntü listeleri**ve ardından **Güncelleştirme ayrıntıları**. 

   **Görüntü listeleri - Update Details** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin.  

    ![Görüntü listeleri - güncelleştirme Ayrıntıları sayfası kayıt seçimi](images/test-drive-region.png)

    **Görüntü listeleri - Update Details** API konsolu açılır.
 
3. İçinde **ListId** kutusunda, liste Kimliğini girin ve sonra abonelik anahtarınızı girin.

4. İçinde **istek gövdesi** kutusuna düzenlemelerinizi yapın ve ardından **Gönder** sayfasında düğme.

   ![Görüntü listeleri - Update Details konsol istek gövdesi düzenlemeleri](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [görüntü listeleri .NET Hızlı Başlangıç](image-lists-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
