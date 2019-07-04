---
title: Azure zaman serisi görüşleri ortamınıza başvuru veri kümesi ekleme | Microsoft Docs
description: Bu makalede, Azure zaman serisi görüşleri ortamınıza veri genişletmek için bir başvuru veri kümesi eklemeyi açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/26/2019
ms.custom: seodec18
ms.openlocfilehash: 99933fa36cc822598ec9c173a470f90264d06d54
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461206"
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Azure portalını kullanarak zaman serisi görüşleri ortamınıza başvuru veri kümesi oluşturma

Bu makalede, Azure zaman serisi görüşleri ortamınıza başvuru veri kümesi ekleme açıklar. Başvuru veri değerleri genişletmek için kaynak verilerinizi birleştirmek yararlıdır.

Başvuru veri kümesi, olaylar, olay kaynağınızdan büyütmek öğeleri koleksiyonudur. Zaman serisi görüşleri giriş altyapısı, başvuru veri kümenizdeki her olay, olay kaynağınızdan karşılık gelen veri satırı ile birleştirir. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu birleşim, başvuru veri kümenizde tanımlı birincil anahtar sütunları dayanır.

Başvuru verileri geriye dönük olarak birleştirilmedi. Bu nedenle, yalnızca mevcut ve gelecekteki giriş verileri eşleşen ve yapılandırılmış ve karşıya sonra başvuru tarih kümesine katılmış.

## <a name="video"></a>Video

### <a name="learn-about-time-series-insights-reference-data-modelbr"></a>Zaman serisi görüşleri'nin başvuru veri modeli hakkında bilgi edinin.</br>

> [!VIDEO https://www.youtube.com/embed/Z0NuWQUMv1o]

## <a name="add-a-reference-data-set"></a>Başvuru veri kümesi Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Mevcut Time Series Insights ortamınızı bulun. Seçin **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

1. Seçin **genel bakış** sayfası. Bulun **Time Series Insights Gezgini URL** ve bağlantıyı açın.  

   TSI ortamınız için Gezgini görüntüleyin.

1. Ortam Seçici TSI Gezgininde genişletin. Etkin bir ortam seçin. Başvuru veri simgesi sağ üst köşedeki Gezgini sayfasında seçin.

   [![Başvuru veri ekleme](media/add-reference-data-set/add-reference-data.png)](media/add-reference-data-set/add-reference-data.png#lightbox)

1. Seçin **+ bir veri kümesi Ekle** yeni veri kümesi eklemeye başlamak için düğme.

   [![Veri kümesi Ekle](media/add-reference-data-set/add-data-set.png)](media/add-reference-data-set/add-data-set.png#lightbox)

1. Üzerinde **yeni başvuru veri kümesi** sayfasında, verilerin biçimini seçin:
   - Seçin **CSV** virgülle ayrılmış veri için. İlk satırı üst bilgi satırı kabul edilir.
   - Seçin **JSON dizisi** için javascript nesne gösterimi (JSON) biçimlendirilmiş verileri.

   [![Veri biçimi seçin.](media/add-reference-data-set/add-data.png)](media/add-reference-data-set/add-data.png#lightbox)

1. İki yöntemden birini kullanarak verileri sağlayın:
   - Verileri bir metin düzenleyiciye yapıştırın. Ardından, **ayrıştırma başvuru verilerini** düğmesi.
   - Seçin **Dosya Seç** düğmesini bir yerel metin dosyasından veri ekleyin.

   Örneğin, CSV veri yapıştırın: [![Yapıştırılan CSV verileri](media/add-reference-data-set/csv-data-pasted.png)](media/add-reference-data-set/csv-data-pasted.png#lightbox)

   Örneğin, JSON dizisi veri yapıştırın: [![JSON verilerini yapıştırın](media/add-reference-data-set/json-data-pasted.png)](media/add-reference-data-set/json-data-pasted.png#lightbox)

   Veri değerleri ayrıştırılırken bir hata varsa, hata sayfanın alt kısmındaki kırmızı gibi görünür `CSV parsing error, no rows extracted`.

1. Veri başarıyla ayrıştırıldı sonra veri kılavuzu sütunları ve verileri temsil eden satırlar görüntüleme gösterilir.  Doğruluk emin olmak için veri kılavuzu gözden geçirin.

   [![Başvuru veri ekleme](media/add-reference-data-set/parse-data.png)](media/add-reference-data-set/parse-data.png#lightbox)

1. Her bir sütunun veri türü varsayıldı görmek için gözden geçirin ve gerekirse veri türünü değiştirin.  Sütun başlığı veri türü simgesini seçin: **#** çift (sayısal veriler) için **T | F** Boole için veya **Abc** dizesi.

   [![Veri türlerinde sütun başlıklarını seçin.](media/add-reference-data-set/choose-datatypes.png)](media/add-reference-data-set/choose-datatypes.png#lightbox)

1. Sütun başlıkları, gerekirse yeniden adlandırın. Olay kaynağınızı karşılık gelen özelliğine katılmayı anahtar sütun adı gereklidir. Başvuru veri anahtar sütun adlarını büyük küçük harf duyarlılığı dahil olmak üzere verilerinize gelen olay adı için tam olarak eşleştiğinden emin olun. Anahtar olmayan sütun adları, gelen veri karşılık gelen başvuru veri değerleri ile genişletmek için kullanılır.

1. Seçin **bir satır ekleyin** veya **sütun ekleme** gerektiğinde daha fazla başvuru veri değerleri eklemek için.

1. Bir değer yazın **satırları filtrele...**  alanı gerektiği gibi belirli satırlar gözden geçirin. Filtre verileri gözden geçirmek için yararlıdır, ancak veriler karşıya yüklenirken uygulanmaz.

1. Veri kümesi doldurarak ad **veri kümesi adı** veri kılavuzu yukarıda alan.

    [![Veri kümesi adı.](media/add-reference-data-set/name-reference-dataset.png)](media/add-reference-data-set/name-reference-dataset.png#lightbox)

1. Sağlamak **birincil anahtar** veri kılavuzu Yukarıdaki açılan seçerek veri kümesinde sütun.

    [![Anahtar sütunları seçin.](media/add-reference-data-set/set-primary-key.png)](media/add-reference-data-set/set-primary-key.png#lightbox)

    İsteğe bağlı olarak **+** düğmesini bileşik bir birincil anahtar olarak ikincil bir anahtar sütunu ekleyin. Seçimi geri almanız gerekiyorsa, boş değer ikincil anahtarı'nı kaldırmak için açılan listeden seçin.

1. Verileri yüklemek için seçin **karşıya satırları** düğmesi.

    [![Karşıya yükleme](media/add-reference-data-set/upload-rows.png)](media/add-reference-data-set/upload-rows.png#lightbox)

    Tamamlanan bir karşıya yükleme ve iletisini görüntüler sayfa onaylar **başarıyla veri kümesi'ni karşıya**.

## <a name="next-steps"></a>Sonraki adımlar

* [Başvuru verilerini programlamayla yönetin](time-series-insights-manage-reference-data-csharp.md).

* Tam API başvurusu için [Başvuru Verileri API'si](/rest/api/time-series-insights/ga-reference-data-api) belgesine bakın.
