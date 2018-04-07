---
title: Hatalı oluşturulmuş giriş olaylarında Azure akış analizi için sorun giderme
description: My girişinde hangi olay nasıl anlarım veri Stream Analytics işinde sorunu neden
services: stream-analytics
author: jasonwhowell
manager: Kfile
ms.author: jasonh
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: fcbb03b4d9aed797cf99c374661c743d39f81276
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Akış analizi ve adımları gidermek için genel sorunlar

## <a name="troubleshoot-for-malformed-input-events"></a>Hatalı oluşturulmuş giriş olayları için sorun giderme

Giriş akışı akış analizi işinin hatalı biçimlendirilmiş iletileri içeriyorsa, serileştirme sorunlarına neden olur. Hatalı biçimlendirilmiş iletileri yanlış serileştirme eksik parantez gibi bir JSON nesnesi ya da hatalı zaman damgası biçimi içerir. Akış analizi işi hatalı bir ileti aldığında, iletiyi bırakır ve bir uyarı ile kullanıcıyı uyarır. Bir uyarı simgesi gösterilir **girişleri** (Bu uyarı oturum mevcut iş çalışır durumda olduğu sürece), Stream Analytics işi parçasına:

![Girişleri döşeme](media/stream-analytics-malformed-events/inputs_tile.png)

Uyarı ayrıntılarını görüntülemek tanılama günlükleri etkinleştirebilirsiniz. Malformatted girdi olayları için yürütme günlüklerini benzer ileti olan bir giriş içerir: "ileti: giriş olay kaldırılamadı kaynaktan <blob URI> json olarak)". 

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

1. Giriş döşemeye gidin ve Uyarıları görüntülemek için tıklatın.
2. Giriş Ayrıntıları kutucuğu sorun hakkında ayrıntılar uyarılarla kümesini görüntüler. Aşağıdaki örnek uyarı iletisi, bölüm, uzaklık ve sıra numaraları uyarı iletisi gösterir hatalı biçimlendirilmiş JSON verilerini olduğu. 

   ![Uzaklık uyarı iletisi](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Hatalı biçimde JSON verilerini almak için CheckMalformedEvents.cs kodu çalıştırmak, BT dan alabileceğiniz [GitHub örnekleri depo](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Bu kod bölüm kimliği, uzaklığı ve bu uzaklığı bulunan verileri yazdırır okur. 

4. Verileri okuduktan sonra, serileştirme biçimini analiz edebilir ve düzeltebilirsiniz. 

## <a name="handling-duplicate-records-when-using-azure-sql-database-as-output-for-a-stream-analytics-job"></a>Azure SQL veritabanı için bir Stream Analytics işi çıkış olarak kullanırken yinelenen kayıtları işleme

Akış analizi işine çıkış olarak Azure SQL veritabanını yapılandırdığınızda yığın kayıtları hedef tabloya ekler. Genel olarak, Azure akış analizi garanti [en az bir kere teslim]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) çıkış havuzu için bir yine [tam olarak elde-kez teslim]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) SQL tablosu tanımlanan benzersiz bir kısıtlamaya sahip olduğunda SQL çıktı. 

Kurulum SQL tablosunda benzersiz anahtar kısıtlamalarını olduğunda ve SQL tablosuna eklenen yinelenen kayıtları sonra Azure akış analizi yinelenen kayıt verileri toplu işleri ve kadar tek bir toplu ekleme yinelemeli olarak ayırarak kaldırır Yinelenen kayıt bulunamadı. Yinelenen satırları hattınızı içinde önemli sayıda olduğunda bölme ve veri yinelenenleri tek tek yoksayılıyor ekleme yinelemeli olarak zaman alan bir işlem olur. Son bir saat içinde etkinlik günlüğünde birden çok anahtar ihlali uyarı iletisi görürseniz, SQL çıkış tüm iş yavaşlamasının olduğunu olasıdır. Bu sorunu gidermek için şunları yapmanız gerekir [dizini yapılandırma]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) , neden anahtar ihlali IGNORE_DUP_KEY seçeneği etkinleştirilerek. SQL Azure yalnızca bir uyarı iletisi yerine bir hata oluşturur ve bu seçeneğin etkinleştirilmesi SQL tarafından toplu eklemeler sırasında yok sayılacak yinelenen değerler sağlar. Azure Stream Analytics birincil anahtar ihlali hataları artık üretmez.

Aşağıdaki gözlemleri IGNORE_DUP_KEY dizin çeşitli türleri için yapılandırırken dikkat edin:

* Bir birincil anahtar veya ALTER INDEX kullanan benzersiz bir kısıtlamaya IGNORE_DUP_KEY ayarlayamazsınız, bırakma ve dizini yeniden oluşturmanız gerekir.  
* BİRİNCİL anahtar/UNIQUE kısıtlaması farklı olan ve CREATE INDEX veya dizin tanımı kullanılarak oluşturulan benzersiz bir dizin için ALTER INDEX kullanarak IGNORE_DUP_KEY seçeneği ayarlayabilirsiniz.  
* Böyle dizinlerin benzersizlik yapılamıyor çünkü IGNORE_DUP_KEY sütun deposu dizinleri için geçerli değildir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

