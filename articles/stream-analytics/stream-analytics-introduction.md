---
title: "Akış Analizi'ne Giriş | Microsoft Belgeleri"
description: "Nesnelerin İnterneti'nden (IoT) sağlanan akış verilerini gerçek zamanlı olarak analiz etmenize yardım eden bir yönetilen hizmet olan Stream Analytics hakkında bilgi edinin."
keywords: "hizmet olarak analytics, yönetilen hizmetler, akış işleme, streaming analytics, stream analytics nedir"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/16/2017
ms.author: jeffstok
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 2fda07b8444b196d831e5e9fe7ade3e5017d36f9
ms.contentlocale: tr-tr
ms.lasthandoff: 07/19/2017

---

# <a name="what-is-stream-analytics"></a>Stream Analytics nedir?

Azure Stream Analytics, akış verilerine gerçek zamanlı analitik hesaplamalar eklemenizi sağlayan tam yönetimli bir olay işleme sistemidir. Veriler cihazlardan, sensörlerden, web sitelerinden, sosyal medya akışlarından, uygulamalardan, altyapı sistemlerinden ve çok daha fazlasından gelebilir. 

## <a name="what-can-i-use-stream-analytics-for"></a>Stream Analytics'i ne için kullanabilirim?

Stream Analytics'i kullanarak cihazlardan veya işlemlerden geçen yüksek hacimli verileri inceleyebilir, veri akışından bilgi çıkarabilir ve modeller, eğilimler ve ilişkilere bakabilirsiniz. Ardından verilerin içeriğine bağlı olarak uygulama görevleri gerçekleştirebilirsiniz. Örneğin uyarı oluşturabilir, otomatik iş akışlarını başlatabilir, Power BI gibi bir raporlama aracına bilgi aktarabilir veya verileri daha sonra incelemek üzere saklayabilirsiniz. 

Stream Analytics senaryolarına örnekler şunlardır:

* Finansal hizmet şirketleri tarafından sunulan kişiselleştirilmiş, gerçek zamanlı borsa analizi ve uyarılar.
* İşlem verilerinin incelenmesiyle gerçek zamanlı sahtekarlık algılama. 
* Veri ve kimlik koruması hizmetleri.
* Fiziksel nesnelere takılı sensörler ve aktüatörler tarafından üretilen verilerin analiz edilmesi (Nesnelerin İnterneti, IoT).
* Web tıklama dizisi çözümlemeleri.
* Belirli bir zaman aralığındaki müşteri deneyimi kötüye gittiğinde uyarı oluşturma gibi müşteri ilişkileri yönetimi (CRM) uygulamaları.

## <a name="how-does-stream-analytics-work"></a>Stream Analytics nasıl çalışır?

Aşağıdaki diyagramda Stream Analytics işlem hattı üzerinde verilerin toplanma, analiz edilme ve sunum ya da eylem için gönderilme şekli gösterilmektedir. 

