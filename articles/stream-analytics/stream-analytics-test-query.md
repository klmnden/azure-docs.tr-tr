---
title: Test örnek verilerle bir Azure Stream Analytics işi
description: Bu makalede, test bir Azure Stream Analytics işi, örnek giriş ve örnek verileri karşıya yükleme için Azure portalını kullanmayı açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 6/21/2019
ms.custom: seodec18
ms.openlocfilehash: ad0e0ca75bf3d3a8d9d1029d42f8609b3c4c627b
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620820"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Örnek verilerle bir Stream Analytics sorgu testi

Azure Stream Analytics kullanarak örnek giriş verileri veya sorguları Azure Portalı'nda başlangıç veya bir işi durduruluyor test etmek için örnek verileri karşıya yükleme.

## <a name="upload-or-sample-data-from-a-live-source-to-test-the-query"></a>Karşıya yükleme veya sorguyu test etmek için Canlı bir kaynaktan örnek veriler

1. Azure Portal’da oturum açın. 

2. Var olan Stream Analytics işinizi bulun ve seçin.

3. Sayfasında, üzerinde Stream Analytics işi, altında **iş topolojisi** başlığı seçin **sorgu** sorgu Düzenleyicisi penceresini açın. 

4. Sorgunuz test etmek için ardından ya da bir canlı girdi veya bir dosyadan karşıya veri örnekleme yapabilirsiniz. Veri, JSON, CSV veya AVRO seri hale getirilmelidir. Örnek giriş UTF-8 olarak kodlanmış ve sıkıştırılmaz. Yalnızca virgül (,) sınırlayıcı CSV giriş portalında test etme için desteklenir.

    1. Canlı girişini kullanarak: girişlerinizi birini sağ tıklayın. Ardından **örnek giriş verileri**. Sonraki ekranda, örnek süresini ayarlayabilirsiniz. Örneklenen verileri belirtilen tam zaman aralığını temsil etmeyebilir için örnekleme olaylarını canlı bir kaynaktan gelen olayları veya 1 MB (hangisi önce gerçekleşirse), 1000 adede kadar alın.

    1. Dosyasıyla: girişlerinizi birini sağ tıklayın. Ardından **örnek verileri dosyadan karşıya**. 

    ![Stream analytics sorgu Düzenleyicisi'ni test sorgusu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Örnekleme veya karşıya yükleme tamamlandıktan sonra seçin **Test** bu sorguyu sağladığınız örnek verilere karşı test etmek için.

    ![Stream analytics sorgu Düzenleyicisi'ni test örnek veriler](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Daha sonra kullanmak üzere test çıkışı ihtiyacınız varsa, sorgu çıktısı indirme sonuçlarını bağlantısını içeren bir tarayıcıda görüntülenir. 

7. Yinelemeli olarak Sorgunuzu değiştirin ve tekrar çıkışın nasıl değiştiğini görmek için test edin.

   ![Stream Analytics sorgu Düzenleyicisi örnek çıktı](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Bir sorguda birden çok çıktı kullandığınızda, sonuçlar ayrı sekmelerde görüntülenir ve bunlar arasında kolayca çevirebilirsiniz.

8. Tarayıcıda gösterilen sonuçları doğruladıktan sonra **Kaydet** sorgunuzu. Ardından **Başlat** işi ve onu gelen olayları işlemeye olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
