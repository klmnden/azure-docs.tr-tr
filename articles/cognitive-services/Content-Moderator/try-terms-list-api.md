---
title: Azure içerik denetleyicinin özel terim listelerinde metinle Orta | Microsoft Docs
description: İçerik denetleyici API konsol özel terim listelerinde dediğini.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 2542e4590781879408aafe8d072eceef157e02c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351815"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>API konsolunda özel terim listeleriyle Orta

Varsayılan Genel Azure içeriği denetleyici koşullarını birçok içerik yönetimini ihtiyaçları için yeterli listesidir. Ancak, kuruluşunuz için belirli koşullar için ekran gerekebilir. Örneğin, daha fazla inceleme için etiket rakip adları isteyebilirsiniz. 

Kullanmak [listesi Yönetimi API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) metin denetleme API ile kullanmak için koşulları özel listesini oluşturmak için. **Metin - ekran** işlemi metninizi uygunsuz metin için tarar ve ayrıca özel ve paylaşılan blacklists karşı metni karşılaştırır.

> [!NOTE]
> Maksimum sınırı yoktur **5 terim listeler** her listesine ile **10.000 koşulları aşmaması**.
>

Liste yönetim API'si, aşağıdaki görevleri gerçekleştirmek için kullanabilirsiniz:
- Bir liste oluşturur.
- Koşulları bir listesine ekleyin.
- Bir liste koşullarını karşı ekran koşulları.
- Koşulları listeden silin.
- Bir listeyi silin.
- Liste bilgilerini düzenleyin.
- Yeni bir tarama listeye yapılan değişiklikler dahil edilmesini dizini yenileyin.

## <a name="use-the-api-console"></a>API Konsolu

Çevrimiçi konsolunda API dediğini önce abonelik anahtarınızı gerekir. Bu anahtarı bulunan **ayarları** sekmesinde **Apim abonelik anahtar Ocp** kutusu. Daha fazla bilgi için bkz: [genel bakış](overview.md).

## <a name="refresh-search-index"></a>Arama dizini Yenile

Bir terim listesine değişiklikleri yaptıktan sonra gelecekteki taramalarında dahil edilecek değişiklikleri dizinini yenilemeniz gerekir. Bu adım, nasıl bir arama motoru (etkinse) masaüstü veya bir web arama motoru sürekli yeni dosyalar veya sayfaları içerecek şekilde dizinini yeniler için benzer.

1.  İçinde [terim listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde seçin **terim listeler**ve ardından **yenileme arama dizini**. 

  **Terim listeler - yenileme arama dizini** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Terim listeleri - yenileme arama dizini sayfa bölge seçimi](images/test-drive-region.png)

  **Terim listeler - yenileme arama dizini** API konsolu açılır.

3.  İçinde **ListId** kutusunda, liste kimliğini girin. Abonelik anahtarınızı girin ve ardından **Gönder**.

  ![Terim listeleri API - yenileme arama dizini konsol yanıt içerik kutusunun](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Bir terim listesi oluşturma
1.  Git [terim listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). 

  **Terim listeler - oluşturmak** sayfası açılır.

2.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Listeleri terim - sayfası bölge seçimi oluşturma](images/test-drive-region.png)

  **Terim listeler - oluşturmak** API konsolu açılır.
 
3.  İçinde **Apim abonelik anahtar Ocp** kutusuna, abonelik anahtarınızı girin.

4.  İçinde **istek gövdesinde** kutusuna, için değerler girin **adı** (örneğin, MyList) ve **açıklama**.

  ![Listeleri terim - konsol istek gövdesi ad ve açıklama oluşturun.](images/try-terms-list-create-1.png)

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

  Liste meta verileri anahtar-değer çiftleri ve gerçek koşulları ekleyin.
 
6.  **Gönder**’i seçin. Listenize oluşturulur. Not **kimliği** yeni listesiyle ilişkili değer. Diğer Terime listesi yönetim işlevleri için bu kimliği gerekir.

  ![Listeleri terim - konsol yanıt kutusu listesi kimliği gösterir içerik oluşturma](images/try-terms-list-create-2.png)
 
