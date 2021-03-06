---
title: Azure Stream Analytics’e Genel Bakış
description: Nesnelerin İnterneti'nden (IoT) sağlanan akış verilerini gerçek zamanlı olarak analiz etmenize yardım eden bir yönetilen hizmet olan Stream Analytics hakkında bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 06/21/2019
ms.openlocfilehash: c50ad2b045f32daf53033318123b68e4b2d58db5
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329331"
---
# <a name="what-is-azure-stream-analytics"></a>Azure Akış Analizi Nedir?

Azure Stream Analytics, gerçek zamanlı analizler ve analiz edin ve yüksek hacimli hızlı akış verileri birden çok kaynaktan eşzamanlı olarak işlemek için tasarlanmış karmaşık olay işleme altyapısı olan. Cihazlar, algılayıcılar, tıklama dizileri, sosyal medya akışları ve uygulamalar dahil olmak üzere giriş kaynakları arasında bir sayı ayıklanan bilgi desenleri ve ilişkileri tanımlanabilir. Bu desenleri gibi uyarılar oluşturma, bir raporlama aracına bilgi besleme veya daha sonra kullanmak için dönüştürülen verilerin depolanması iş akışlarını başlatmak ve eylemleri tetiklemek için kullanılabilir. Ayrıca, Stream Analytics, Azure IOT Edge çalışma zamanı üzerinde kullanılabilir ve aynı tam dil veya Bulut söz dizimini destekler. 

Azure Stream Analytics kullandığınızda, aşağıdaki senaryolarda örnekleri şunlardır:

* IOT cihazlarından gelen gerçek zamanlı telemetri akışlarını analiz edin
* Web günlükleri/clickstream analizi
* Filo yönetimi ve sürücüsüz araçlar için jeo-uzamsal analiz
* Uzaktan izleme ve Tahmine dayalı bakım yüksek değerli varlıklar
* Envanter denetimi ve anomali algılama için Satış Noktasında gerçek zamanlı analizler

## <a name="how-does-stream-analytics-work"></a>Stream Analytics nasıl çalışır?

Azure Stream Analytics işi, bir giriş, sorgu ve çıktı oluşur. Stream Analytics, Azure Event Hubs, Azure IOT Hub veya Azure Blob Depolama verilerini alır. SQL sorgu dilini temel alan, sorgu kolayca filtre, sıralama, toplama ve bir zaman aralığında toplanan akış verilerinin birleştirme için kullanılabilir. Bu SQL dili ile JavaScript da genişletebilir ve C# kullanıcı tanımlı işlevler (UDF'ler). Olay toplama işlemleri basit dil yapıları ve/veya yapılandırmaları preforming olduğunda seçenekleri ve zaman pencerelerinin süresini sıralama, bir kolayca ayarlayabilirsiniz.

Her bir iş, dönüştürülen veriler için bir çıktı sahiptir ve analiz ettiğiniz bilgilere yanıt olarak neler denetleyebilirsiniz. Örneğin, şunları yapabilirsiniz:

* Veri Hizmetleri iletişimleri ya da özel iş akışlarını aşağı yönde tetiklemek için Azure işlevleri, Service Bus konuları veya kuyruklar gibi gönderebilirsiniz.
* Gerçek zamanlı yönelik Kompozit için Power BI panosuna veri gönderebilirsiniz.
* Bir machine learning geçmiş verileri temel alan modeli eğitme veya toplu analiz gerçekleştirmek için diğer Azure depolama hizmetleri Data Store.

Aşağıdaki görüntüde verilerin Stream Analytics'e gönderilen, analiz ve depolama ya da sunum gibi diğer eylemler için gönderilen gösterilmektedir:

![Stream Analytics giriş işlem hattı](./media/stream-analytics-introduction/stream-analytics-intro-pipeline.png)

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar

Azure Stream Analytics kullanımı kolay, esnek, güvenilir ve her boyuttaki iş için ölçeklenebilir olacak şekilde tasarlanmıştır. Azure bölgelerinde kullanılabilir. Aşağıdaki görüntüde Azure Stream Analytics temel özellikleri gösterilmektedir:

![Stream Analytics temel özellikleri](./media/stream-analytics-introduction/stream-analytics-key-capabilities.png)

## <a name="ease-of-getting-started"></a>Başlama kolaylığı

