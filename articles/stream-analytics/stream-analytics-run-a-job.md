---
title: "Akış başlatmak nasıl Stream Analytics işleri | Microsoft Docs"
description: "İş akışında Azure Stream Analytics çalışma şeklini | yol kesimi öğrenme."
keywords: "Akış işi"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: fb084f1c6b53e2582849e71271e8114a22dcf05c
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Azure Stream Analytics akış işi çalıştırma
Ne zaman bir giriş işi, sorgu ve çıkış tüm Stream Analytics işi başlatabilirsiniz belirtilmedi.

İşinizi başlatmak için:

1. Klasik Azure portalında, iş panosundan tıklatın **Başlat** sayfanın sonundaki.
   
   ![Başlangıç iş düğmesi](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   Azure portalında tıklatın **Başlat** sayfanın üst kısmındaki, iş.
   
   ![Azure portal başlangıç işi düğmesi](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Belirtin bir **Başlat çıktı** çıkış üretmeyi bu işin ne zaman başlatılır belirlemek için değeri. Daha önce başlatılmamış işleri için varsayılan ayardır **iş başlangıç saati**, yani iş veriler işlenirken hemen başlar. Ayrıca belirtebilirsiniz bir **özel** zaman geçmiş (için geçmiş verileri tüketme) veya gelecekteki (gelecekteki bir tarih kadar işleme gecikmesi için). Bir iş önceden başlatıldığında ve durdurulduğunda, seçeneği durumlarda **son durdurulma zamanı** son çıktı zamandan işi sürdürme ve veri kaybını önlemek için kullanılabilir.  

> [!NOTE]
> Bölümler kullanırken, son durdurulma zamanı tüm bölümler en son çıktı süreyi temsil eder.
   
   ![Zaman iş akışı Başlat](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portal başlangıç akış işi zaman](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Seçiminizi onaylayın. İş durumunu değiştirir *başlangıç* ve kısa süre içinde taşır *çalıştıran* işi başladıktan sonra. İlerleme durumunu izleyebilirsiniz **Başlat** işleminde **bildirim hub'ı**:
   
   ![Akış işi ilerleme durumu](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![İşin ilerleme durumunu akış azure portalı](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

