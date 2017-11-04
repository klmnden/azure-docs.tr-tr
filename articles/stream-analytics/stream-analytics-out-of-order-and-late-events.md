---
title: "İşleme olayı sırası ve Azure akış Analizi ile lateness | Microsoft Docs"
description: "Stream Analytics veri akışında düzen dışı ya da geç olaylarla işleyişi hakkında bilgi edinin."
keywords: "bozuk, geç, olayları"
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
ms.openlocfilehash: cf57bd12a62b3de8ac49b26ce7cdc40aec0b6738
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Azure Stream Analytics olay sipariş işleme

Olayları bir zamana bağlı veri akışında her olay bir zaman damgası atanır. Azure Stream Analytics geliş saati ya da uygulama zamanı kullanarak her olay zaman damgası atar. "System.Timestamp" sütun olaya atanan zaman damgası vardır. Olay kaynağı ulaştığında geliş saati giriş kaynakta atanır. Geliş saati için olay hub'ı giriş EventEnqueuedTime olduğu ve [blob son değiştirme zamanı](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) blob giriş. Uygulama zamanı olayı oluşturulur ve yükü parçası olduğunda atanır. Uygulama zamanına göre olayları işlemek için "Zaman damgası tarafından" yan tümcesi select sorgusundaki kullanın. "Zaman damgası tarafından" yan tümcesi olmazsa olayları geliş saati tarafından işlenir. Geliş saati, olay hub'ı için EventEnqueuedTime özelliğini kullanarak ve blob giriş BlobLastModified özelliği kullanılarak erişilebilir. Azure Stream Analytics zaman damgası sırada bir çıktı üretir ve bozuk verilerle mücadele etmek için birkaç ayarları sağlar.

![Stream Analytics olay işleme](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

Sırayla olmayan giriş akışları ya da şunlardır:
* Sıralanmış (ve bu nedenle **Gecikmeli**).
* Sistem tarafından kullanıcı tanımlı ilkesine göre ayarlanır.

Akış analizi tarafından uygulama zamanı işlerken geç ve bozuk olayları göstereceği.

**Geç varış dayanıklılık**

* Bu ayar yalnızca uygulama zamanına göre işlerken geçerlidir, aksi halde yoksayılır.
* Geliş saati ve uygulama saat arasındaki maksimum fark budur. Uygulama süresi (saat varış - geç varış penceresi) önce ise (süresi varış - geç varış penceresi) için ayarlanır
* Birden çok bölüm aynı giriş akışından veya birden çok giriş akışları birlikte birlikte kullanıldığında, geç varış dayanıklılık yeni veriler için her bölüm bekleyeceği en fazla süreyi ' dir. 

Kısaca, geç varış pencere olay oluşturma ile olay giriş kaynağı, alınmasına arasındaki en büyük gecikme olur.
Geç varış toleransı göre ayarlama ilk yapılır ve bozuk sonraki yapılır. **System.Timestamp** olaya atanan son zaman damgası sütunu olacaktır.

**Sıralama dışı tolerans**

* Sıranın dışında ancak "sıralama dışı tolerans penceresi" kümesi içinde gelmesini olaylar **zaman damgası tarafından kaldırılmasında**. 
* Dayanıklılık daha sonra gelen olaylar **bırakılan veya ayarlanmış**.
    * **Ayarlanmış**: kabul edilebilir en son zaman gelmedi görünecek şekilde ayarlanmış. 
    * **Bırakılan**: atılır.

"Sıralama dışı tolerans penceresi içinde" alınan olayları yeniden sıralamak için sorgu çıkışıdır **sıralama dışı tolerans penceresi tarafından Gecikmeli**.

**Örnek**

Geç varış dayanıklılık = 10 dakika<br/>
Sipariş tolerans dışı = 3 dakika<br/>
Uygulama tarafından işleme<br/>

Etkinlikler:

Olay 1 _uygulama zamanı_ 00:00:00 = _geliş saati_ 00:10:01 = _System.Timestamp_ 00:00: çünkü ayarlanmış 01, = (_geliş saati_  -  _Uygulama zamanı_) geç varış dayanıklılık büyük.

Olay 2 _uygulama zamanı_ 00:00:01 = _geliş saati_ 00:10:01 = _System.Timestamp_ 00:00: uygulama süresi içinde geç varış olduğundan ayarlanmış değil 01, = penceresini açın.

%3 olayı _uygulama zamanı_ 00:10:00 = _geliş saati_ 00:10:02 = _System.Timestamp_ 00:10: uygulama zamanı geç varış pencereye olduğundan ayarlanmış değil 00 = .

Olay 4 _uygulama zamanı_ 00:09:00 = _geliş saati_ 00:10:03 = _System.Timestamp_ 00:09: uygulama süresi içinde olduğu gibi özgün damgasıyla kabul 00 = Sipariş dayanıklılık.

Olay 5 _uygulama zamanı_ 00:06:00 = _geliş saati_ 00:10:04 = _System.Timestamp_ 00:07: uygulama zamanı bozuk eski olduğu için ayarlanmış 00 = dayanıklılık.



## <a name="get-help"></a>Yardım alın
Ek Yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