Azure Stream Analytics, başlatmak kolay bir işlemdir. Yalnızca birkaç tıklamayla çeşitli kaynaklar ve havuzlar, için uçtan uca bir işlem hattı oluşturma bağlamak için alır. Stream Analytics bağlanabilir [Azure Event Hubs](/azure/event-hubs/) ve [Azure IOT hub'ı](/azure/iot-hub/) akış verilerini almak için de [Azure Blob Depolama](/azure/storage/storage-introduction) geçmiş verilerin alımı için. İş girdisi, statik veya yavaş değişen başvuru verilerini Azure Blob Depolama ayrıca içerebilir veya [SQL veritabanı](stream-analytics-use-reference-data.md#azure-sql-database) arama işlemleri gerçekleştirmek için veri akış katılabilirsiniz.

Stream Analytics yönlendirebilir iş çıktısı için birçok depolama sistemleri gibi [Azure Blob Depolama](/azure/storage/storage-introduction), [Azure SQL veritabanı](/azure/sql-database/), [Azure Data Lake Store](/azure/data-lake-store/), ve [Azure CosmosDB](/azure/cosmos-db/introduction). Saklı çıktıyı Azure HDInsight ile toplu analiz çalıştırabilir veya çıktıyı tüketim için Event Hubs gibi başka bir hizmete gönderebilirsiniz veya [Power BI](https://docs.microsoft.com/power-bi/) gerçek zamanlı görselleştirme için.

Stream Analytics çıktıların tamamı listesi için bkz [anlamak Azure Stream Analytics çıkışları](stream-analytics-define-outputs.md).

## <a name="programmer-productivity"></a>Programcı üretkenliği

Azure Stream Analytics, Hareket halindeki verileri çözümlemek için güçlü geçici kısıtlamalarla büyütülmüş bir basit SQL tabanlı sorgu dili kullanır. İş dönüşümlerini tanımlamak için, basit SQL yapıları kullanarak karmaşık geçici sorgular ve analizler yazmanıza olanak tanıyan basit, bildirim temelli bir [Stream Analytics sorgu dili](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) kullanırsınız. Stream Analytics sorgu dili SQL dili için tutarlı olduğundan, tanıdık SQL ile işleri oluşturmaya başlamak yeterli olur. Azure PowerShell gibi geliştirici araçlarını kullanarak işleri oluşturabilirsiniz [Stream Analytics Visual Studio Araçları](stream-analytics-tools-for-visual-studio-install.md), [Stream Analytics Visual Studio Code uzantısı](quick-create-vs-code.md), veya Azure Resource Manager şablonları . Geliştirici araçlarının kullanılması, çevrimdışı ortamda dönüşüm sorguları geliştirmenize ve [CI/CD işlem hattı](stream-analytics-tools-for-visual-studio-cicd.md) kullanarak Azure’a iş göndermenize olanak tanır.

Stream Analytics sorgu dili, akış verilerini işleme ve çözümleme için işlevleri geniş bir yelpazede sunar. Bu sorgu dili basit veri işleme, toplama işlevlerini ve karmaşık Jeo-uzamsal işlevleri destekler. Portalda sorguları düzenleyin ve bunları test Canlı akıştan ayıklanan örnek verileri kullanarak.

Ek işlevler tanımlayıp çağırarak sorgu dilinin yapabileceklerini artırabilirsiniz. Azure Machine Learning çözümlerinden yararlanmak için Azure Machine Learning hizmetinde işlev çağrıları tanımlayın ve JavaScript tümleştirin veya C# kullanıcı tanımlı işlevlerle (UDF) veya bir parçası olarak karmaşık hesaplamalar gerçekleştirmek için kullanıcı tanımlı toplamlarda bir Stream Analytics sorgu.

## <a name="fully-managed"></a>Tam olarak yönetilir

Azure Stream Analytics, Azure üzerinde tam olarak yönetilen bir sunucusuz (PaaS) tekliftir. İşlerinizi çalıştırmak için kümeleri yönetme ya da herhangi bir donanım sağlamanız gerekmez. Azure Stream Analytics, bulutta karmaşık işlem kümeleri ayarlama ve alma işi çalıştırmak için gereken ayar performansını önemli işinizi tam olarak yönetir. Azure Event Hubs'a ve Azure IOT Hub ile tümleştirme, iş başına bağlı cihazlar, tıklama dizileri, dahil etmek ve günlük dosyaları için kaynaklar, çeşitli kaynaklardan gelen, saniyede milyonlarca olayı içe alacak şekilde sağlar. Olay hub'larının bölümleme özelliğini kullanarak hesaplamaları mantıksal adımlara, her daha da fazla olanağı ile ölçeklenebilirliği artırmak için.

## <a name="run-in-the-cloud-or-on-the-intelligent-edge"></a>Akıllı uç veya bulutta çalıştırın

Azure Stream Analytics, bulutta büyük ölçekli analiz çalıştırabilir veya son derece düşük gecikme süresi analiz için IOT Edge üzerinde çalıştırın. Azure Stream Analytics akış işleme için karma mimari gerçekten oluşturmak üzere geliştiricilere etkinleştirme hem buluttaki hem de edge, aynı sorgu dili kullanır. 

## <a name="low-total-cost-of-ownership"></a>Düşük toplam sahip olma maliyeti

Bir bulut hizmeti olan Stream Analytics, maliyet için iyileştirilmiştir. Hiçbir ön maliyet dahil - yalnızca için ödeme yaparsınız [akış birimleri tükettiğiniz](stream-analytics-streaming-unit-consumption.md)ve işlenen veri miktarı. Taahhüt veya küme sağlama gerekli yoktur ve iş gereksinimlerinize göre yukarı veya aşağı iş ölçeklendirebilirsiniz.

## <a name="mission-critical-ready"></a>Görev açısından kritik hazır

Azure Stream Analytics, birden çok bölgede dünya çapında kullanılabilir ve güvenilirlik, güvenlik ve uyumluluk gereksinimlerini destekleyen görev açısından kritik iş yüklerini çalıştırmak için tasarlanmıştır.

### <a name="reliability"></a>Güvenilirlik

Azure Stream Analytics garantiler-sonra bunu olayları hiç olay işleme ve olay teslimini en az bir kez kaybolur. Tam olarak-bir kez işlemeyi garanti edilir ile seçilen çıkış açıklandığı [olay teslimat Garantileriyle](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics).

Azure Stream Analytics, bir olayın tesliminin başarısız olması durumunda yerleşik kurtarma özellikleri vardır. Stream Analytics işinizin durumunu korumak üzere yerleşik denetim noktaları sağlar ve tekrarlanabilir sonuçlar sunar.

Yönetilen bir hizmet olarak Stream Analytics, olay işleme ile dakikalık bir ayrıntı düzeyini, % 99,9 kullanılabilirlik garanti eder. Daha fazla bilgi için [Stream Analytics SLA](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/) sayfası. 

### <a name="security"></a>Güvenlik

Güvenlik açısından Azure Stream Analytics, tüm gelen ve giden iletişimi şifreler ve TLS 1.2 destekler. Yerleşik denetim noktaları de şifrelenir. Stream Analytics, tüm işlem, bellek içi bittikten sonra gelen verileri depolamaz.

### <a name="compliance"></a>Uyumluluk

Azure Stream Analytics, birden çok uyumluluk sertifikaları açıklandığı izleyen [Azure uyumluluk bakış](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 

## <a name="performance"></a>Performans

Stream Analytics, saniyede milyonlarca olay işleyebilir ve sonuçları ultra düşük gecikme süreleriyle sunabilir. Ölçek büyütme ve büyük gerçek zamanlı ve karmaşık olay işleme uygulamalarını kullanmak için ölçek genişletme olanak tanır. Stream Analytics daha yüksek performans bölümleme tarafından paralel ve birden çok akış düğümünde yürütülebilir karmaşık sorgular destekler. Azure Stream Analytics, üzerine kurulmuştur [Trill](https://github.com/Microsoft/Trill), yüksek performanslı bellek içi akış analiz altyapısı geliştirilen Microsoft Research işbirliğiyle.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stream Analytics’e genel bakışı gördünüz. Bundan sonra derinlere inerek ilk Stream Analytics işinizi oluşturabilirsiniz:

* [Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md).
* [Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md).
* [Visual Studio kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md).
* [Visual Studio Code kullanarak bir Stream Analytics işi oluşturma](quick-create-vs-code.md).
