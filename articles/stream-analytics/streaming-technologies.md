---
title: Gerçek zamanlı analiz ve işleme teknolojisi azure'da akış seçin
description: Şu gerçek zamanlı analizler ve azure'da uygulamanızı oluşturmak üzere akış işleme teknolojisinin seçme hakkında bilgi edinin.
author: zhongc
ms.author: zhongc
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: f46a35d971c008b61d4899e30101ea562d3cefea
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483403"
---
# <a name="choose-a-real-time-analytics-and-streaming-processing-technology-on-azure"></a>Gerçek zamanlı analiz ve işleme teknolojisi azure'da akış seçin

Gerçek zamanlı analizler ve azure'da akış işleme için kullanılabilen birkaç hizmet mevcuttur. Bu makalede, uygulamanız için en uygun teknolojiyi olduğuna karar vermek ihtiyacınız olan bilgileri sağlar.

## <a name="when-to-use-azure-stream-analytics"></a>Azure Stream Analytics'i ne zaman

Azure Stream Analytics, Azure stream analytics için önerilen hizmetidir. Çok çeşitli dahil ancak bunlarla sınırlı olmayan senaryolar için tasarlanmıştır:

* Panolar için veri Görselleştirme
* Gerçek zamanlı [uyarılar](stream-analytics-set-up-alerts.md) zamana bağlı ve uzamsal desenleri veya anomalileri
* Ayıklama, Dönüştürme, Yükleme (ETL)
* [Olay kaynağını belirleme düzeni](/azure/architecture/patterns/event-sourcing)
* [IoT Edge](stream-analytics-edge.md)

Bir Azure Stream Analytics ekleme iş uygulamanıza Yukarı Akış analizi en hızlı yoludur ve SQL dilini kullanarak Azure'da çalışan zaten biliyorsunuz. Azure Stream Analytics işi hizmet zaman yönetme kümeleri harcamanız gerekmez ve iş düzeyinde % 99,9 SLA ile kapalı kalma süresi hakkında endişelenmeniz gerekmez olduğundan. Faturalandırma, başlangıç maliyetleri düşük yapma proje düzeyinde (bir akış birimi), ölçeklenebilir (en fazla akış birimi 192) ancak da gerçekleştirilir. Bu çok çalıştırın ve bir küme olduğundan bazı Stream Analytics işlerini çalıştırmak için uygun maliyetlidir.

Azure Stream Analytics, zengin bir giden kutusu deneyimi vardır. Hemen başka bir kurulum yapmadan aşağıdaki özelliklerden yararlanabilirsiniz:

* Yerleşik geçici işleçler gibi [pencereli toplamlar](stream-analytics-window-functions.md), zamana bağlı birleştirmeler ve geçici analiz işlevleri.
* Yerel Azure [giriş](stream-analytics-add-inputs.md) ve [çıkış](stream-analytics-define-outputs.md) bağdaştırıcıları
* Yavaş değiştirmek için destek [başvuru verileri](stream-analytics-use-reference-data.md) (olarak da bilinen bir arama tabloları), bölge sınırlaması için Jeo-uzamsal başvuru verileriyle birleştirme dahil.
* Çözümleri gibi tümleşik [Anomali algılama](stream-analytics-machine-learning-anomaly-detection.md)
* Aynı sorguda birden çok zaman pencereleri
* Birden çok geçici işleçler rastgele serileri oluşturma olanağı.
* 100 ms altında gelen ve olay hub'ları, ağ gecikmesi Sürdürülen üretilen yüksek iş düzeylerinde olay hub'larındaki giriş çıktısı için Event Hubs, gelen girişten alınan uçtan uca gecikme süresi dahil olmak üzere

## <a name="when-to-use-other-technologies"></a>Ne zaman diğer teknolojileri kullanın

### <a name="you-need-to-input-from-or-output-to-kafka"></a>Gelen giriş veya çıkış kafka'ya gerekir

Azure Stream Analytics Apache Kafka'ya giriş veya çıkış bağdaştırıcısı değil. İçinde giriş olayları veya Kafka'ya göndermeniz gerekir ve kendi Kafka kümesini çalıştırmak için bir gereksinim yoktur, Stream Analytics, Event hubs'a olay gönderenin değiştirmeksizin Event Hubs Kafka API'si kullanarak olay göndermeye kullanmaya devam edebilirsiniz. Kendi Kafka kümesi çalıştırmanız gerekiyorsa, Spark yapılandırılmış tam olarak desteklenir, akış, kullanabilirsiniz [Azure Databricks](../azure-databricks/index.yml), veya üzerinde Storm [Azure HDInsight](../hdinsight/storm/apache-storm-overview.md).

### <a name="you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c"></a>UDF Uda'lar ve özel deserializers JavaScript dışındaki bir dilde yazmak istediğiniz veyaC#

Azure Stream Analytics işleri için bulut kullanıcı tanımlı işlevler (UDF) veya kullanıcı tanımlı toplamlarda (UDA) JavaScript'te destekler ve C# IOT Edge işler. C#Kullanıcı tanımlı deserializers de desteklenir. Java veya Python gibi diğer dillerdeki bir seri durumdan çıkarıcı, bir UDF ve UDA uygulamak istiyorsanız, Spark yapılandırılmış akışı kullanabilirsiniz. Event hubs'ı da çalıştırabilirsiniz **EventProcessorHost** rastgele bir akış işleme yapmak için kendi sanal makinelerde.

### <a name="your-solution-is-in-a-multi-cloud-or-on-premises-environment"></a>Bir çoklu Bulut veya şirket içi ortamda bir çözümdür

Azure Stream Analytics, Microsoft'un mülkiyet teknolojisidir ve yalnızca Azure'da kullanıma sunuldu. Çözümünüzü Bulut veya şirket içi arasında taşınabilir olması gerekiyorsa, Spark yapılandırılmış akış veya Storm gibi açık kaynak teknolojileri göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Visual Studio kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Visual Studio Code kullanarak bir Stream Analytics işi oluşturma](quick-create-vs-code.md)