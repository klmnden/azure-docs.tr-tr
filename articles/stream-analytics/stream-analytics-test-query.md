---
title: Azure Stream Analytics test sorguları
description: Bu makalede, Stream Analytics işlerini örnek veri dosyalarını kullanarak sorguları test etme açıklanmaktadır.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: fb7d936dfdc543c208ce98c588f5ad83704ff5dc
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>Azure portalında Azure akış analizi sorgu testi

Azure Stream Analytics ile başlatmak veya bir işi durdurmak zorunda kalmadan sorguları Azure portalında test edebilirsiniz.

## <a name="test-the-input"></a>Giriş test

1. Örnek giriş verilerle sınamak için girişlerinizi hiçbirini sağ tıklayın ve ardından **dosyasından örnek verileri yükleme**. Şu anda yalnızca biçimlendirilmiş JSON verilerini karşıya yükleyebilir. Verilerinizi CSV gibi farklı bir biçimde ise, karşıya yüklemeden önce şu JSON biçiminde seri hale bunu dönüştürmeniz. Herhangi bir açık kaynaklı dönüştürme aracı gibi kullanabilir [JSON Dönüştürücüsü CSV'ye](http://www.convertcsv.com/csv-to-json.htm) için JSON, verilerinizi dönüştürmek için.

    ![stream analytics query editor test query](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Yükleme tamamlandıktan sonra tıklayın **Test** bu sorguyu sağladığınız örnek verileri karşı test etmek için.

    ![Stream analytics sorgu Düzenleyicisi'ni test örnek veriler](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

Sorgunuzun çıktısı, daha sonra kullanmak için test çıkışı kaydetmek istediğiniz tarayıcıda, indirme sonuçlarını bağlantıyla görüntülenir. Şimdi kolayca ve yinelemeli olarak Sorgunuzu değiştirin ve tekrar tekrar çıkış nasıl değiştiğini görmek için test kullanabilirsiniz.

![Stream Analytics sorgu Düzenleyicisi'ni örnek çıkış](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Sorguda kullanılan birden fazla çıkış ile ayrı ayrı her iki çıktıları için sonuçları görmek ve kolayca aralarında geçiş.

Sorgunuzu kaydetmek, işinizi başlatmak ve izin tarayıcıda gösterilen Sonuçlardan memnun sonra bu işlemi hata olmayan olaylar.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
