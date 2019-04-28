---
title: Azure Stream Analytics’e Genel Bakış
description: Nesnelerin İnterneti'nden (IoT) sağlanan akış verilerini gerçek zamanlı olarak analiz etmenize yardım eden bir yönetilen hizmet olan Stream Analytics hakkında bilgi edinin.
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: overview
ms.custom: seodec18
ms.date: 12/07/2018
ms.openlocfilehash: 09f402f81700b53eb9e4a95e36545ef02850660a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479720"
---
# <a name="what-is-azure-stream-analytics"></a>Azure Akış Analizi Nedir?

Azure Stream Analytics, cihazlardan yüksek hacimlerde veri akışı incelemenize olanak tanıyan bir olay işleme altyapısıdır. Gelen veriler cihazlardan, sensörlerden, web sitelerinden, sosyal medya akışlarından, uygulamalardan ve çok daha fazlasından alınabilir. Ayrıca, veri akışlarından bilgi ayıklamayı, desenleri ve ilişkileri tanımlamayı destekler. Ardından, diğer aşağı yönde eylemleri tetiklemek, gibi uyarılar oluşturma, bir raporlama aracına bilgi akışı veya daha sonra kullanmak üzere depolamak için bu desenleri kullanabilirsiniz.

Azure Stream Analytics’in kullanılabildiği bazı örnekler aşağıda verilmiştir: 

* Nesnelerin İnterneti (IoT) Sensör füzyonu ve cihaz telemetrisi üzerinde gerçek zamanlı analizler
* Web günlükleri/clickstream analizi
* Filo yönetimi ve sürücüsüz araçlar için jeo-uzamsal analiz
* Yüksek değerli varlıklar için uzaktan izleme ve tahmine dayalı bakım
* Envanter denetimi ve anomali algılama için Satış Noktasında gerçek zamanlı analizler

## <a name="how-does-stream-analytics-work"></a>Stream Analytics nasıl çalışır?

Azure Stream Analytics; Azure Event Hub, Azure IoT Hub’a veya Azure Blob Depolama gibi bir veri deposundan alınan bir akış verileri kaynağı ile başlar. Akışları incelemek için veri akışı yapan giriş kaynağını belirten bir Stream Analytics işi oluşturursunuz. İş ayrıca verilerin, desenlerin veya ilişkilerin arama şeklini tanımlayan bir dönüşüm sorgusu belirtir. Dönüşüm sorgusunu kolayca filtre, sıralama, toplama ve bir zaman aralığında toplanan akış verilerinin birleştirme SQL sorgu dili kullanır. İşi yürütürken olay sıralama seçeneklerini ve toplama işlemlerini gerçekleştirirken zaman pencerelerinin süresini ayarlayabilirsiniz.

Gelen verileri analiz ettikten sonra dönüştürülen verilerin bir çıktısını belirtebilir ve analiz ettiğiniz bilgilere yanıt olarak neler yapılacağını denetleyebilirsiniz. Örneğin, aşağıdaki gibi eylemler gerçekleştirebilirsiniz:

* Uyarıları tetikleyin veya özel iş akışlarını aşağı yönde izlenen bir kuyruğa veri gönderebilirsiniz.
* Gerçek zamanlı görselleştirme için bir Power BI panosuna veri gönderme.
* Bir machine learning geçmiş verileri temel alan modeli eğitme veya toplu analiz gerçekleştirmek için diğer Azure depolama hizmetlerine verileri Store.

Aşağıdaki görüntüde Stream Analytics işlem hattı gösterilmektedir. Stream Analytics işiniz tümünü veya giriş ve çıkış yapması durumunda seçili bir kümeyi kullanabilir. Bu görüntüde verilerin Stream Analytics’e gönderilmesi, analiz edilmesi ve depolama ya da sunum gibi diğer eylemler için gönderilmesi gösterilmiştir:

![Stream Analytics giriş işlem hattı](./media/stream-analytics-introduction/stream-analytics-intro-pipeline.png)

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar

Azure Stream Analytics kullanımı kolay, esnek, güvenilir ve her boyuttaki iş için ölçeklenebilir olacak şekilde tasarlanmıştır. Azure bölgelerinde kullanılabilir. Aşağıdaki görüntüde Azure Stream Analytics’in temel özellikleri gösterilmiştir:

