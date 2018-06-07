---
title: Azure zaman serisi Öngörüler ortamınız için bir başvuru veri kümesi ekleme
description: Bu makalede, Azure zaman serisi Öngörüler ortamınızda veri büyütmek için bir başvuru veri kümesi eklemeyi açıklar.
ms.service: time-series-insights
services: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/15/2018
ms.openlocfilehash: 7da2393bb5114de20747581e366a8f416c9ff9a4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34653646"
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Azure Portalı'nı kullanarak, zaman serisinin Öngörüler ortamınız için bir başvuru veri kümesi oluşturma

Bu makalede, Azure zaman serisi Öngörüler ortamınız için bir başvuru veri kümesi eklemeyi açıklar. Başvuru verileri değerleri genişletmek için kaynak verilerinizi katılmak kullanışlıdır.

Bir başvuru veri kümesi, olay kaynağı olayların büyütmek öğelerin koleksiyonudur. Zaman serisi Öngörüler giriş altyapısı başvuru Veri kümenizi her olay Olay kaynağınızdan karşılık gelen veri satırı ile birleştirir. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu katılımı başvuru veri kümesinde tanımlanan birincil anahtar sütunları temel alır.

Başvuru verileri firmalarda geriye dönük birleştirilmedi. Bu, yalnızca geçerli ve gelecekteki giriş verileri eşleşen ve yapılandırılmış ve karşıya sonra başvuru tarih kümesine birleşik anlamına gelir.

## <a name="add-a-reference-data-set"></a>Bir başvuru veri kümesi Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut zaman serisi Öngörüler ortamınızın bulun. Tıklatın **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Seçin **genel bakış** sayfası. Bulun **zaman serisi Öngörüler Gezgini URL'si** ve bağlantıyı açın.  

   TSI ortamınız için explorer görüntüleyin.

4. Ortam Seçici TSI Explorer'da genişletin. Etkin ortam seçin. Başvuru veri simgesine sağ üst explorer sayfasını seçin.

   ![Başvuru veri ekleme](media/add-reference-data-set/add_reference_data.png)

5. Seçin **+ bir veri kümesi Ekle** yeni veri kümesi eklemeye başlamak için düğmesi.

   ![Veri kümesi Ekle](media/add-reference-data-set/add_data_set.png)

6. Üzerinde **yeni veri kümesi başvurusu** sayfasında, verilerin biçimini seçin: 
   - Seçin **CSV** virgülle ayrılmış veriler için. İlk satırı sütun başlığı kabul edilir. 
   - Seçin **JSON dizisi** için javascript nesne gösterimi (JSON) veri biçimlendirilmiş.

   ![Veri biçimi seçin.](media/add-reference-data-set/add_data.png)

7. İki yöntemden birini kullanarak bu verileri sağlar:
   - Veri metin düzenleyicisine yapıştırın. Ardından, seçin **ayrıştırma başvuru verileri** düğmesi.
   - Seçin **Dosya Seç** yerel metin dosyasından veri eklemek için düğmesi. 

   Örneğin, CSV Verileri yapıştırın: ![yapıştırılan CSV verileri](media/add-reference-data-set/csv_data_pasted.png)

   Örneğin, JSON dizi veri yapıştırın: ![JSON Yapıştır verileri](media/add-reference-data-set/json_data_pasted.png)

   Veri değeri ayrıştırılırken bir hata varsa, hata sayfasının alt kısmındaki kırmızı gibi görünür `CSV parsing error, no rows extracted`.

8. Verileri başarıyla ayrıştırılır sonra veri kılavuzu verileri temsil eden satırlar ve sütunlar görüntüleme gösterilir.  Veri Kılavuzu doğruluğundan emin olmak için gözden geçirin.

   ![Başvuru veri ekleme](media/add-reference-data-set/parse_data.png)

9. Veri türü kabul görmek için her bir sütunun gözden geçirin ve gerekirse veri türünü değiştirin.  Veri türü simgesi sütun başlığını seçin: **#** çift (sayısal veriler), için **T | F** boolean için veya **Abc** dize.

   ![Veri türlerinde sütun başlıkları seçin.](media/add-reference-data-set/choose_datatypes.png)

10. Sütun üstbilgileri gerekirse yeniden adlandırın. Olay kaynağı karşılık gelen özelliği katılmaya anahtar sütun adı gereklidir. Başvuru veri anahtar sütun adları büyük küçük harf duyarlılığı dahil olmak üzere verilerinize gelen olay adı için tam olarak eşleştiğinden emin olun. Anahtar olmayan sütun adları, ilgili başvuru veri değerleri ile gelen veri büyütmek için kullanılır.

11. Tıklatın **satır eklemek** veya **bir sütun ekleyin** gerektiği gibi birden fazla başvuru veri değeri eklemek için.

12. Bir değer yazın **satırları filtrele...**  alan gerektiği gibi belirli satırlar gözden geçirin. Filtre verileri gözden geçirmek için yararlıdır, ancak verileri karşıya yüklenirken uygulanmıyor.
 
13. Doldurarak veri kümesi adı **veri kümesi adı** veri kılavuzu yukarıda alan.

   ![Veri kümesi adı.](media/add-reference-data-set/name_reference_dataset.png)

14. Sağlamak **birincil anahtar** veri kılavuzu yukarıdaki açılır seçerek veri kümesinde sütun.

   ![Anahtar sütunları seçin.](media/add-reference-data-set/set_primary_key.png)

   İsteğe bağlı olarak, seçin **+** birleşik birincil anahtar olarak ikincil bir anahtar sütun eklemek için düğmesi. Seçimi geri almanız gerekiyorsa, boş değer ikincil anahtarı'nı kaldırmak için açılan listeden seçin.

15.  Verileri yüklemek için seçin **karşıya satırları** düğmesi.

   ![Karşıya Yükle](media/add-reference-data-set/upload_rows.png)

   Tamamlanan karşıya yükleme ve iletiyi görüntülemek sayfa onaylar **başarıyla veri kümesi'ni karşıya**.

## <a name="next-steps"></a>Sonraki adımlar
* [Başvuru verilerini programlamayla yönetin](time-series-insights-manage-reference-data-csharp.md).
* Tam API başvurusu için [Başvuru Verileri API'si](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) belgesine bakın.
