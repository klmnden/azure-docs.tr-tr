---
title: "Stream Analytics içinde sorguları yazmaya | Microsoft Docs"
description: "Akış analizi ve sorgu veri sorguları yazma | yol kesimi öğrenme."
keywords: "nasıl yazılacağını sorgular, verileri sorgulama, sorgu yazmak için bir sorgu yazın"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 215b774c20d80a67b1cefa2634131bd44860c692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-write-queries-in-stream-analytics"></a>Stream Analytics nasıl yazılacağını sorgular
Akış işleme mantığı Azure akış analizi de sorgu yazma "bir iş başlatılır ve iş ulaştığında gibi verileri yürütülen önce tanımlı bir durumu sorgu" olarak uygulanır. Veri dönüştürme SQL benzeri bir sorgu dili ifade eklenen dil uzantıları gibi büyük ölçüde T-SQL bir alt bazı olduğu [Pencereleme](https://msdn.microsoft.com/library/azure/dn835019.aspx) zamana bağlı semantiği ifade etmek için kullanılır.

## <a name="writing-queries"></a>Sorgu yazma:
1. Azure Portalı'da, Stream Analytics işinde tıklatın **sorgu**.
   
    ![Seçme sorgusu](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    Azure portalında tıklatın **sorgu**.
   
    ![Sorgu Önizleme'yi seçin](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. Yeni işlerin başlamanıza yardımcı olması için bir sorgu şablonu vardır. Sorgu şablonu çıkış giriş olaylarından projeleri tüm alanları "geçiş" bir sorgu gerçekleştirir.  
   
   * İşiniz için en az bir giriş ve çıkış tanımladıysanız, "[YourOutputAlias]" yer tutucu değiştirebilir ve istediğiniz çıkış ve giriş diğer adları "[YourInputAlias]" alanlarla ilk kullanın. Ayrıca, hala yazar ve sorgunuzu Azure Klasik Portalı'nda girişleri ve çıkışları işinde tanımlamadan test kullanabilirsiniz.
   * Daha fazla işlem daha basit bir geçiş gerçekleştirmek istiyorsanız, sorgu tanımı düzenleyebilirsiniz. Sorgu yazma ile çalışmaya başlamak için bazı yaygın sorgusu desenleri yakalanır göz atın [burada](stream-analytics-stream-analytics-query-patterns.md).  
   
   ![Sorgu veri penceresi](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Sorgu verileri doğrulamak için çalışma:
Sorgunuz test verileri içeren bir veya daha fazla yerel JSON dosyaları tarayıcıda çalıştırarak beklendiği gibi davranır test edebilirsiniz. Bu iş başlatmaz veya herhangi fatura ödenmesi gerekebilir.

> [!NOTE]
> Şu anda tarayıcı içi sorgu testi Azure portalında desteklenmiyor.  
> 
> 

1. (Aksi halde Test düğmesi devre dışı bırakılır) sorguda herhangi bir hata olduğundan emin olun ve ardından Test düğmesine tıklayın.  
   
   ![Test verileri Sorgulama](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. Her sorguda başvurulan girdi dosyaları belirtmeniz istenir. Bu örnekte, şablon sorgu olarak kaldığını-iletişim "yourinputalias" adlı bir giriş istemeden, olduğundan.
   
   ![Test verileri sorgusu](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. Bir test dosyasına göz atın. Birkaç örnek dosyalarını kullanılabilir [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) ve kendi veri akışı girişlerini aracılığıyla örnek verileri işlevi giriş sekmesinde ndan örnek veri alabilirsiniz.
   
   ![Sorgu giriş](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. İletişim kutusunu kapattıktan sonra sorgunuzu test verileri üzerinde çalışacak ve sorgu sayfanın sonundaki sonuçları görürsünüz.
   
   ![Sorgu özeti](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

