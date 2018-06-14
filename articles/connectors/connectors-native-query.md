---
title: Logic apps içinde sorgu eylemi ekleme | Microsoft Docs
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
ms.openlocfilehash: 05dd4ae3c4ee439d66401a3f5595f9104051f8ee
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27962654"
---
# <a name="get-started-with-the-query-action"></a>Sorgu eylemi ile çalışmaya başlama
Sorgu eylemini kullanarak, toplu işleri ve iş akışları için gerçekleştirmek için diziler ile çalışabilirsiniz:

* Tüm yüksek öncelikli kayıtları için bir görev veritabanından oluşturun.
* Bir Azure blob e-postalara tüm PDF ekleri kaydedin.

Sorgu eylemi bir mantıksal uygulama kullanmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-query-action"></a>Sorgu eylemi kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](connectors-overview.md).  

Sorgu eylemi, şu anda Tasarımcısı'nda gösterilen filtre dizisi adlı bir işlem yok. Bu, bir dizi sorgulamak ve filtrelenmiş sonuç kümesi dönmek sağlar.

İşte nasıl bir mantıksal uygulama ekleyebilirsiniz:

1. Seçin **yeni adım** düğmesi.
2. Seçin **Eylem Ekle**.
3. Eylem arama kutusuna yazın **filtre** listesine **filtre dizisi** eylem.
   
    ![Sorgu eylemi seçin](./media/connectors-native-query/using-action-1.png)
4. Filtre uygulamak için bir dizi seçin. (Aşağıdaki ekran görüntüsünde bir Twitter Arama sonuçlarından dizisi gösterir.)
5. Her bir öğede değerlendirmek için bir koşul oluşturun. (Aşağıdaki ekran görüntüsünde tweet'leri 100'den fazla followers olan kullanıcılardan filtreler.)
   
    ![Sorgu eylemi tamamlamak](./media/connectors-native-query/using-action-2.png)
   
    Eylem filtresi gereksinimlerini karşılaması sonuçlarını içeren yeni bir dizi çıkarır.
6. Kaydetmek için araç sol üst köşesindeki tıklayın ve mantıksal uygulamanızı kaydetmek yayımlama hem (etkinleştirin).

\*Bir HTTP uç noktası çağrılmadan ve JSON yanıtını alma kullanın _ayrıştırma JSON_ eylem JSON yanıtı ayrıştırılamadı. Bu adım olmadan _filtre dizisi_ yalnızca gövde görebilir ve JSON yükü yapısını anlamak değil.

## <a name="query-action"></a>Sorgu eylemi
Aşağıda, bu bağlayıcıyı destekler eylemi için Ayrıntılar verilmiştir. Bağlayıcısı bir olası eylem vardır.

| Eylem | Açıklama |
| --- | --- |
| Diziyi filtrele |Dizideki her öğe için bir koşulu değerlendirir ve sonuçları döndürür |

## <a name="action-details"></a>Eylem ayrıntıları
Sorgu eylemi bir olası eylemiyle birlikte gelir. Aşağıdaki tablolar, eylem ve eylem kullanımıyla ilişkili karşılık gelen çıkış ayrıntıları için gerekli ve isteğe bağlı giriş alanlarının açıklamaktadır.

### <a name="filter-array"></a>Diziyi filtrele
HTTP giden isteğinde eylemi için girdi alanlarının verilmiştir.
A * gerekli bir alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Gelen * |başlangıç |Dizinin filtre uygulamak için |
| Koşul * |Burada |Her öğe için değerlendirmek için koşulun |

<br>

### <a name="output-details"></a>Çıkış ayrıntıları
HTTP yanıtı için çıkış ayrıntıları verilmiştir.

| Özellik adı | Veri türü | Açıklama |
| --- | --- | --- |
| Filtrelenmiş dizisi |array |Her filtre uygulanmış bir sonucu için bir nesne içeren bir dizi |

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, platform deneyin ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic apps diğer kullanılabilir bağlayıcılar bakarak keşfedebilirsiniz bizim [API'leri listesi](apis-list.md).

