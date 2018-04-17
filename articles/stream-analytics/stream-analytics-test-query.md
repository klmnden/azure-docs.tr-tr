---
title: Azure Stream Analytics işiniz örnek verilerle test | Microsoft Docs
description: Stream Analytics işlerine sorgularınızı test etme.
keywords: test bir işi, giriş örnekleme, örnek tarih karşıya yükle
documentationcenter: ''
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: stream-analytics
ms.workload: data-services
ms.date: 03/18/2018
ms.author: sngun
ms.openlocfilehash: c026a91fff5b8ef5774993b335f8d61877aa5d39
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="test-your-stream-analytics-query-with-sample-data"></a>Stream Analytics sorgunuzu örnek verilerle test

Azure akış analizi kullanarak örnek veri ve test sorguları portalında başlayan veya durdurulan bir işi olmadan yükleyebilirsiniz.

## <a name="upload-sample-data-and-test-the-query"></a>Örnek verileri yüklemek ve sorgu test etme

1. Var olan Stream Analytics işiniz birine gidin > tıklayın **sorgu** sorgu düzenleyici penceresini açın. 

2. Sorgunuzu örnek giriş verilerle sınamak için herhangi bir girişlerinizi üzerinde sağ tıklayın ve ardından **dosyasından örnek verileri yükleme**. Şu anda yalnızca biçimlendirilmiş JSON verilerini karşıya yükleyebilir. Verilerinizi CSV gibi farklı bir biçimde ise, karşıya yüklemeden önce şu JSON biçiminde seri hale bunu dönüştürmeniz. Herhangi bir açık kaynaklı dönüştürme aracı gibi kullanabilir [JSON Dönüştürücüsü CSV'ye](http://www.convertcsv.com/csv-to-json.htm) için JSON, verilerinizi dönüştürmek için.

    ![Stream analytics sorgu Düzenleyicisi'ni test sorgusu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

3. Yükleme tamamlandıktan sonra tıklayın **Test** bu sorguyu sağladığınız örnek verileri karşı test etmek için.

    ![Stream analytics sorgu Düzenleyicisi'ni test örnek veriler](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

4. Daha sonra kullanmak üzere çıktı testini kaydetmek istiyorsanız, sorgunuzun çıktısı indirme sonuçlarını bağlantı tarayıcısında görüntülenir. Şimdi kolayca ve yinelemeli olarak Sorgunuzu değiştirin ve tekrar tekrar çıkış nasıl değiştiğini görmek için test kullanabilirsiniz.

   ![Stream Analytics sorgu Düzenleyicisi'ni örnek çıkış](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Sorguda birden çok çıkış kullandığınızda, ayrı ayrı her çıktı için sonuçları görmek ve kolayca aralarında geçiş. Sorgunuzdaki kaydedebilirsiniz tarayıcıda gösterilen sonuçları işini başlatmak ve izin doğruladıktan sonra işlem hatasız olaylar.
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
