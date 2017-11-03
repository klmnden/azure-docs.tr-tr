---
title: Azure Stream Analytics sorgu testi | Microsoft Docs
description: "Stream Analytics işlerine sorgularınızı test etme."
keywords: "Sorguyu sınamak, sorgu sorun giderme"
documentation center: 
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
ms.openlocfilehash: 5e7bab0b0c3222ba093a93dc2d15f1e41898e62c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>Azure portalında Azure akış analizi sorgu testi

Azure Stream Analytics ile başlatmak veya bir işi durdurmak zorunda kalmadan sorguları Azure portalında test edebilirsiniz.

## <a name="test-the-input"></a>Giriş test

1. Örnek giriş verilerle sınamak için girişlerinizi hiçbirini sağ tıklayın ve ardından **dosyasından örnek verileri yükleme**.

    ![Stream analytics sorgu Düzenleyicisi'ni test sorgusu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Yükleme tamamlandıktan sonra tıklayın **Test** bu sorguyu sağladığınız örnek verileri karşı test etmek için.

    ![Stream analytics sorgu Düzenleyicisi'ni test örnek veriler](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

Sorgunuzun çıktısı, daha sonra kullanmak için test çıkışı kaydetmek istediğiniz tarayıcıda, indirme sonuçlarını bağlantıyla görüntülenir. Şimdi kolayca ve yinelemeli olarak Sorgunuzu değiştirin ve tekrar tekrar çıkış nasıl değiştiğini görmek için test kullanabilirsiniz.

![Stream Analytics sorgu Düzenleyicisi'ni örnek çıkış](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Sorguda kullanılan birden fazla çıkış ile ayrı ayrı her iki çıktıları için sonuçları görmek ve kolayca aralarında geçiş.

Sorgunuzu kaydetmek, işinizi başlatmak ve izin tarayıcıda gösterilen Sonuçlardan memnun sonra bu işlemi hata olmayan olaylar.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
