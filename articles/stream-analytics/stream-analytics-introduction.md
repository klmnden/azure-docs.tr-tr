---
title: "Akış Analizi&quot;ne Giriş | Microsoft Belgeleri"
description: "Nesnelerin İnterneti&quot;nden (IoT) sağlanan akış verilerini gerçek zamanlı olarak analiz etmenize yardım eden bir yönetilen hizmet olan Stream Analytics hakkında bilgi edinin."
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
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: a6b1df79f4fc6b09b681755f5adbb7f56faaf225
ms.lasthandoff: 05/01/2017


---
# <a name="what-is-stream-analytics"></a>Stream Analytics nedir?
Azure Akış Analizi (ASA), verilerden ayrıntılı öngörüler elde etmenizi sağlayan, tam olarak yönetilen, düşük maliyetli ve gerçek zamanlı bir olay işleme altyapısıdır. Stream Analytics cihazlar, algılayıcılar, web siteleri, sosyal medya, uygulamalar, altyapı sistemleri ve daha fazlasından sağlanan veri akışlarında gerçek zamanlı analitik hesaplamalar ayarlamanızı kolaylaştırır.

Azure portalında birkaç tıklama ile akış verilerine ilişkin giriş kaynağının, işinizin sonuçları için çıkış havuzunun ve SQL benzeri bir dille ifade edilen bir veri dönüşümünün yer aldığı bir Stream Analytics işini yazabilirsiniz. Azure portalında işinizin ölçeğini/hızını izleyip ayarlayabilir ve her saniye işlenen olayların birkaç kilobayttan bir gigabayta kadarını veya daha fazlasını ölçeklendirebilirsiniz.

Stream Analytics, zamana duyarlı işlemeye yönelik üst düzey ayarlamaları yapılmış olan akış altyapılarının, aynı zamanda bu altyapıların sezgisel belirtimleri için dil tümleştirmelerinin geliştirilmesi konusunda Microsoft Research'ün yıllar süren çalışmalarından faydalanmaktadır.

