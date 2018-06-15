---
title: Azure Stream Analytics gidermek için genel sorunlar
description: Bu makalede Azure akış analizi ve adımları bu sorunları gidermek için birçok yaygın sorunları açıklar.
services: stream-analytics
author: jasonwhowell
manager: kfile
ms.author: jasonh
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: e04d1072acee635235b0a5bd8465ca38c861017b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31523532"
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Akış analizi ve adımları gidermek için genel sorunlar

## <a name="troubleshoot-malformed-input-events"></a>Hatalı oluşturulmuş giriş olaylarını sorun giderme

 Giriş akışı akış analizi işinin hatalı biçimlendirilmiş iletileri içerdiğinde serileştirme sorunları neden olur. Örneğin, hatalı bir ileti nedeniyle eksik parantez ya da bir JSON nesnesi eksik bir ayraç ya da yanlış olabilir zaman damgası biçimi Zamanı alanındaki. 
 
 Akış analizi işine giriş yapmanızı hatalı bir ileti aldığında, iletiyi bırakır ve bir uyarı ile kullanıcıyı uyarır. Bir uyarı simgesi gösterilir **girişleri** (Bu uyarı oturum mevcut iş çalışır durumda olduğu sürece), Stream Analytics işi parçasına:

![Girişleri döşeme](media/stream-analytics-malformed-events/inputs_tile.png)

Daha fazla bilgi için uyarı ayrıntılarını görüntülemek tanılama günlüklerini etkinleştirin. Hatalı oluşturulmuş giriş olaylar için yürütme günlüklerini benzer ileti olan bir giriş içerir: "ileti: kaynaktan giriş olay kaldırılamadı <blob URI> json olarak". 

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

1. Giriş döşemeye gidin ve Uyarıları görüntülemek için tıklatın.

2. Giriş Ayrıntıları kutucuğu sorun hakkında ayrıntılar uyarılarla kümesini görüntüler. Aşağıdaki örnek uyarı iletisi, bölüm, uzaklık ve sıra numaraları uyarı iletisi gösterir hatalı biçimlendirilmiş JSON verilerini olduğu. 

   ![Uzaklık uyarı iletisi](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Hatalı biçimde JSON verilerini almak için CheckMalformedEvents.cs kodu çalıştırın. Bu örnekte kullanılabilir [GitHub örnekleri depo](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Bu kod okuma bölüm kimliği, uzaklığı ve bu uzaklığı bulunan verileri yazdırır. 

4. Verileri okuduktan sonra, serileştirme biçimini analiz edebilir ve düzeltebilirsiniz. 

## <a name="delayed-output"></a>Gecikmeli çıkış

### <a name="first-output-is-delayed"></a>İlk çıkış ertelendi
Akış analizi işi başlatıldığında, giriş olaylarını okuma, ancak bazı durumlarda üretilen çıktıda bir gecikme olabilir.

Zamana bağlı sorgusu öğeleri büyük saat değerleri çıktı gecikme katkıda bulunabilir. Doğru çıktı büyük zaman pencereleri oluşturmak için iş akışında zaman pencereyi doldurmak için en son zaman mümkün (en çok yedi gün önce) veri okuyarak başlar. Bekleyen giriş olayların yakalama okuma işlemi tamamlanana kadar bu süre boyunca hiçbir çıktı oluşturulur. Sistem iş böylece yeniden akış işi yükseltildiğinde bu sorun ortaya. Bu tür yükseltmeler genellikle her birkaç kez ay oluşur. 

Bu nedenle, Stream Analytics sorgu tasarlarken dikkatli olun. Büyük zaman penceresi kullanıyorsanız (birden fazla birkaç saat yukarı yedi gün olarak) iş başlatıldığında veya yeniden işin sorgu sözdizimi zamana bağlı öğeler için onu bir gecikme ilk çıktı yol açabilir.  

Bu tür bir ilk çıkış gecikme için bir Azaltıcı Etkenler (veri bölümlendirme) sorgu paralelleştirme tekniklerini kullanın ya da daha fazla akış işi arayı kapatıncaya kadar verimini artırmak için birim ekleyin sağlamaktır.  Daha fazla bilgi için bkz: [akış analizi işleri oluştururken dikkat edilecek noktalar](stream-analytics-concepts-checkpoint-replay.md)

Bu etkenler oluşturulan ilk çıkış dakikliğini etkileyebilir:

1. Pencerelenmiş toplamlar (grubu tarafından atlamalı ve windows kayan dönen,) kullanın
   - Dönen veya pencere toplamalar atlaması için sonuçları penceresi süre sonunda oluşturulur. 
   - Bir olay girdiğinde veya kayan pencere çıktığında kayan bir pencere için sonuçları üretilir. 
   - Büyük pencere boyutu (> 1 saat) kullanmayı planlıyorsanız, çıktı daha sık görebilmeniz için atlamalı veya kayan pencere seçmek en iyisidir.

2. Zamana bağlı birleşimler (DATEDIFF katılma) kullanımı
   - Her iki tarafında eşleşen olaylar geldiğinde hemen eşleşmeleri üretilir.
   - Bir eşleşme (sol dış birleşim) eksik veriler DATEDIFF pencerenin sol tarafındaki her olay göre sonunda oluşturulur.

3. Zamana bağlı analitik işlevlerini (ISFİRST, son ve GECİKME sınırı süre ile)
   - Analitik İşlevler, çıkışı her olay için oluşturulur, gecikme yoktur.

### <a name="output-falls-behind"></a>Çıktı döner
İş normal işlem sırasında (uzun ve daha uzun gecikme), iş çıkışı dönmeden fark ederseniz bu Etkenler inceleyerek nedenlerini saptayabilirler:
- Aşağı Akış havuz olup kısıtlanıyor
- Yukarı Akış kaynağı olup olmadığını kısıtlanıyor
- Sorgu işleme mantığı işlem yoğunluklu olup olmadığı

Azure portalında, bu ayrıntıları görmek için akış işi seçin ve seçin **iş diyagramı**. Her giriş için var olan bir bölüm biriktirme listesi olay ölçüm başına. Biriktirme listesi olay ölçüm artmaya devam ederse, sistem kaynaklarının sınırlı olduğu bir göstergesidir. Olası nedeni havuz çıkış azaltma veya yüksek CPU olmasıdır. İş diyagramı kullanma hakkında daha fazla bilgi için bkz: [veri temelli iş diyagramı kullanarak hata ayıklama](stream-analytics-job-diagram-with-metrics.md).

## <a name="handle-duplicate-records-in-azure-sql-database-output"></a>Azure SQL veritabanı çıkışı yinelenen kayıtları işleme

Akış analizi işine çıkış olarak Azure SQL veritabanını yapılandırdığınızda yığın kayıtları hedef tabloya ekler. Genel olarak, Azure akış analizi garanti [en az bir kere teslim]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) çıkış havuzu için bir yine [tam olarak elde-kez teslim]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) SQL tablosu tanımlanan benzersiz bir kısıtlamaya sahip olduğunda SQL çıktı. 

