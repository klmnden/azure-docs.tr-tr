---
title: "Akış analizi işleri için veri girişi ekleme | Microsoft Docs"
description: "Stream Analytics işiniz Blog depolama verileri olay hub'ları veya başvuru veri girişi akış olarak bir veri kaynağına bağlanacağını öğrenin."
keywords: "veri akış girişi, veri"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 8bdbcf78f2892cbd1e1cc09cef220dff08dd9490
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Stream Analytics işe akış veri girişi veya başvuru veri ekleme
Olay hub'ları veya başvuru verileri Blob depolama biriminden veri girişten akış olarak Stream Analytics işiniz için bir veri kaynağı bağlanacağını öğrenin.

Azure akış analizi işleri, bir veri girişi veya varolan bir veri kaynağına her biri tanımlayan bir bağlantı daha fazla bağlanabilir. Bu veri kaynağına gönderilen veri gibi akış analizi işi tarafından tüketilen ve gerçek zamanlı veri akışı olarak işlenir. Akış Analizi ile birinci sınıf tümleştirme sahip [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) ve [Azure Blob Depolama](../storage/blobs/storage-dotnet-how-to-use-blobs.md) hem içinde hem de iş abonelik dışında.

Bu makalede bir adımdır [Stream Analytics öğrenme yolu](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Veri girişi: akış veri ve başvuru verileri
İki farklı Stream Analytics girdi vardır: veri akışlarını ve başvuru verileri.

* **Veri akışları**: akış analizi işleri, en az bir veri akış girişine tüketilen ve iş tarafından dönüştürülmüş içermelidir. Azure Blob Depolama ve Azure Event Hubs veri akışı giriş kaynağı olarak desteklenir. Azure Event Hubs bağlı aygıtları, hizmetler ve uygulamalar olay akışları toplayacak şekilde kullanılır. Azure Blob Depolama toplu veri akışı olarak alma için bir giriş kaynağı kullanılabilir.  
* **Başvuru verileri**: akış analizi, ikinci bir yardımcı giriş çağrılan başvuru veri türünü destekler.  Hareket halindeki verileriniz aksine, bu verileri statik veya yavaşlamasının değiştirme.  Bu genellikle göz atmayı ve veri akışları ile bağıntıları gerçekleştirmek için daha zengin bir veri kümesi oluşturmak için kullanılır.  Azure Blob Depolama şu anda yalnızca desteklenen giriş başvuru verileri kaynağıdır.  

Stream Analytics işiniz için bir giriş eklemek için:

1. Azure portal'ı tıklayın **girişleri** ve ardından **bir giriş eklemek** Stream Analytics işi.
   
    ![Klasik Azure portalı - bir giriş ekleyin.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    Azure portal'ı tıklayın **girişleri** döşeme Stream Analytics işinde.  
   
    ![Azure portal - veri girişi ekleyin.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. Giriş türünü belirtin: ya da **veri akışı** veya **başvuru verileri**.
   
    ![Doğru veri girişi, akışı veya başvuru ekleme](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Doğru veri girişi, akışı veya başvuru ekleme](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. Bir veri akış girişine oluşturuyorsanız, giriş için kaynak türünü belirtin.  Bu adım, yalnızca depolama birimi şu anda desteklenen Blob olarak başvuru verileri oluşturma sırasında atlanabilir.
   
    ![Veri akışı veri girişi ekleme](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Veri akışı veri girişi ekleme](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. Giriş diğer adı kutusuna bu girişi için kolay bir ad sağlayın.  Bu ad, işin sorguyu daha sonra giriş başvurmak için kullanılır.
   
    Veri kaynağına bağlanmak için gereken bağlantı özelliklerini doldurun. Bu alanlar giriş ve kaynak türü türüne göre değişir ve ayrıntılı olarak tanımlanan [burada](stream-analytics-create-a-job.md).  
   
    ![Olay hub'ı veri girişi ekleme](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. Giriş verilerini seri hale getirme ayarlarını belirtin:
   
   * Sorgularınızın beklediğiniz gibi çalıştığından emin olmak için belirtmek **olayı seri hale getirme biçimi** gelen veri.  Desteklenen serileştirme biçimleri, JSON, CSV ve Avro olacaktır.
   * Doğrulama **kodlama** veriler için.  Şu anda desteklenen tek kodlama biçimi UTF-8'dir.
     
     ![Giriş verileri için veri seri hale getirme ayarları](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Giriş verileri için veri seri hale getirme ayarları](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. Giriş oluşturmayı tamamladıktan sonra Stream Analytics giriş kaynağına bağlanabildiğinizi doğrular.  Bildirim hub'ı Bağlantıyı Sına işlemin durumunu görüntüleyebilirsiniz.
   
    ![Giriş akış verilerini bağlantıyı sınayın](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Giriş akış verilerini bağlantıyı sınayın](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Veri girişleri akış ile ilgili yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

