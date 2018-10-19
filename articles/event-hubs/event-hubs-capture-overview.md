---
title: Azure Event Hubs'a genel bakış yakalama | Microsoft Docs
description: Event Hubs yakalama ile telemetri verilerini yakalama
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 91263d7f6e8c0c2774df560a45eeff9efc672343
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408433"
---
# <a name="azure-event-hubs-capture"></a>Azure Event Hubs yakalama

Azure Event Hubs yakalama, sağlar, Event hubs'dan akış verilerini otomatik olarak teslim etmek bir [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) veya [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) eklenen esnekliğini tercih ettiğiniz Hesapla bir zaman veya boyut aralığı belirtme. Yakalama ayarı hızlı, çalıştırmak için hiçbir yönetim maliyeti yoktur ve Event Hubs ile otomatik olarak ölçeklenen [üretilen iş birimleri](event-hubs-features.md#capacity). Event Hubs yakalama, Azure'a akış verileri yüklemenin en kolay yoludur ve veri yakalama yerine veri işleme odaklanmasını sağlar.

Event Hubs yakalama, aynı akışta gerçek zamanlı ve toplu işlem tabanlı işlem hatlarını işlemenizi sağlar. Başka bir deyişle, gereksinimlerinizi zamanla ile büyütün çözümleri oluşturabilirsiniz. Event Hubs yakalama batch tabanlı sistemlerde göz önünde doğrultusunda gelecekteki gerçek zamanlı işleme ile bugün oluşturuyorsunuz ve verimli bir Durgun yol var olan bir gerçek zamanlı çözüme eklemek istediğiniz olsun, akış verileri daha kolay ile çalışma hale getirir.

## <a name="how-event-hubs-capture-works"></a>Event Hubs yakalama nasıl çalışır

Event Hubs, telemetri giriş, dağıtılmış bir günlük için benzer bir saklama süresi dayanıklı arabellek. Olay hub'ları, ölçeklendirme için anahtar [bölümlenmiş tüketici modelinin](event-hubs-features.md#partitions). Her bölüm veri bağımsız bir parçası olan ve bağımsız olarak kullanılır. Zaman içinde yapılandırılabilir bekletme aralığını temel kapalı, bu verileri eskir. Sonuç olarak, belirli bir olay hub'ı hiçbir zaman "dolu." alır

Event Hubs yakalama, kendi Azure Blob Depolama hesabı ve kapsayıcı veya yakalanan verileri depolamak için kullanılan Azure Data Lake Store hesabı belirtmenizi sağlar. Bu hesaplar, olay hub'ı ile aynı bölgede veya başka bir bölgede Event Hubs yakalama özelliğini esnekliğini ekleme olabilir.

Yakalanan verileri yazılmış [Apache Avro] [ Apache Avro] biçimi: satır içi şema ile zengin veri yapılarını sağlar compact, hızlı ve ikili biçimi. Bu biçim, Hadoop ekosistemi, Stream Analytics ve Azure Data Factory yaygın olarak kullanılır. Avro ile çalışma hakkında daha fazla bilgi makalenin sonraki bölümlerinde kullanılabilir.

### <a name="capture-windowing"></a>Pencereleme yakalama

Event Hubs yakalama, yakalama denetlemek için bir penceresi ayarlama sağlar. Bu, en küçük boyut ve zaman yapılandırma İlkesi'yle "karşılaşılan ilk tetikleyici bir yakalama işlemi neden olduğunu, yani ilk WINS," penceredir. On beş dakikalık varsa, 100 MB Yakala ve 1 MB saniye başına zaman penceresi önce boyutu pencere Tetikleyicileri gönderin. Her bölüm bağımsız olarak yakalar ve tamamlanan blok blobu, yakalama aralığı karşılaşıldı süredir adlı yakalama, zamanında yazar. Depolama adlandırma kuralı aşağıdaki gibidir:

```
{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}
```

Tarih değerlerini sıfırlarla doldurulur dikkat edin. bir örnek dosya adı olabilir:

```
https://mystorageaccount.blob.core.windows.net/mycontainer/mynamespace/myeventhub/0/2017/12/08/03/03/17.avro
```

### <a name="scaling-to-throughput-units"></a>İşleme birimleri ile ölçeklendirme

Olay hub'ları trafiği tarafından denetlenir [üretilen iş birimleri](event-hubs-features.md#capacity). Tek bir işleme birimi, ikinci veya 1000 olay giriş ve çıkış miktarı iki kez saniye başına 1 MB sağlar. Standart Event Hubs, 1-20 üretilen iş birimleri ile yapılandırılabilir ve daha fazlasını satın alabilirsiniz ile kota artırma [destek isteği][support request]. Kullanım, satın alınan işleme birimlerinin ötesinde kısıtlanır. Event Hubs yakalama doğrudan İç olay hub'ları depolama alanındaki verileri, aktarım hızı birimi çıkış kotaları atlayarak ve Stream Analytics veya Spark gibi diğer işleme okuyucular için çıkış kaydetme kopyalar.