Benzersiz anahtar kısıtlamalarını SQL tablo üzerinde ayarlanır ve SQL tablosuna eklenen yinelenen kayıtları vardır sonra Azure akış analizi yinelenen kaydını kaldırır. Toplu işlemleri ve tek bir yinelenen kayıt bulunana kadar toplu ekleme yinelemeli olarak veri böler. Daha az verimli ve zaman alıcı olan birer birer, yinelemeleri yoksaymak iş akışında bir önemli yinelenen satır sayısı, bu bölme varsa ve işlem eklemek vardır. Son bir saat içinde etkinlik günlüğünde birden çok anahtar ihlali uyarı iletisi görürseniz, SQL çıkış tüm iş yavaşlamasının olduğunu olasıdır. 

Bu sorunu gidermek için şunları yapmanız gerekir [dizini yapılandırma]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) , neden anahtar ihlali IGNORE_DUP_KEY seçeneği etkinleştirilerek. SQL Azure yalnızca bir uyarı iletisi yerine bir hata oluşturur ve bu seçeneğin etkinleştirilmesi SQL tarafından toplu eklemeler sırasında yok sayılacak yinelenen değerler sağlar. Azure Stream Analytics birincil anahtar ihlali hataları artık üretmez.

Aşağıdaki gözlemleri IGNORE_DUP_KEY dizin çeşitli türleri için yapılandırırken dikkat edin:

* Bir birincil anahtar veya ALTER INDEX kullanan benzersiz bir kısıtlamaya IGNORE_DUP_KEY ayarlayamazsınız, bırakma ve dizini yeniden oluşturmanız gerekir.  
* BİRİNCİL anahtar/UNIQUE kısıtlaması farklı olan ve CREATE INDEX veya dizin tanımı kullanılarak oluşturulan benzersiz bir dizin için ALTER INDEX kullanarak IGNORE_DUP_KEY seçeneği ayarlayabilirsiniz.  
* Böyle dizinlerin benzersizlik yapılamıyor çünkü IGNORE_DUP_KEY sütun deposu dizinleri için geçerli değildir.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