## <a name="what-can-i-use-stream-analytics-for"></a>Stream Analytics'i ne için kullanabilirim?
Günümüzde çok büyük miktarda veri kablo üzerinden yüksek hızlarda akmaktadır. Bu akış verilerini gerçek zamanlı olarak işleyip buna göre hareket edebilen kuruluşlar, verimliliklerini ciddi düzeyde yükseltebilir ve piyasada rakiplerinden ayrılabilirler. Gerçek zamanlı akış analizlerini içeren senaryolar tüm sektörlerde bulunabilir: finansal hizmetler şirketlerinin sağladığı kişiselleştirilmiş ve gerçek zamanlı hisse senedi alım-satım analizleri ve uyarıları; gerçek zamanlı sahtekarlık algılama; veri ve kimlik koruma hizmetleri; fiziksel nesnelere katıştırılmış algılayıcılar ve erişim düzenekleri tarafından oluşturulan verilerin güvenilir şekilde alımı ve analizi (Nesnelerin İnterneti veya IoT); web tıklama dizisi analizleri; bir zaman dilimi içindeki müşteri deneyimi değeri düştüğünde uyarı sağlayan müşteri ilişkileri yönetimi (CRM) uygulamaları. İşletmeler, modern iş dünyasının son derece rekabetçi olan ortamında başarılı olmak için söz konusu gerçek zamanlı olay akışı veri analizlerini kullanmanın en esnek, güvenilir ve uygun maliyetli yolunu aramaktadır.

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar
* **Kullanım kolaylığı:** Stream Analytics, dönüşümlerin açıklanmasında basit ve bildirim temelli bir sorgu modelini destekler. Akış Analizi, kullanım kolaylığını iyileştirmek için bir T-SQL varyantı kullanır, böylece müşterilerin akış işleme sistemlerinin teknik karmaşıklıklarıyla uğraşma zorunluluğunu ortadan kaldırır. Tarayıcı içi sorgu düzenleyicisinde [Akış Analizi sorgu dilini](https://msdn.microsoft.com/library/azure/dn834998.aspx) kullanarak IntelliSense otomatik tamamlama özelliğinden yararlanabilirsiniz; bu özellik zaman tabanlı birleşimler, pencereli toplamlar, zamana bağlı filtreler; ayrıca birleşimler, toplamlar, izdüşümler ve filtreler gibi diğer yaygın işlemler de dahil olmak üzere zaman serisi sorgularını hızlı ve kolay bir şekilde uygulamanızı sağlar. Ayrıca bir örnek veri dosyasında tarayıcı içi sorgu testi yapılması, hızlı ve yinelemeli bir geliştirme sağlar.  
* **Ölçeklenebilirlik:** Stream Analytics 1 GB/saniyeye varan yüksek olay performanslarını işleyebilir. [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) ve [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)'ları ile tümleştirme; bağlı cihazlar, tıklama dizileri ve günlük dosyaları gibi çeşitli kaynaklardan gelen, saniyede milyonlarca olayın alınması sorunu için çözüm sağlar. Stream Analytics bunu başarmak için Event Hubs'ın bölüm başına 1 MB/sn sağlayabilen bölümleme işlevinden yararlanır. Kullanıcılar sorgu tanımı içinde hesaplamayı belirli sayıda mantıksal adıma bölebilirler, bu adımların her biri ölçeklenebilirliği artırmak üzere yeniden bölümlenebilir.  
* **Güvenilirlik, yinelenebilirlik ve hızlı kurtarma:** Bulutta yönetilen bir hizmet olan Akış Analizi, veri kaybını önlemeye yardımcı olur, yerleşik kurtarma özellikleri sayesinde hatalar söz konusu olduğunda işin devamlılığını sağlar. Dahili olarak durumunu koruma özelliği sayesinde, hizmet tekrarlanabilir sonuçlar sunar ve gelecekte işlemenin yeniden uygulanmasını ve olayların arşivlenmesini, böylelikle de her zaman için aynı sonuçların alınmasını sağlar. Bu sayede müşteriler kök neden analizi, durum çözümlemesi vb. gerçekleştirirken zamanda geriye giderek hesaplamaları araştırabilirler.  
* **Düşük maliyet:** Bir bulut hizmeti olan Stream Analytics, kullanıcıların çok düşük bir maliyetle başlangıç yapmalarını ve gerçek zamanlı analiz sonuçlarını kullanmalarını sağlamak amacıyla iyileştirilmiştir. Hizmet, Akış Birimi kullanımına ve sistemin işlediği veri miktarına göre kullandıkça ödemenizi sağlayacak şekilde oluşturulmuştur. Kullanım miktarı, ilgili Stream Analytics işlerinin işlenmesi için küme içinde sağlanan işlem gücü miktarına ve işlenen olayların hacmine göre üretilir.  
* **Başvuru verileri:** Stream Analytics kullanıcıların başvuru verilerini belirtmelerine ve kullanmalarına olanak tanır. Bunlar zaman içinde daha nadir olarak değişen akış dışındaki veriler veya geçmiş verileri olabilir. Sistem, başvuru verilerinin kullanımını basitleştirerek bunların diğer herhangi bir gelen olay akışı şeklinde değerlendirilmesini sağlar ve bunları gerçek zamanlı olarak alınan diğer olay akışlarıyla birleştirerek dönüşümleri gerçekleştirir.  
* **Kullanıcı Tanımlı İşlevler:** Akış Analizi, Machine Learning hizmetindeki işlev çağrılarını Akış Analizi sorgusunun bir parçası olarak tanımlamak üzere Azure Machine Learning ile tümleşiktir. Bu, Akış Analizi'nin mevcut Azure Machine Learning çözümlerinden faydalanma olanağını artırır. Bu konuda daha fazla bilgi edinmek için lütfen bkz. [Machine Learning tümleştirme öğreticisi](stream-analytics-machine-learning-integration-tutorial.md).
* **Bağlantı:** Akış Analizi, akış alımı için doğrudan Azure Event Hubs'a ve Azure IoT Hub'larına; geçmiş verilerin alımı için de Azure Blob hizmetine bağlanır. Sonuçlar; Akış Analizi'nden Azure Depolama Bloblarına veya Tablolarına, Azure SQL DB'ye, Azure Data Lake Depolarına, DocumentDB'ye, Event Hubs'a, Azure Service Bus Konu Başlıklarına veya Kuyruklarına ve Power BI'a yazılabilir; burada sonuçlar görselleştirilebilir, iş akışları tarafından daha fazla işlenebilir, [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) yoluyla toplu analizlerde kullanılabilir veya bir olay serisi olarak yeniden işlenebilir. Event Hubs'ı kullanırken, hesaplamaların akış yapısını kaybetmeden birden çok Stream Analytics'i diğer veri kaynaklarıyla ve işleme altyapılarıyla birlikte oluşturmak mümkündür.  

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Nesnelerin İnterneti'nden gelen verilerdeki akış analizlerine yönelik bir yönetilen hizmet olan Stream Analytics'e giriş yaptınız. Bu hizmet hakkında daha fazla bilgi edinmek için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-get-started.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)