![Stream Analytics işlem hattı](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics bir veri kaynağından gelen akışla başlar. Veriler, Azure olay hub'ı veya IoT hub'ı kullanan bir cihazdan Azure'a aktarılabilir. Veriler aynı zamanda Azure Blob Depolama gibi bir veri kaynağından da çekilebilir. 

Akışı incelemek için verilerin nereden geldiğini belirten bir Stream Analytics *işi* oluşturursunuz. İş ayrıca verilerin, modellerin veya ilişkilerin arama şeklini belirten bir *dönüşüme*&mdash; de sahiptir. Bu görev için Stream Analytics belirli bir zaman aralığında toplanan akış verilerinin filtrelenmesini, sıralanmasını, toplanmasını ve birleştirilmesini destekleyen SQL benzeri bir sorgu dilini destekler.

Son olarak iş, dönüştürülen verilerin gönderileceği bir çıktı belirtir. Bu da analiz edilen verileri ne yapacağınızı belirlemenizi sağlar. Örneğin analiz sonucunda:

* Bir cihazın ayarlarını değiştirmek üzere komut gönderebilirsiniz. 
* Bulgulara göre eyleme geçen bir işlem tarafından izlenen bir kuyruğa veri gönderebilirsiniz. 
* Raporlama için bir Power BI panosuna veri gönderebilirsiniz.
* Verileri Data Lake Store, SQL Server veritabanı, Azure Blob veya Tablo depolama gibi depolama ortamlarına gönderebilirsiniz.

Bir iş çalışırken onu izleyebilir ve saniye başına işlenen olay sayısını ayarlayabilirsiniz. Aynı zamanda sorun giderme amacıyla tanılama günlüğü oluşturan işler de oluşturabilirsiniz.

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar

Stream Analytics kullanımı kolay, esnek, her boyuttaki iş için ölçeklenebilir ve ekonomik olacak şekilde tasarlanmıştır.

### <a name="connectivity-to-many-inputs-and-outputs"></a>Birçok giriş ve çıkışa bağlanabilme

Stream Analytics, akış alımı için doğrudan [Azure Olay Hub'larına](https://azure.microsoft.com/services/event-hubs/) ve [Azure IoT Hub'ına](https://azure.microsoft.com/services/iot-hub/); geçmiş verilerin alımı için de [Azure Blob depolama](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) hizmetine bağlanır. Olay hub'larından veri alıyorsanız Stream Analytics'i diğer veri kaynakları ve işleme sistemleriyle birlikte kullanabilirsiniz.

İş girdisi aynı zamanda başvuru bilgilerini (statik veya yavaş değişen veriler) de içerebilir. Akış verilerini bu başvuru verileriyle birleştirerek veritabanı sorgusu gibi arama işlemleri gerçekleştirebilirsiniz.

Bir Stream Analytics işinin çıktısı birçok şekilde yönlendirilebilir. Azure Depolama blobları veya tabloları, Azure SQL DB, Azure Data Lake Store veya Azure Cosmos DB gibi depolama ortamlarına yazılabilir. Veriler buradan Azure HDInsight aracılığıyla toplu analizden geçirilebilir. Çıktıyı başka bir işlem tarafından kullanılmak üzere başka bir hizmete gönderebilirsiniz (olay hub'ları, Azure Service Bus konuları veya kuyruklar gibi). Çıktıyı görselleştirme için Power BI'a gönderebilirsiniz.

### <a name="ease-of-use"></a>Kullanım kolaylığı

Dönüşümleri tanımlamak için programlama bilgisine ihtiyaç duymadan gelişmiş analizler oluşturmanızı sağlayan basit ve bildirim temelli [Stream Analytics sorgu dilini](https://msdn.microsoft.com/library/azure/dn834998.aspx) kullanırsınız. Sorgu dili, girdi olarak veri akışını kullanır. Ardından verileri filtreleyip sıralayabilir, değerleri toplayabilir, hesaplamalar yapabilir, verileri birleştirebilir (akış veya başvuru verileriyle) ve jeo-uzamsal işlevleri kullanabilirsiniz. IntelliSense ve söz dizimi denetimini kullanarak sorguları portalda düzenleyebilir ve sorguları canlı akıştan ayıklayabileceğiniz örnek verilerle test edebilirsiniz.

### <a name="extensible-query-language"></a>Genişletilebilir sorgu dili

Ek işlevler tanımlayıp çağırarak sorgu dilinin yapabileceklerini artırabilirsiniz. Azure Machine Learning hizmetinde işlev çağrıları tanımlayarak Azure Machine Learning çözümlerinden faydalanabilirsiniz. Ayrıca JavaScript kullanıcı tanımlı işlevlerini (UDF) tümleştirerek Stream Analytics sorgusunun bir parçası olarak karmaşık hesaplamalar gerçekleştirebilirsiniz.

### <a name="scalability"></a>Ölçeklenebilirlik

Stream Analytics saniyede 1 GB gelen veri işleyebilir. [Azure Olay Hub'ları](https://azure.microsoft.com/services/event-hubs/) ve [Azure IoT Hub'ı](https://azure.microsoft.com/services/iot-hub/) ile tümleştirme; bağlı cihazlar, tıklama dizileri ve günlük dosyaları gibi çeşitli kaynaklardan gelen, saniyede milyonlarca olayın işler tarafından alınmasını sağlar. Olay hub'larının bölümleme özelliğini kullanarak hesaplamaları mantıksal adımlara ayırabilir, her birini bölümlere ayırarak ölçeklenebilirliği artırabilirsiniz.

### <a name="low-cost"></a>Düşük maliyet

Bir bulut hizmeti olan Stream Analytics, maliyetlerinizi düşürmeniz için iyileştirilmiştir. Akış birimi kullanımına ve sistemin işlediği veri miktarına göre kullandıkça ödersiniz. Kullanım miktarı, Stream Analytics işlerinin işlenmesi için küme içinde sağlanan işlem gücü miktarına ve işlenen olayların hacmine göre belirlenir.

### <a name="reliability-quick-recovery-and-repeatability"></a>Güvenilirlik, hızlı kurtarma ve tekrarlama

Bulutta yönetilen bir hizmet olan Stream Analytics, veri kaybının önlenmesine yardımcı olur ve iş sürekliliği sağlar. Hata oluşması halinde hizmetin sunduğu yerleşik kurtarma özelliklerinden faydalanabilirsiniz. Dahili olarak durumunu koruma özelliği sayesinde, hizmet tekrarlanabilir sonuçlar sunar ve gelecekte işlemenin yeniden uygulanmasını ve olayların arşivlenmesini, böylelikle de her zaman için aynı sonuçların alınmasını sağlar. Bu sayede kök neden analizi, durum çözümlemesi vb. gerçekleştirirken zamanda geriye giderek hesaplamaları araştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT cihazlarından gelen girdiler ve sorgularla çalışmayı deneyerek](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md) başlangıç yapın.
* Dolandırıcılık amaçlı çağrıları bulmak için telefon meta verilerini inceleyen [uçtan uca Stream Analytics çözümü](stream-analytics-real-time-fraud-detection.md) oluşturun.
* Stream Analytics'in SQL benzeri sorgu dili ve [pencere işlevleri](stream-analytics-window-functions.md) gibi benzersiz kavramları hakkında bilgi edinin.
* [Stream Analytics işlerini ölçeklendirmeyi](stream-analytics-scale-jobs.md) öğrenin. 
* [Stream Analytics ve Azure Machine Learning hizmetlerini tümleştirmeyi](stream-analytics-machine-learning-integration-tutorial.md) öğrenin.
* Stream Analytics ile ilgili sorularınıza yanıt almak için [Azure Stream Analytics forumunu](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics) ziyaret edin.


