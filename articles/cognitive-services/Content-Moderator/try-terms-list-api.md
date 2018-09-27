---
title: Özel terim listeleri - Content Moderator ile orta metni
titlesuffix: Azure Cognitive Services
description: Content Moderator API'si konsolunda özel terim listeleri test edin.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 99df9fda2cc56f169a61ec215a976de28fc13d27
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220287"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>API konsolunda özel terim listeleri ile orta

Varsayılan Genel Azure Content Moderator koşullarını çoğu içerik denetimi ihtiyaçları için yeterli listesidir. Ancak, kuruluşunuz için belirli koşulların ekran gerekebilir. Örneğin, daha fazla inceleme için rakip etiketlerden isteyebilirsiniz. 

Kullanma [listesi Yönetimi API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) özel metin denetimi API'si ile kullanmak için koşullarını listesini oluşturmak için. **Metin - ekran** işlemi metninizi küfür tarar ve ayrıca özel ve paylaşılan kara karşı metni karşılaştırır.

> [!NOTE]
> Bir maksimum sınırı **5 terim listeleri** her listesine ile **10.000 koşulları aşmayacak**.
>

Listeyi yönetim API'si, aşağıdaki görevleri gerçekleştirmek için kullanabilirsiniz:
- Bir liste oluşturur.
- Koşulları bir listesine ekleyin.
- Bir liste koşullarını karşı ekran koşulları.
- Koşulları listeden silin.
- Bir listeyi silin.
- Liste bilgileri düzenleyin.
- Listeye yeni bir tarama dahil edilmesini dizini yenileyin.

## <a name="use-the-api-console"></a>API Konsolu

Çevrimiçi konsolunda API'yi test sürüşü önce abonelik anahtarınızı gerekir. Bu anahtar bulunan **ayarları** sekmesinde **Ocp-Apim-Subscription-Key** kutusu. Daha fazla bilgi için [genel bakış](overview.md).

## <a name="refresh-search-index"></a>Arama dizini Yenile

Bir terimi listesine değişiklikler yaptıktan sonra gelecekteki taramalarında dahil edilecek değişiklikler dizinini yenilemeniz gerekir. Bu adım, nasıl bir arama motoru (etkinse) masaüstü veya bir web arama motoru sürekli olarak yeni dosyalar veya sayfalar eklemek için dizinini yeniler için benzerdir.

