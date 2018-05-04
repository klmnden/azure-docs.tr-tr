---
title: Test örnek verilerle bir Azure akış analizi işi
description: Stream Analytics işlerine sorgularınızı test etme.
keywords: Bu makalede Azure portalı bir Azure akış analizi işi, örnek giriş sınamak ve örnek verileri yüklemek için nasıl kullanılacağını açıklar.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: 3dc9091934f3db8ededc13f74d2f302eccace4d6
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Örnek verilerle bir akış analizi sorgu testi

Azure akış analizi kullanarak Azure portalında örnek veri ve test sorguları başlayan veya durdurulan bir işi olmadan yükleyebilirsiniz.

## <a name="upload-sample-data-and-test-the-query"></a>Örnek verileri yüklemek ve sorgu test etme

1. Azure Portal’da oturum açın. 

2. Var olan Stream Analytics işi bulun ve seçin.

3. Üzerinde Stream Analytics altında sayfası, proje **iş topoloji** başlığını seçin **sorgu** sorgu düzenleyici penceresini açın. 

4. Sorgunuz örnek giriş verilerle sınamak için herhangi bir girişlerinizi üzerinde sağ tıklayın.  Ardından **dosyasından örnek verileri yükleme**.

   Verileri yalnızca biçimlendirilmiş JSON verilerini olması gerekir. Verilerinizi CSV gibi farklı bir biçimde ise, karşıya yüklemeden önce şu JSON biçiminde seri hale bunu dönüştürmeniz. Herhangi bir açık kaynaklı dönüştürme aracı gibi kullanabilir [JSON Dönüştürücüsü CSV'ye](http://www.convertcsv.com/csv-to-json.htm) için JSON, verilerinizi dönüştürmek için.

    ![Stream analytics sorgu Düzenleyicisi'ni test sorgusu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Yükleme tamamlandıktan sonra Seç **Test** bu sorguyu sağladığınız örnek verileri karşı test etmek için.

    ![Stream analytics sorgu Düzenleyicisi'ni test örnek veriler](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Daha sonra kullanmak üzere test çıkışı gerekiyorsa, sorgunuzun çıktısı indirme sonuçlarını bağlantı tarayıcıda görüntülenir. 

7. Yinelemeli olarak Sorgunuzu değiştirin ve tekrar çıkış nasıl değiştiğini görmek için test edin.

   ![Stream Analytics sorgu Düzenleyicisi'ni örnek çıkış](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Sorguda birden çok çıkış kullandığınızda, sonuçları ayrı sekmelerde gösterilir ve bunlar arasında kolayca çevirebilirsiniz.

8. Tarayıcıda, gösterilen sonuçları doğruladıktan sonra **kaydetmek** sorgunuzu. Ardından **Başlat** iş ve onu gelen olayları işlemek izin verin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
