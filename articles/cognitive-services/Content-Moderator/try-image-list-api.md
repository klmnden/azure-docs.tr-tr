---
title: Azure içerik Aracı alanında özel listeleri kullanarak görüntüleri Orta | Microsoft Docs
description: Özel görüntü listeleri içerik denetleyici API Konsolu'nda dediğini.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 2d714f017be16d978ffbb877a2b7e78e1caf9169
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351826"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>Özel görüntü listeleriyle API konsolundaki Orta

Kullandığınız [listesi Yönetimi API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) görüntüleri özel listesini oluşturmak için Azure içerik denetleyicinin içinde. Görüntüleri özel listelerini Görüntü Yönetimi API'si ile kullanın. Görüntü denetleme işlemi görüntünüzü değerlendirir. Özel listeler oluşturursanız, işlemi aynı zamanda özel listelerinizi görüntülerinde karşılaştırır. Engellemek veya görüntü izin vermek için özel listeleri kullanabilirsiniz.

> [!NOTE]
> Maksimum sınırı yoktur **5 görüntü listeleri** her listesine ile **10.000 görüntüleri aşmaması**.
>

Liste Yönetimi API aşağıdaki görevleri gerçekleştirmek için kullanın:

- Bir liste oluşturur.
- Görüntüleri bir listesine ekleyin.
- Ekran görüntüleri karşı bir listede görüntüler.
- Görüntüleri bir listeden silin.
- Bir listeyi silin.
- Liste bilgilerini düzenleyin.
- Yeni bir tarama listeye yapılan değişiklikler dahil edilmesini dizini yenileyin.

## <a name="use-the-api-console"></a>API Konsolu
Çevrimiçi konsolunda API dediğini önce abonelik anahtarınızı gerekir. Bu bulunan **ayarları** sekmesinde **Apim abonelik anahtar Ocp** kutusu. Daha fazla bilgi için bkz: [genel bakış](overview.md).

## <a name="refresh-search-index"></a>Arama dizini Yenile

Bir görüntü listesi değişiklikleri yaptıktan sonra gelecekteki taramalarında dahil edilecek değişiklikleri dizinini yenilemeniz gerekir. Bu adım, nasıl bir arama motoru (etkinse) masaüstü veya bir web arama motoru sürekli yeni dosyalar veya sayfaları içerecek şekilde dizinini yeniler için benzer.

1.  İçinde [görüntü listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), soldaki menüde seçin **görüntü listeleri**ve ardından **yenileme arama dizini**.

  **Görüntü listeleri - yenileme arama dizini** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 
 
    ![Görüntü listeleri - yenileme arama dizini sayfa bölge seçimi](images/test-drive-region.png)

    **Görüntü listeleri - yenileme arama dizini** API konsolu açılır.