7.  Koşulları için MyList ekleyin. Soldaki menüde altında **terim**seçin **eklemek terim**. 

  **Terim - terim eklemek** sayfası açılır. 

8.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Terim - terim sayfa bölge seçimi ekleme](images/test-drive-region.png)

  **Terim - terim eklemek** API konsolu açılır.
 
9.  İçinde **ListId** kutusunda, oluşturulan liste kimliği girin ve için bir değer seçin **dil**. Abonelik anahtarınızı girin ve ardından **Gönder**.

  ![Terim - terim konsol sorgu parametreleri ekleme](images/try-terms-list-create-3.png)
 
10. Terimi soldaki menüde listesine eklendiğini doğrulamak için seçeneklerini **terim**ve ardından **alma tüm koşulları**. 

  **Terim - Al tüm koşulları** API konsolu açılır.

11. İçinde **ListId** kutusuna liste kimliği girin ve sonra abonelik anahtarınızı girin. **Gönder**’i seçin.

12. İçinde **yanıt içeriği** kutusunda, girdiğiniz koşulları doğrulayın.

  ![Terim - Get tüm koşulları konsol yanıt içerik kutusunun listeleri girdiğiniz koşulları](images/try-terms-list-create-4.png)
 
13. Birkaç daha fazla koşulları ekleyin. Özel terimleri listesini oluşturduğunuza göre deneyin [bazı metinleri tarama](try-text-api.md) özel terim listesini kullanarak. 

## <a name="delete-terms-and-lists"></a>Hüküm ve listeleri sil

Bir terim veya bir liste silme basittir. Aşağıdaki görevleri gerçekleştirmek için API kullanın:

- Bir terim silin. (**Terim - Sil**)
- Bir listedeki tüm koşulları listesi silmeden silin. (**Terim - tüm koşulları silme**)
- Bir listesi ve tüm içeriğini silin. (**Terim listeleri - Delete**)

Bu örnek, tek bir terim siler.

1.  İçinde [terim listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde seçin **terim**ve ardından **silmek**. 

  **Terim - Sil** açar.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Terim - Delete sayfa bölge seçimi](images/test-drive-region.png)

  **Terim - Sil** API konsolu açılır.
  
3.  İçinde **ListId** kutusuna, gelen bir terim silmek istediğiniz listesi Kimliğini girin. Bu kimlik numarasıdır (örneğimizde **122**) içinde döndürülen **terim listeler - Al ayrıntıları** MyList için konsolu. Terim girin ve bir dil seçin.
 
  ![Terim - Delete konsol sorgu parametreleri](images/try-terms-list-delete-1.png)

4.  Abonelik anahtarınızı girin ve ardından **Gönder**.

5.  Terim silinip silinmediğini doğrulamak için kullanılan **terim listeler -, Get tüm** konsol.

  ![Listeleri terim - tüm konsol yanıt kutusu terim silinip silinmediğini gösterir içerik Al](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Liste bilgilerini değiştirme

Listenin adını ve açıklamasını düzenleyin ve meta veri öğeleri ekleyin.

1.  İçinde [terim listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), soldaki menüde seçin **terim listeler**ve ardından **Güncelleştirme ayrıntıları**. 

  **Terim listeler - Güncelleştirme ayrıntıları** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Terim listeleri - güncelleştirme Ayrıntıları sayfası bölge seçimi](images/test-drive-region.png)

  **Terim listeler - Güncelleştirme ayrıntıları** API konsolu açılır.

3.  İçinde **ListId** kutusuna liste kimliği girin ve sonra abonelik anahtarınızı girin.

4.  İçinde **istek gövdesinde** kutusunda yaptığınız düzenlemeleri yapın ve ardından **Gönder**.

  ![Terim listeleri - Güncelleştirme ayrıntıları konsol istek gövdesi düzenlemeleri](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Sonraki adımlar

REST API kodunuzdaki kullanın veya başlayın [terim listeler .NET quickstart](term-lists-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
