---
title: Akış analizi işleri için verilerin nasıl yapılandırılacağına çıkarır | Microsoft Docs
description: Çıkış akış analizi işleri için yapılandırma | yol kesimi öğrenme.
keywords: Çıkış, verileri veri taşıma
documentationcenter: ''
services: stream-analytics
author: jseb225
manager: ryanw
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/26/2017
ms.author: jeanb
ms.openlocfilehash: c7b6a519a8465221953a0f6fb20169431e229e30
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Akış analizi işleri için verilerin nasıl yapılandırılacağına çıkarır

Azure akış analizi işleri, bir bağlantı varolan bir veri havuzunu tanımlamak, bir veya daha fazla veri çıkışlarına bağlanabilir. Stream Analytics işiniz işler ve gelen verileri dönüştüren gibi veri çıkış olayları akışı, iş çıkışı yazılır.

Akış analizi veri çıkışları gerçek zamanlı panolar veya uyarıları, kaynak için kullanılabilir veri taşıma iş akışlarını tetikleyebilir ve yalnızca sonraki toplu işleme için veri ARŞİVLEMEYİN. Akış analizi birinci sınıf burada ayrıntılı şekilde belgelenen birkaç Azure hizmetleriyle tümleştirme vardır.

Stream Analytics işiniz için bir çıktı eklemek için:

1. İçinde [Azure portal](https://portal.azure.com), işinizi açıp tıklatın **çıkışları** ve ardından **Ekle** görünür çıkışları dikey penceresinde.
   
    ![Çıkış ekleme](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. Bu çıktı için bir kolay ad **çıkış diğer adları** kutusu. Bu ad, işin sorguyu daha sonra çıkış başvurmak için kullanılabilir.  
   
    Çıktı bağlanmak için gereken bağlantı özelliklerini doldurun.  Bu alanlar çıktı türüne göre değişir ve burada ayrıntılı olarak tanımlanır.  
   
    ![Veri taşıma türü seçin](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. Çıktı türüne bağlı olarak, verileri nasıl serileştirilecek veya biçimlendirilmiş belirtmeniz gerekebilir. Her çıktı türü için belirli serileştirme ayarlarını aşağıda belirtilmiştir.
   
    Veri kaynağına bağlanmak için gereken bağlantı özelliklerini doldurun. Bu alanlar giriş ve kaynak türü türüne göre değişir ve ayrıntılı olarak tanımlanan [işi oluştur makale](stream-analytics-create-a-job.md).  

> [!Note]
>
> İş başlatıldı ve akan olayları başlatmak için önce projeye eklenen herhangi bir çıkış öğesi bulunmalıdır. Blob Depolama çıkış olarak kullanırsanız, örneğin, iş bir depolama hesabı otomatik olarak oluşturmaz. ASA işi başlatılmadan önce kullanıcı tarafından oluşturulmuş gerekir.
> 
 

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

