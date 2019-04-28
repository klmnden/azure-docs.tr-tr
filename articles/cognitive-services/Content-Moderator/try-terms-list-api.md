---
title: Özel terim listeleri - Content Moderator ile orta metni
titlesuffix: Azure Cognitive Services
description: Özel metin denetimi API'si ile kullanmak için koşullarını listesini oluşturmak için listesi yönetim API'sini kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 28029fe92a207dba85e2ab5a22c08879b7172925
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097955"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>API konsolunda özel terim listeleri ile orta

Azure Content Moderator'daki varsayılan genel terim listesi, içerik moderasyonu ihtiyaçlarının büyük bölümü için yeterlidir. Bununla birlikte, kuruluşunuza özgü terimleri elemek gerekebilir. Örneğin, daha fazla incelemek üzere rakiplerin adlarını etiketlemek isteyebilirsiniz. 

Kullanma [listesi Yönetimi API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) özel metin denetimi API'si ile kullanmak için koşullarını listesini oluşturmak için. **Metin - ekran** işlemi metninizi küfür tarar ve ayrıca özel ve paylaşılan kara karşı metni karşılaştırır.

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

Listeyi yönetim API'si, aşağıdaki görevleri gerçekleştirmek için kullanabilirsiniz:
- Liste oluşturma.
- Listeye terimleri ekleme.
- Terimleri listedeki terimlere göre eleme.
- Listeden terim silme.
- Listeyi silme.
- Liste bilgileri düzenleme.
- Listede yapılan değişikliklerin yeni taramaya eklenmesi için dizini yenileyin.

## <a name="use-the-api-console"></a>API Konsolu

Çevrimiçi konsolunda API'yi test sürüşü önce abonelik anahtarınızı gerekir. Bu anahtar bulunan **ayarları** sekmesinde **Ocp-Apim-Subscription-Key** kutusu. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="refresh-search-index"></a>Arama dizini Yenile

Bir terimi listesine değişiklikler yaptıktan sonra gelecekteki taramalarında dahil edilecek değişiklikler dizinini yenilemeniz gerekir. Bu adım, nasıl bir arama motoru (etkinse) masaüstü veya bir web arama motoru sürekli olarak yeni dosyalar veya sayfalar eklemek için dizinini yeniler için benzerdir.

1. İçinde [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde **terim listeleri**ve ardından **Yenile arama dizini**. 

   **Terim listeleri - yenileme arama dizini** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Terim listeleri - yenileme arama dizin sayfası kayıt seçimi](images/test-drive-region.png)

   **Terim listeleri - yenileme arama dizini** API konsolu açılır.

3. İçinde **ListId** kutusunda, liste kimliğini girin. Abonelik anahtarınızı girin ve ardından **Gönder**.

   ![Terim listesi API'si - yenileme arama dizin Konsolu yanıt içerik kutusu](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Terim listesi oluşturma
1. Git [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). 

   **Terim listeleri - oluşturma** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Terim listeleri - kayıt seçimi sayfası oluşturma](images/test-drive-region.png)

   **Terim listeleri - oluşturma** API konsolu açılır.
 
3. İçinde **Ocp-Apim-Subscription-Key** kutusuna, abonelik anahtarınızı girin.

4. İçinde **istek gövdesi** için değerleri girin, kutusunda **adı** (örneğin, MyList) ve **açıklama**.

   ![Terim listeleri - konsol istek gövdesi ad ve açıklama oluşturma](images/try-terms-list-create-1.png)

5. Anahtar-değer çifti yer tutucuları daha açıklayıcı meta verileri listenize atamak için kullanın.

       {
          "Name": "MyExclusionList",
          "Description": "MyListDescription",
          "Metadata": 
          {
             "Category": "Competitors",
             "Type": "Exclude"
          }
       }

   Liste meta verilerini, anahtar-değer çiftleri ve gerçek koşulları ekleyin.
 