![Stream Analytics temel özellikleri](./media/stream-analytics-introduction/stream-analytics-key-capabilities.png)

## <a name="ease-of-getting-started"></a>Başlama kolaylığı

Azure Stream Analytics’i kullanmaya başlamak kolaydır. Yalnızca birkaç tıklama ile birden fazla kaynağa ve havuza bağlanılabilir ve uçtan uca bir işlem hattı oluşturulabilir. Stream Analytics, akış verilerini almak için [Azure Event Hubs](/azure/event-hubs/), [Azure IoT Hub](/azure/iot-hub/)’a bağlanabilir. Ayrıca, geçmiş verileri almak için [Azure Blob depolama](/azure/storage/storage-introduction) hizmetine de bağlanabilir. Olay hub'larından gelen verileri diğer veri kaynakları ve işleme altyapıları ile birleştirebilir. İş girdileri, statik veya yavaş değişen verilerden oluşan başvuru verileri de içerebilir ve arama işlemleri gerçekleştirmek için akış verilerini bu başvuru verilerine ekleyebilirsiniz.

Stream Analytics, iş çıktısını [Azure Blob](/azure/storage/storage-introduction), [Azure SQL Veritabanı](/azure/sql-database/), [Azure Data Lake Stores](/azure/data-lake-store/) veya [Azure Cosmos DB](/azure/cosmos-db/introduction) gibi çok sayıda depolama sistemine yönlendirebilir. Depolama sonrasında, Azure HDInsight ile toplu analiz çalıştırmanıza veya başka bir hizmete veya çok tüketim için event hubs gibi çıkış göndermek [Power BI](https://docs.microsoft.com/power-bi/) Power BI akış API'sini kullanarak gerçek zamanlı görselleştirme için.

## <a name="programmer-productivity"></a>Programcı Verimliliği

Azure Stream Analytics, hareket halindeki verileri çözümlemek için güçlü geçici kısıtlamalarla büyütülmüş basit bir SQL tabanlı sorgu dili kullanır. İş dönüşümlerini tanımlamak için, basit SQL yapıları kullanarak karmaşık geçici sorgular ve analizler yazmanıza olanak tanıyan basit, bildirim temelli bir [Stream Analytics sorgu dili](https://msdn.microsoft.com/library/azure/dn834998.aspx) kullanırsınız. Stream Analytics sorgu dili SQL dili ile tutarlıdır ve iş oluşturmaya başlamak için SQL dilinin tanınması yeterlidir. Ayrıca Azure PowerShell, [Stream Analytics Visual Studio araçları](stream-analytics-tools-for-visual-studio-install.md) veya Azure Resource Manager şablonları gibi geliştirici araçlarını kullanarak da iş oluşturabilirsiniz. Geliştirici araçlarının kullanılması, çevrimdışı ortamda dönüşüm sorguları geliştirmenize ve [CI/CD işlem hattı](stream-analytics-tools-for-visual-studio-cicd.md) kullanarak Azure’a iş göndermenize olanak tanır. 

Stream Analytics sorgu dili, akış verilerini analiz etme ve işlemeye yönelik çok çeşitli işlevler sunar. Bu sorgu dili basit veri işleme, toplama işlevlerini ve karmaşık jeo-uzamsal işlevleri destekler. Portalda sorguları düzenleyebilir ve canlı akıştan ayıklanan örnek verileri kullanarak test edebilirsiniz.

Ek işlevler tanımlayıp çağırarak sorgu dilinin yapabileceklerini artırabilirsiniz. Azure Machine Learning çözümlerinden yararlanmak için Azure Machine Learning hizmetinde işlev çağrıları tanımlayabilir ve bir Stream Analytics sorgusunun parçası olarak karmaşık hesaplamalar gerçekleştirmek üzere JavaScript kullanıcı tanımlı işlevlerini (UDF) ya da kullanıcı tanımlı toplamları tümleştirebilirsiniz.

## <a name="fully-managed"></a>Tam olarak yönetilir 

Azure Stream Analytics, Azure üzerinde tam olarak yönetilen bir sunucusuz (PaaS) tekliftir. Diğer bir deyişle, işlerinizi çalıştırmak için herhangi bir donanım sağlamanız veya kümeleri yönetmeniz gerekmez. Azure Stream Analytics, bulutta karmaşık işlem kümeleri oluşturma işlemini ve işi çalıştırmak için gereken performans ayarını yaparak işinizi tam olarak yönetir. Azure Event Hubs ve Azure IoT Hub ile tümleştirme; bağlı cihazlar, tıklama dizileri ve günlük dosyaları gibi çeşitli kaynaklardan gelen, saniyede milyonlarca olayın işler tarafından alınmasını sağlar. Olay hub'larının bölümleme özelliğini kullanarak hesaplamaları mantıksal adımlara ayırabilir, her birini bölümlere ayırarak ölçeklenebilirliği artırabilirsiniz.

## <a name="run-in-the-cloud-on-in-the-intelligent-edge"></a>Akıllı bir ucu bulut üzerinde çalıştığı

Azure Stream Analytics, bulutta büyük ölçekli analiz çalıştırabilir veya son derece düşük gecikme süresi analiz için akıllı bir ucu çalıştırın.
Azure Stream Analytics akış işleme için karma mimari gerçekten oluşturmak üzere geliştiricilere etkinleştirme hem buluttaki hem de akıllı bir ucu aynı sorgu dili kullanır.

## <a name="low-total-cost-of-ownership"></a>Toplam Sahip Olma Maliyetinin Düşüklüğü

Bir bulut hizmeti olan Stream Analytics, maliyet için iyileştirilmiştir. Herhangi bir ön maliyet yoktur; yalnızca [kullandığınız akış birimleri](stream-analytics-streaming-unit-consumption.md) ve işlenen veri miktarı için ödeme yaparsınız. Herhangi bir taahhüt veya küme sağlama gerekli değildir. İş gereksinimlerinize göre akış işlerinizin ölçeğini artırabilir veya azaltabilirsiniz. 

## <a name="mission-critical-ready"></a>Görev açısından kritik hazır
Azure Stream Analytics, birden çok bölgede dünya çapında kullanılabilir ve güvenilirlik, güvenlik ve uyumluluk gereksinimlerini destekleyen görev açısından kritik iş yüklerini çalıştırmak için tasarlanmıştır.
### <a name="reliability"></a>Güvenilirlik
Azure Stream Analytics garantiler-sonra bunu olayları hiç olay işleme ve olay teslimini en az bir kez kaybolur. Tam olarak-bir kez işlemeyi garanti edilir ile seçilen çıkış açıklandığı [olay teslimat Garantileriyle](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics).
Azure Stream Analytics, bir olayın tesliminin başarısız olması durumunda yerleşik kurtarma özellikleri vardır. Ayrıca, Stream Analytics işinizin durumunu korumak üzere yerleşik denetim noktası sağlar ve tekrarlanabilir sonuçlar sunar.

Yönetilen bir hizmet olarak Stream Analytics, olay işleme ile dakika düzeyinde % 99,9 kullanılabilirlik garanti eder. Daha fazla bilgi için [Stream Analytics SLA](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/) daha fazla ayrıntı için. 

### <a name="security"></a>Güvenlik
Güvenlik açısından Azure Stream Analytics, tüm gelen ve giden iletişimi şifreler ve TLS 1.2 destekler. Yerleşik denetim noktaları de şifrelenir. Stream Analytics, tüm işlem, bellek içi bittikten sonra gelen verileri depolamaz. 

### <a name="compliance"></a>Uyumluluk
Azure Stream Analytics, birden çok uyumluluk sertifikaları açıklandığı izleyen [Azure uyumluluk bakış](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 

## <a name="performance"></a>Performans

Stream Analytics her saniyede milyonlarca olay işleyebilir ve düşük gecikme süresiyle sonuçlar sunabilir.
Ölçek büyütme ve büyük gerçek zamanlı ve karmaşık olay işleme uygulamalarını kullanmak için ölçek genişletme olanak tanır. Stream Analytics bölümleme yoluyla performansı, paralel ve birden çok akış düğümünde yürütülebilir karmaşık sorgular izin vererek destekler.
Azure Stream Analytics, üzerine kurulmuştur [Trill](https://github.com/Microsoft/Trill), yüksek performanslı bellek içi akış analiz altyapısı geliştirilen Microsoft Research işbirliğiyle. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Stream Analytics’e genel bakışı gördünüz. Bundan sonra derinlere inerek ilk Stream Analytics işinizi oluşturabilirsiniz:

* [Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md).
* [Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md).
* [Visual Studio kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md).

