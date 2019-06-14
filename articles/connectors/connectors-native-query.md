---
title: Logic apps'te sorgu eylemi ekleyin | Microsoft Docs
description: Filtre dizisi gibi eylemleri gerçekleştirmek için sorgu eylemi genel bakış.
services: ''
documentationcenter: ''
author: jeffhollan
manager: erikre
editor: ''
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: 2a82afe396039857e5b9ad6b8a6d0e710573037f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60538252"
---
# <a name="get-started-with-the-query-action"></a>Sorgu eylemi ile çalışmaya başlama
Sorgu eylemi kullanarak, toplu işleri ve iş akışlarına gerçekleştirmek için dizileri ile çalışabilirsiniz:

* Tüm yüksek öncelikli kayıt için bir görev, bir veritabanından oluşturun.
* Bir Azure blob içinde e-postalar için tüm PDF ekleri kaydedin.

Bir mantıksal uygulama çalıştırmasında sorgu eylemi ile çalışmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-query-action"></a>Sorgu eylemi kullanın
Bir eylem mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. 
[Eylemler hakkında daha fazla bilgi](../connectors/apis-list.md).  

Sorgu eylemi şu anda Tasarımcısı'nda sunulan filtre dizisi olarak adlandırılan tek bir işlem yok. Bu, bir dizi sorgu ve bir filtre uygulanmış bir sonuç kümesinin dönmesini sağlar.

İşte nasıl bir mantıksal uygulama çalıştırmasında ekleyebilirsiniz:

1. Seçin **yeni adım** düğmesi.
2. Seçin **Eylem Ekle**.
3. Eylem arama kutusuna **filtre** listesine **filtre dizisi** eylem.
   
    ![Sorgu eylemi seçin](./media/connectors-native-query/using-action-1.png)
4. Filtre uygulamak için bir dizi seçin. (Aşağıdaki ekran görüntüsünde bir Twitter arama sonuçlarını dizisini gösterir.)
5. Her bir öğede değerlendirmek için bir koşul oluşturun. (Aşağıdaki ekran görüntüsünde, 100'den fazla Takipçisi olan kullanıcılardan gelen tweetleri filtreler.)
   
    ![Sorgu eylemi tamamlayın](./media/connectors-native-query/using-action-2.png)
   
    Eylem filtresi gereksinimleri karşılayan sonuçları içeren yeni bir dizi çıkarır.
6. Kaydetmek için araç çubuğunun sol üst köşesindeki tıklayın ve mantıksal uygulamanızı kaydetmek yayımlama hem (etkinleştir).

\* Bir HTTP uç noktasını çağırmak ve bir JSON yanıtı alma, kullanmanız _JSON Ayrıştır_ eylemi JSON yanıtı ayrıştıramadı. Bu adım olmadan _filtre dizisi_ yalnızca gövdesi görebilir ve JSON yükü yapısını anlamak değil.

## <a name="query-action"></a>Sorgu eylemi
Bu bağlayıcıyı destekler eylemini ilgili ayrıntıları aşağıda verilmiştir. Bağlayıcısı, olası bir eylem vardır.

| Eylem | Açıklama |
| --- | --- |
| Diziyi filtreleme |Bir dizideki her öğe için bir koşulu değerlendirir ve sonuçları döndürür |

## <a name="action-details"></a>Eylem ayrıntıları
Sorgu eylemi olası tek bir eylem ile birlikte gelir. Aşağıdaki tablolar, gerekli ve isteğe bağlı giriş alanları için eylem ve eylemini kullanarak ile ilişkilendirilmiş ilgili çıkış ayrıntılarını açıklar.

### <a name="filter-array"></a>Diziyi filtreleme
Giden HTTP isteği yapan eylem için giriş alanlarını verilmiştir.
A * gerekli alan olduğu anlamına gelir.

| Display name | Özellik adı | Açıklama |
| --- | --- | --- |
| Gelen * |from |Filtrelenecek dizi |
| Koşul * |Burada |Her öğe için değerlendirilecek koşulu |

<br>

### <a name="output-details"></a>Çıkış Ayrıntıları
HTTP yanıtının çıkış ayrıntılarını aşağıda verilmiştir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Filtrelenmiş bir dizi |array |Her filtre uygulanmış bir sonuç için bir nesne içeren bir dizi |

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, platformu deneyin ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Diğer bağlayıcıları logic apps'teki bakarak keşfedebilirsiniz bizim [API listesi](apis-list.md).