1.  İçinde [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde **terim listeleri**ve ardından **Yenile arama dizini**. 

  **Terim listeleri - yenileme arama dizini** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

  ![Terim listeleri - yenileme arama dizin sayfası kayıt seçimi](images/test-drive-region.png)

  **Terim listeleri - yenileme arama dizini** API konsolu açılır.

3.  İçinde **ListId** kutusunda, liste kimliğini girin. Abonelik anahtarınızı girin ve ardından **Gönder**.

  ![Terim listesi API'si - yenileme arama dizin Konsolu yanıt içerik kutusu](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Terim listesi oluşturma
1.  Git [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). 

  **Terim listeleri - oluşturma** sayfası açılır.

2.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

  ![Terim listeleri - kayıt seçimi sayfası oluşturma](images/test-drive-region.png)

  **Terim listeleri - oluşturma** API konsolu açılır.
 
3.  İçinde **Ocp-Apim-Subscription-Key** kutusuna, abonelik anahtarınızı girin.

4.  İçinde **istek gövdesi** için değerleri girin, kutusunda **adı** (örneğin, MyList) ve **açıklama**.

  ![Terim listeleri - konsol istek gövdesi ad ve açıklama oluşturma](images/try-terms-list-create-1.png)

5.  Anahtar-değer çifti yer tutucuları daha açıklayıcı meta verileri listenize atamak için kullanın.

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
 
6.  **Gönder**’i seçin. Listenize oluşturulur. Not **kimliği** yeni listesiyle ilişkili değer. Diğer Terime listesi Yönetimi işlevleri için bu kodu ihtiyacınız var.

  ![Terim listeleri - konsol yanıt kutusu liste kimliği gösterir içerik oluşturma](images/try-terms-list-create-2.png)
 
7.  Koşulları için MyList ekleyin. Soldaki menüde altında **terimi**seçin **terim ekleme**. 

  **Terim - terim ekleme** sayfası açılır. 

8.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

  ![Terim - terimi sayfa bölge seçimi ekleme](images/test-drive-region.png)

  **Terim - terim ekleme** API konsolu açılır.
 
9.  İçinde **ListId** kutusuna oluşturduğunuz liste kimliği girin ve seçmek için bir değer **dil**. Abonelik anahtarınızı girin ve ardından **Gönder**.

  ![Terim - terimi konsol sorgu parametreleri ekleme](images/try-terms-list-create-3.png)
 
10. Terim soldaki menüde listesine eklendiğini doğrulamak için **terimi**ve ardından **alma olan tüm koşulları**. 

  **Terimi - tüm koşulları alma** API konsolu açılır.

11. İçinde **ListId** kutusunda, liste Kimliğini girin ve sonra abonelik anahtarınızı girin. **Gönder**’i seçin.

12. İçinde **yanıt içeriği** kutusunda, girdiğiniz koşulları doğrulayın.

  ![Terim - Get tüm koşulları Konsolu yanıt içerik kutusu listeleri girdiğiniz koşulları](images/try-terms-list-create-4.png)
 
13. Birkaç daha fazla koşulları ekleyin. Özel terimleri listesini oluşturduğunuza göre deneyin [metin tarama](try-text-api.md) kullanarak özel terim listesi. 

## <a name="delete-terms-and-lists"></a>Hüküm ve listeleri sil

Bir terimi veya ayrılmış bir listeyi silmek oldukça basittir. Aşağıdaki görevleri gerçekleştirmek için API'yi kullanın:

- Bir terimi silin. (**Terim - Sil**)
- Bir listedeki tüm koşulları, listenin silmeden silin. (**Terim - tüm koşulları Sil**)
- Bir liste ve tüm içeriğini silin. (**Terim listeleri - silme**)

Bu örnek, tek bir terim siler.

1.  İçinde [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde **terimi**ve ardından **Sil**. 

  **Terim - Sil** açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

  ![Terim - silme sayfası kayıt seçimi](images/test-drive-region.png)

  **Terim - Sil** API konsolu açılır.
  
3.  İçinde **ListId** kutusunda, bir terimden silmek istediğiniz listenin kimliği girin. Bu kimliği sayıdır (Bizim örneğimizde **122**) döndürülen **terim listeleri - Al ayrıntıları** MyList Konsolu. Terimini girin ve bir dil seçin.
 
  ![Terim - Delete konsol sorgu parametreleri](images/try-terms-list-delete-1.png)

4.  Abonelik anahtarınızı girin ve ardından **Gönder**.

5.  Terim silinip silinmediğini doğrulamak için **terim listeleri -, alma tüm** Konsolu.

  ![Terim listeleri: tüm konsol yanıt kutusu terimi silindiğini gösterir içerik alma](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Liste bilgilerini değiştirme

Bir listenin adını ve açıklamasını düzenlemek ve meta veri öğeleri ekleyin.

1.  İçinde [terim listesi yönetim API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde **terim listeleri**ve ardından **Update Details**. 

  **Terim listeleri - Update Details** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

  ![Terim listeleri - güncelleştirme Ayrıntıları sayfası kayıt seçimi](images/test-drive-region.png)

  **Terim listeleri - Update Details** API konsolu açılır.

3.  İçinde **ListId** kutusunda, liste Kimliğini girin ve sonra abonelik anahtarınızı girin.

4.  İçinde **istek gövdesi** kutusuna düzenlemelerinizi yapın ve ardından **Gönder**.

  ![Terim listeleri - Update Details konsol istek gövdesi düzenlemeleri](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [terim listeleri .NET Hızlı Başlangıç](term-lists-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
