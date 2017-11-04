---
title: "Azure Stream Analytics girişleri örnekleme | Microsoft Docs"
description: "Akış analizi işleri giderirken sorunları belirlemenize."
keywords: "Giriş, giriş örnekleme sorun giderme"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: db9686fad7153d63fc659869b93821200e08397c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a>Azure Stream Analytics Giriş akışı örnekleme

Azure akış analizi kullanarak, bir dosyadan dönün ve sorguları başlatmak veya bir işi durdurmak zorunda kalmadan Portalı'nda test giriş olayları örnek oluşturabilirsiniz.

## <a name="testing-your-query"></a>Sorgunuz test etme

Akış analizi işi Ayrıntılar bölmesinde açmak **sorgu Düzenleyicisi'ni** sorgu adı altında dikey **sorgu**. (Bizim örnek senaryoda, sorgu henüz oluşturulduğundan tıklatın **< >** yer tutucu.)

![Stream Analytics sorgu Düzenleyicisi](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

Sorgunuzu oluşturmak için zengin bir düzenleyici dikey penceresinde önceki sürümünde olduğu gibi görüntülenir. Şimdi dikey giriş ve sorgu tarafından kullanılan ve bu proje için tanımlanan çıkış gösteren yeni bir sol bölmesi ile güncelleştirilmiştir.

![Stream Analytics sorgu Düzenleyicisi'ni girdilerin ve listeleri çıkarır](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

Ayrıca gösterilen bir ek giriş ve çıkış, tanımlanmamış'dır. İle başlayan yeni sorgu şablonu geliyor. Bunlar değiştirmek veya bile sorgu düzenlerken tamamen kaybolur. Bunları şu an için güvenle yoksayabilirsiniz.

Örnek giriş verilerle sınamak için girişlerinizi hiçbirini sağ tıklayın ve ardından **dosyasından örnek verileri yükleme**.

![Stream Analytics sorgu Düzenleyicisi'ni karşıya yükleme örnek verilerden dosya komutu](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

Yükleme tamamlandıktan sonra tıklayın **Test** bu sorgu yalnızca sağlanan örnek verileri karşı test etmek için.

![Stream Analytics sorgu Düzenleyicisi'ni Test düğmesi](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

Daha sonra kullanmak üzere çıktı testini kaydetmek istiyorsanız, sorgunuzun çıktısı indirme sonuçlarını bağlantı tarayıcısında görüntülenir. Şimdi kolayca ve yinelemeli olarak Sorgunuzu değiştirin ve tekrar tekrar çıkış nasıl değiştiğini görmek için test kullanabilirsiniz.

![Stream Analytics sorgu Düzenleyicisi'ni örnek çıkış](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

Önceki görüntüde, ikinci bir çıkış eklendi, adlı **HighAvgTempOutput**.

Sorguda birden çok çıkış kullandığınızda, ayrı ayrı her çıktı için sonuçları görmek ve kolayca aralarında geçiş.

Sonuçlardan memnun kaldığınızda, sorgunuzu kaydedebilir, işinizi başlatmak, arkanıza yaslanın ve akış analizi Sihirli izleyin.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