Event Hubs yakalama yapılandırıldıktan sonra ilk olay gönderdiğinizde, otomatik olarak çalışır ve çalışmaya devam eder. İşlemin çalıştığını bilmek, aşağı akış işleme kolaylaştırmak için hiçbir veri olduğunda olay hub'ları boş dosyalarını yazar. Bu işlem, tahmin edilebilir temposu ve, batch işlemci besleyebilecek işaret sunar.

## <a name="setting-up-event-hubs-capture"></a>Event Hubs yakalama özelliğini ayarlama

Olay hub'ı kullanarak, oluşturma sırasında yakalama özelliğini yapılandırabilirsiniz [Azure portalında](https://portal.azure.com), veya Azure Resource Manager şablonlarını kullanarak. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure portalını kullanarak Event Hubs yakalama özelliğini etkinleştirme](event-hubs-capture-enable-through-portal.md)
- [Bir olay hub'ı ile bir Event Hubs ad alanı oluşturma ve bir Azure Resource Manager şablonu kullanarak yakalamayı etkinleştirme](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-the-captured-files-and-working-with-avro"></a>Yakalanan dosyaları keşfetme ve Avro ile çalışma

Event Hubs yakalama, yapılandırılmış zaman penceresi belirtildiği gibi Avro biçiminde dosyaları oluşturur. Bu dosyalar gibi herhangi bir aracı görüntüleyebilirsiniz [Azure Depolama Gezgini][Azure Storage Explorer]. Yerel olarak çalışacak şekilde dosyalarını indirebilirsiniz.

Event Hubs yakalama tarafından üretilen dosyalar aşağıdaki Avro şemanın vardır:

![][3]

Avro dosyalarının keşfetmek için kolay bir yol kullanmaktır [Avro Araçları] [ Avro Tools] jar Apache öğesinden. Bu jar'ı indirdikten sonra aşağıdaki komutu çalıştırarak belirli bir Avro dosya şeması görebilirsiniz:

```shell
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

Bu komut döndürür

```json
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Avro araçları, dosya JSON biçimine dönüştürün ve başka bir işlem gerçekleştirmek için de kullanabilirsiniz.

Daha gelişmiş işleme gerçekleştirmek için indirin ve kendi seçtiğiniz platform için Avro yükleyin. Bu makalenin yazıldığı sırada, kullanılabilir uygulamalar için C, C++, C\#, Java, NodeJS, Perl, PHP, Python ve Ruby.

Apache Avro için tam Başlarken kılavuzları sahip [Java] [ Java] ve [Python][Python]. Ayrıca okuyabilirsiniz [Event Hubs yakalama ile çalışmaya başlama](event-hubs-capture-python.md) makalesi.

## <a name="how-event-hubs-capture-is-charged"></a>Event Hubs yakalama nasıl ücretlendirilir

Event Hubs yakalama ölçülen benzer şekilde üretilen iş birimleri: saatlik bir ücret olarak. Ücretlendirme, ad alanı için satın alınan işleme birimlerinin sayısı doğrudan orantılıdır. İşleme birimleri artan ve azalan olarak Event Hubs yakalama ölçümleri artırın ve eşleşen performans sağlayacak şekilde azaltın. Ölçümleri dağıtımınızla oluşur. Fiyatlandırma ayrıntıları için bkz: [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/). 

## <a name="integration-with-event-grid"></a>Event Grid ile tümleştirme 
Bir Event Hubs ad alanı ile kaynağı olarak Azure Event Grid aboneliği oluşturabilirsiniz. Aşağıdaki öğreticide kaynağı olarak bir olay hub'ı ve havuz olarak bir Azure işlev uygulaması ile bir Event Grid aboneliği oluşturmak gösterilmektedir: [işlem ve Event Grid ve Azure işlevlerikullanarakbirSQLveriambarıyakalananEventHubsverilerinigeçirme](store-captured-data-data-warehouse.md).


## <a name="next-steps"></a>Sonraki adımlar

Event Hubs yakalama, Azure'a veri almak için en kolay yoludur. Azure Data Lake, Azure Data Factory ve Azure HDInsight'ı kullanarak, toplu işlem gerçekleştirebilir ve dilediğiniz ölçekte alışık olduğunuz araçları ve seçtiğiniz platformları kullanarak, diğer analiz gerekir.

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olayları gönderip kullanmaya başlayın](event-hubs-dotnet-framework-getstarted-send.md)
* [Event Hubs'a genel bakış][Event Hubs overview]

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
