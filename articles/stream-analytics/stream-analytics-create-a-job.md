---
title: Akış analizi için veri analizi işlem işi oluşturma | Microsoft Docs
description: Akış analizi için veri analizi işlem işi oluşturma | yol kesimi öğrenme.
keywords: veri analizi işleme
documentationcenter: ''
services: stream-analytics
author: jseb225
manager: ryanw
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeanb
ms.openlocfilehash: e332651af29514ca773b1476eafb0381207df86e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Akış analizi için veri analizi işlem işi oluşturma
Üst düzey Azure akış analizi, akış analizi işi kaynaktır.  Bir veya daha fazla giriş veri kaynağı, veri dönüştürme ifade bir sorgu ve sonuçları yazılan bir veya daha fazla çıkış hedefleri oluşur. Birlikte bu veri analitik veri senaryoları akış için işleme gerçekleştirmek kullanıcının etkinleştirir.

Stream Analytics kullanmaya başlamak için yeni bir akış analizi işi oluşturarak başlayın.  İş başlatılana kadar bu eylem hiçbir fatura şifrelemelerini unutmayın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Seçin **kaynak oluşturma** > **veri + analiz** > **Stream Analytics işi**.
3. **Oluştur**’u seçin.
   
3. Stream Analytics işi için istenen yapılandırmayı belirtin.
   
   * İçinde **iş adı** kutusuna, Stream Analytics işi tanımlamak için bir ad girin. Zaman **iş adı** doğrulandı, iş adı kutusunda yeşil bir onay işareti görünür. **İş adı** yalnızca alfasayısal karakterler içerebilir ve '-' karakteri ve 3 ile 63 karakter arasında olmalıdır.
   * Kullanım **konumu** , işi çalıştırmak istediğiniz coğrafi konumu belirtmek için.
   * Yeni bir belirtin veya varolan **kaynak grubu** uygulamanız için ilgili kaynakları tutmak için.
4. **Oluştur**’u seçin.
Akış analizi işinin oluşturulması birkaç dakika sürebilir. Durumu denetlemek için bildirim hub'ında ilerlemesini izleyebilirsiniz.
    
   ![Azure portal veri analitik işleme işi oluştur işi](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. Yeni iş durumunu gösterecektir **oluşturulan**. Dikkat **Başlat** düğmesi devre dışıdır. İş başlamadan önce iş girişi, sorgu ve çıkış yapılandırın.

   
   ![Azure portal veri analitik iş durumunu işleme](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