6. **Gönder**’i seçin. Listenize oluşturulur. Not **kimliği** yeni listesiyle ilişkili değer. Diğer Terime listesi Yönetimi işlevleri için bu kodu ihtiyacınız var.

   ![Terim listeleri - konsol yanıt kutusu liste kimliği gösterir içerik oluşturma](images/try-terms-list-create-2.png)
 
7. Koşulları için MyList ekleyin. Soldaki menüde altında **terimi**seçin **terim ekleme**. 

   **Terim - terim ekleme** sayfası açılır. 

8. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Terim - terimi sayfa bölge seçimi ekleme](images/test-drive-region.png)

   **Terim - terim ekleme** API konsolu açılır.
 
9. İçinde **ListId** kutusuna oluşturduğunuz liste kimliği girin ve seçmek için bir değer **dil**. Abonelik anahtarınızı girin ve ardından **Gönder**.

   ![Terim - terimi konsol sorgu parametreleri ekleme](images/try-terms-list-create-3.png)
 
10. Terim soldaki menüde listesine eklendiğini doğrulamak için **terimi**ve ardından **alma olan tüm koşulları**. 

    **Terimi - tüm koşulları alma** API konsolu açılır.

11. İçinde **ListId** kutusunda, liste Kimliğini girin ve sonra abonelik anahtarınızı girin. **Gönder**’i seçin.

12. İçinde **yanıt içeriği** kutusunda, girdiğiniz koşulları doğrulayın.

    ![Terim - Get tüm koşulları Konsolu yanıt içerik kutusu listeleri girdiğiniz koşulları](images/try-terms-list-create-4.png)
 
13. Birkaç daha fazla koşulları ekleyin. Özel terimleri listesini oluşturduğunuza göre deneyin [metin tarama](try-text-api.md) kullanarak özel terim listesi. 

## <a name="delete-terms-and-lists"></a>Terimleri ve listeleri silme

Terim veya listeyi silmek basit bir işlemdir. Aşağıdaki görevleri gerçekleştirmek için API'yi kullanın:

- Terim silme. (**Terim - Sil**)
- Listeyi silmeden listedeki tüm terimleri silme. (**Terim - tüm koşulları Sil**)
- Listeyi ve tüm içeriğini silme. (**Terim listeleri - silme**)

Bu örnek, tek bir terim siler.

1. İçinde [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde **terimi**ve ardından **Sil**. 

   **Terim - Sil** açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Terim - silme sayfası kayıt seçimi](images/test-drive-region.png)

   **Terim - Sil** API konsolu açılır.
  
3. İçinde **ListId** kutusunda, bir terimden silmek istediğiniz listenin kimliği girin. Bu kimliği sayıdır (Bizim örneğimizde **122**) döndürülen **terim listeleri - Al ayrıntıları** MyList Konsolu. Terimini girin ve bir dil seçin.
 
   ![Terim - Delete konsol sorgu parametreleri](images/try-terms-list-delete-1.png)

4. Abonelik anahtarınızı girin ve ardından **Gönder**.

5. Terim silinip silinmediğini doğrulamak için **terim listeleri -, alma tüm** Konsolu.

   ![Terim listeleri: tüm konsol yanıt kutusu terimi silindiğini gösterir içerik alma](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Liste bilgilerini değiştirme

Bir listenin adını ve açıklamasını düzenlemek ve meta veri öğeleri ekleyin.

1. İçinde [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde **terim listeleri**ve ardından **Update Details**. 

   **Terim listeleri - Update Details** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Terim listeleri - güncelleştirme Ayrıntıları sayfası kayıt seçimi](images/test-drive-region.png)

   **Terim listeleri - Update Details** API konsolu açılır.

3. İçinde **ListId** kutusunda, liste Kimliğini girin ve sonra abonelik anahtarınızı girin.

4. İçinde **istek gövdesi** kutusuna düzenlemelerinizi yapın ve ardından **Gönder**.

   ![Terim listeleri - Update Details konsol istek gövdesi düzenlemeleri](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [terim listeleri .NET Hızlı Başlangıç](term-lists-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