3.  İçinde **ListId** kutusunda, liste kimliğini girin. Abonelik anahtarınızı girin ve ardından **Gönder**.

  ![Görüntü listeleri - yenileme arama dizini konsol yanıt içerik kutusunun](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Bir görüntü listesi oluşturma

1.  Git [görüntü listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672).

  **Görüntü listeleri - oluşturmak** sayfası açılır. 

3.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin.

    ![Görüntü listeleri - sayfası bölge seçimi oluşturma](images/test-drive-region.png)

    **Görüntü listeleri - oluşturmak** API konsolu açılır.
 
4.  İçinde **Apim abonelik anahtar Ocp** kutusuna, abonelik anahtarınızı girin.

5.  İçinde **istek gövdesinde** kutusuna, için değerler girin **adı** (örneğin, MyList) ve **açıklama**.

  ![Görüntü listeleri - konsol istek gövdesi ad ve açıklama oluşturun.](images/try-terms-list-create-1.png)

6.  Anahtar-değer çifti yer tutucuları daha açıklayıcı meta verileri listenize atamak için kullanın.

        {
           "Name": "MyExclusionList",
           "Description": "MyListDescription",
           "Metadata": 
           {
             "Category": "Competitors",
             "Type": "Exclude"
           }
        }

  Liste meta verileri anahtar-değer çiftleri ve değil gerçek resim olarak ekleyin.
 
7.  **Gönder**’i seçin. Listenize oluşturulur. Not **kimliği** yeni listesiyle ilişkili değer. Diğer görüntü listesi yönetim işlevleri için bu kimliği gerekir.

  ![Görüntü listeleri - konsol yanıt kutusu listesi kimliği gösterir içerik oluşturma](images/try-terms-list-create-2.png)
 
8.  Ardından, görüntüleri için MyList ekleyin. Soldaki menüde seçin **görüntü**ve ardından **görüntüsü Ekle**.

  **Görüntü - görüntüsü ekleme** sayfası açılır. 

9. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin.

  ![Görüntü - görüntü sayfa bölge seçimi ekleme](images/test-drive-region.png)

  **Görüntü - görüntüsü ekleme** API konsolu açılır.
 
10. İçinde **ListId** kutusuna, oluşturulan liste kimliği girin ve ardından eklemek istediğiniz görüntünün URL'sini girin. Abonelik anahtarınızı girin ve ardından **Gönder**.

11. Görüntü soldaki menüde listesine eklenen doğrulamak için seçin **görüntü**ve ardından **tüm resim kimlikleri alma**.

  **Image - tüm resim kimlikleri alma** API konsolu açılır.
  
12. İçinde **ListId** kutusuna liste kimliği girin ve sonra abonelik anahtarınızı girin. **Gönder**’i seçin.

  ![Görüntü - Get tüm resim kimlikleri içerik kutusuna girdiğiniz görüntüleri listeler yanıt konsol](images/try-image-list-create-11.png)
 
10. Birkaç daha fazla görüntü ekleyin. Özel resimler listesini oluşturduğunuza göre deneyin [görüntüleri değerlendirme](try-image-api.md) özel görüntü listesini kullanarak. 

## <a name="delete-images-and-lists"></a>Görüntüleri ve listeleri sil

Bir görüntü veya bir liste silme basittir. Aşağıdaki görevleri gerçekleştirmek için API'yi kullanabilirsiniz:

- Görüntüyü silin. (**Görüntü - Sil**)
- Listenin silmeden bir listedeki tüm görüntüleri silin. (**Görüntü - tüm görüntüleri silin**)
- Bir listesi ve tüm içeriğini silin. (**Görüntü listeleri - Delete**)

Bu örnek, tek bir görüntü siler:

1. İçinde [görüntü listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), soldaki menüde seçin **görüntü**ve ardından **silmek**. 

  **Görüntü - Sil** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Görüntü - Delete sayfa bölge seçimi](images/test-drive-region.png)
 
  **Görüntü - Sil** API konsolu açılır.
 
3.  İçinde **ListId** kutusunda, bir görüntüden silmek için listeden Kimliğini girin.  Bu iade numarasıdır **Image - tüm resim kimlikleri alma** MyList için konsolu. Ardından, girin **ImageId** silmek için görüntünün. 

Bizim örneğimizde liste kimliğidir **58953**, değeri **ContentSource**. Kimliktir görüntü **59021**, değeri **ContentIds**.

4.  Abonelik anahtarınızı girin ve ardından **Gönder**.

5.  Görüntü silinip silinmediğini doğrulamak için kullanılan **Image - tüm resim kimlikleri alma** konsol.
 
## <a name="change-list-information"></a>Liste bilgilerini değiştirme

Listenin adını ve açıklamasını düzenleyin ve meta veri öğeleri ekleyin.

1.  İçinde [görüntü listesi Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), soldaki menüde seçin **görüntü listeleri**ve ardından **Güncelleştirme ayrıntıları**. 

  **Görüntü listeleri - Güncelleştirme ayrıntıları** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin.  

    ![Görüntü listeleri - güncelleştirme Ayrıntıları sayfası bölge seçimi](images/test-drive-region.png)

    **Görüntü listeleri - Güncelleştirme ayrıntıları** API konsolu açılır.
 
3.  İçinde **ListId** kutusuna liste kimliği girin ve sonra abonelik anahtarınızı girin.

4.  İçinde **istek gövdesinde** kutusunda yaptığınız düzenlemeleri yapın ve ardından **Gönder** sayfasında düğmesini.

  ![Görüntü listeleri - Güncelleştirme ayrıntıları konsol istek gövdesi düzenlemeleri](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Sonraki adımlar

REST API kodunuzdaki kullanın veya başlayın [görüntü listeleri .NET quickstart](image-lists-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
