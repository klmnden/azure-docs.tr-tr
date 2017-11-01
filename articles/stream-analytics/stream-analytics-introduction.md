---
title: "Akış Analizi'ne Giriş | Microsoft Belgeleri"
description: "Nesnelerin İnterneti'nden (IoT) sağlanan akış verilerini gerçek zamanlı olarak analiz etmenize yardım eden bir yönetilen hizmet olan Stream Analytics hakkında bilgi edinin."
keywords: "hizmet olarak analytics, yönetilen hizmetler, akış işleme, streaming analytics, stream analytics nedir"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/13/2017
ms.author: samacha
ms.openlocfilehash: 57437875da80bb5eece650063b7c5c2b0ffd65f7
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="what-is-stream-analytics"></a>Stream Analytics nedir?

Azure Stream Analytics, akış verilerine gerçek zamanlı analitik hesaplamalar ekleyen sağlayan yönetimli bir olay işleme sistemidir. Veriler cihazlardan, sensörlerden, web sitelerinden, sosyal medya akışlarından, uygulamalardan, altyapı sistemlerinden ve çok daha fazlasından gelebilir. 

Stream Analytics'i kullanarak cihazlardan veya işlemlerden geçen yüksek hacimli verileri inceleyebilir, veri akışından bilgi ayıklayabilir ve modeller, eğilimler ve ilişkiler tanımlayabilirsiniz. Uyarılar, otomasyon iş akışları, akış bilgileri gibi diğer işlemleri veya eylemleri bir raporlama aracına tetiklemek ya da daha sonra araştırmak üzere depolamak için bu modelleri kullanabilirsiniz. 

Bazı örnekler:

* Hisse senedi analizi ve uyarıları.
* Sahtekarlık algılama, veri ve kimlik korumaları. 
* Yerleşik sensör ve aktüatör analizi.
* Web tıklama dizisi çözümlemeleri.

## <a name="how-does-stream-analytics-work"></a>Stream Analytics nasıl çalışır?

Bu diyagramda Stream Analytics işlem hattı üzerinde verilerin toplanma, analiz edilme ve sunum ya da eylem için gönderilme şekli gösterilmektedir. 

![Stream Analytics işlem hattı](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics bir veri kaynağından gelen akışla başlar. Veriler, Azure olay hub'ı veya IoT hub'ı kullanan bir cihazdan Azure'a aktarılabilir. Veriler aynı zamanda Azure Blob Depolama gibi bir veri kaynağından da çekilebilir. 

Akışı incelemek için verilerin nereden geldiğini belirten bir Stream Analytics *işi* oluşturursunuz. İş ayrıca verilerin, modellerin veya ilişkilerin arama şeklini belirten bir *dönüşüme* de sahiptir. Bu görev için Stream Analytics belirli bir zaman aralığında toplanan akış verilerinin filtrelenmesini, sıralanmasını, toplanmasını ve birleştirilmesini destekleyen SQL benzeri bir sorgu dilini destekler.

Son olarak iş, dönüştürülen veriler için bir çıktı belirtir. Analiz edilen verileri ne yapacağınızı siz belirlersiniz. Örneğin analiz sonucunda:

* Bir cihazın ayarlarını değiştirmek üzere komut gönderebilirsiniz. 
* Verileri bulgulara göre eylem gerçekleştirilmesi için izlenen bir kuyruğa gönderebilirsiniz. 
* Verileri bir Power BI panosuna gönderebilirsiniz.
* Verileri Data Lake Store, Azure SQL Veritabanı veya Azure Blob depolama alanı gibi depolama ortamlarına gönderebilirsiniz.

İş çalışırken saniye başına işlenecek etkinlik sayısını ayarlayabilirsiniz. Aynı zamanda sorun giderme amacıyla tanılama günlüğü oluşturan işler de oluşturabilirsiniz.

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar

Stream Analytics kullanımı kolay, esnek ve her boyuttaki iş için ölçeklenebilir olacak şekilde tasarlanmıştır.

### <a name="connect-inputs-and-outputs"></a>Giriş ve çıkışları bağlama

Stream Analytics, akış alımı için doğrudan [Azure Olay Hub'larına](https://azure.microsoft.com/services/event-hubs/) ve [Azure IoT Hub'ına](https://azure.microsoft.com/services/iot-hub/); geçmiş verilerin alımı için de [Azure Blob depolama](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) hizmetine bağlanır. Stream Analytics ile olay hub'larından gelen verileri diğer veri kaynakları ve işleme sistemleriyle birleştirebilirsiniz. İş girdisi aynı zamanda başvuru bilgilerini (statik veya yavaş değişen veriler) de içerebilir. Akış verilerini bu başvuru verileriyle birleştirerek veritabanı sorgusu gibi arama işlemleri gerçekleştirebilirsiniz.

Stream Analytics iş çıktısını birçok yöne yönlendirme. Azure Blob, Azure SQL Veritabanı, Azure Data Lake Stores veya Azure Cosmos DB gibi depolama alanlarına yazabilirsiniz. Bu konumlardan Azure HDInsight ile toplu analiz gerçekleştirebilirsiniz. Ya da çıktıyı başka bir işlem tarafından kullanılmak üzere başka bir hizmete gönderebilirsiniz (görselleştirme için olay hub'ları, Azure Service Bus, kuyruklar veya Power BI).

### <a name="easy-to-use"></a>Kullanımı kolay

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
* Stream Analytics sorularınıza yanıt almak için [Azure Stream Analytics forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) ziyaret edin.

