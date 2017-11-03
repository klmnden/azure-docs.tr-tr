---
title: "Akış analizi için veri analizi işlem işi oluşturma | Microsoft Docs"
description: "Akış analizi için veri analizi işlem işi oluşturma | yol kesimi öğrenme."
keywords: "veri analizi işleme"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 05fdf1e20efd129cdfc27e1d37bc9e124edf5dcd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Akış analizi için veri analizi işlem işi oluşturma
Üst düzey Azure akış analizi, akış analizi işi kaynaktır.  Bir veya daha fazla giriş veri kaynağı, veri dönüştürme ifade bir sorgu ve sonuçları yazılan bir veya daha fazla çıkış hedefleri oluşur. Birlikte bu veri analitik veri senaryoları akış için işleme gerçekleştirmek kullanıcının etkinleştirir.

Stream Analytics kullanmaya başlamak için yeni bir akış analizi işi oluşturarak başlayın.  İş başlatılana kadar bu eylem hiçbir fatura şifrelemelerini unutmayın.

1. Online'da oturum [Klasik Azure portalı](http://manage.windowsazure.com) veya [Azure portal](https://portal.azure.com/).
2. Portalda: **Yeni'yi**, ardından **Veri Hizmetleri** veya **veri analizi** portal ve ardından bağlı olarak **Azure akış analizi**veya **akış analizi** ve ardından **hızlı Oluştur**.
   
   ![Veri analizi işleme işi Sihirbazı](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![İş işleme veri analizi oluşturma](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Stream Analytics işi için istenen yapılandırmayı belirtin.
   
   * İçinde **iş adı** kutusuna, Stream Analytics işi tanımlamak için bir ad girin. Zaman **iş adı** doğrulandı, iş adı kutusunda yeşil bir onay işareti görünür. **İş adı** yalnızca alfasayısal karakterler içerebilir ve '-' karakteri ve 3 ile 63 karakter arasında olmalıdır.
   * Kullanım **bölge** Azure portalında veya **konumu** , işi çalıştırmak istediğiniz coğrafi konumu belirtmek için Azure Portalı'nda.
   * Azure portalını kullanıyorsanız, seçin veya olarak kullanılmak üzere depolama hesabı oluşturma **bölgesel izleme depolama hesabı**. Bu depolama hesabı, bu bölgede çalışan tüm Stream Analytics işleri izleme verilerini depolamak için kullanılır.
   * Azure Portalı'nı kullanarak, yeni bir belirtin veya varolan **kaynak grubu** uygulamanız için ilgili kaynakları tutmak için.
4. Yeni akış analizi işi seçenekleri yapılandırıldıktan sonra tıklatın **Stream Analytics işi oluşturmak**. Akış analizi işinin oluşturulması birkaç dakika sürebilir. Durumu denetlemek için bildirim hub'ında ilerlemesini izleyebilirsiniz.
   
   ![Veri analizi işleme iş bildirimler hub'ı](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Azure portal veri analitik işleme işi oluştur işi](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. Yeni iş durumunu gösterecektir **oluşturulan**. Dikkat **Başlat** düğmesi devre dışıdır. İş başlamadan önce iş girişi, sorgu ve çıkış yapılandırın.
   
   ![Veri analizi işleme iş durumu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portal veri analitik iş durumunu işleme](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

