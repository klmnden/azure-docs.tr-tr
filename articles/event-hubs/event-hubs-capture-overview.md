---
title: "Azure Event Hubs'a genel bakış yakalama | Microsoft Docs"
description: "Olay hub'ları yakalama telemetri verilerini yakalama"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: sethm;darosa
ms.openlocfilehash: fbd4aef62891341ad3760b74cd8aaee7abf7b827
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="azure-event-hubs-capture"></a>Azure Event Hubs yakalama

Azure olay hub'ları yakalama, sağlar, Event Hubs'a veri akışı bir otomatik olarak göndermeyi bir [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) veya [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) eklenen esnekliğini tercih ettiğiniz hesabıyla bir kez veya boyut aralığı belirtme. Yakalama kurduğunuzda ayardır hızlı, çalıştırmak için yönetim maliyetleri olmadan vardır ve Event Hubs ile otomatik olarak ölçeklendirir [üretilen iş birimleri](event-hubs-features.md#capacity). Olay hub'ları yakalama akış verileri Azure'a yüklemek için en kolay yoludur ve odaklanmak veri işleme yerine veri yakalama sağlar.

Olay hub'ları yakalama aynı akışını gerçek zamanlı ve toplu tabanlı ardışık düzen işleme sağlar. Bu, zaman içinde gereksinimlerinizi ile büyümesine çözümleri oluşturmanıza anlamına gelir. Toplu işlem tabanlı sistemleriyle bugün gelecekteki gerçek zamanlı işleme doğru bir göz oluşturmakta olduğunuz veya varolan bir gerçek zamanlı çözümü bir etkin yolunuzda eklemek istediğiniz olay hub'ları yakalamak daha kolay veri akış ile çalışma hale getirir.

## <a name="how-event-hubs-capture-works"></a>Olay hub'ları yakalama nasıl çalışır

Olay hub'ları telemetri giriş, dağıtılmış günlüğe benzer bir süre bekletme dayanıklı arabellek olur. Olay hub ' Ölçeklendirmesi anahtar [bölümlenmiş tüketici modelinin](event-hubs-features.md#partitions). Her bölüm veri bağımsız bir parçasıdır ve bağımsız olarak kullanılır. Zaman içinde yapılandırılabilir saklama dönemi dayalı kapalı, bu verileri eskir. Sonuç olarak, belirli olay hub'ı hiçbir zaman "dolu." alır

Olay hub'ları yakalama kendi Azure Blob Depolama hesabı ve kapsayıcı ya da yakalanan verileri depolamak için kullanılan Azure Data Lake Store hesabı belirtmenizi sağlar. Bu hesaplar, olay hub'ı ile aynı bölgede ya da başka bir bölgede olay hub'ları yakalama özelliği esnekliğini ekleme olabilir.

Yakalanan verileri yazılmış [Apache Avro] [ Apache Avro] biçimi: satır içi şeması ile zengin veri yapılarını sağlar compact, hızlı ve ikili biçimi. Bu biçim, Hadoop ekosistemi, akış analizi ve Azure Data Factory yaygın olarak kullanılır. Avro ile çalışma hakkında daha fazla bilgi, bu makalenin sonraki bölümlerinde kullanılabilir.

### <a name="capture-windowing"></a>Pencereleme yakalama

Olay hub'ları yakalama yakalama denetlemek için bir pencere ayarlamanıza olanak tanır. Bu, en küçük boyut ve zaman yapılandırma İlkesi'yle "karşılaştı ilk tetikleyici bir yakalama işlemi neden yani ilk WINS," penceredir. On beş dakikalık varsa, 100 MB Yakalama penceresinin ve zaman penceresini önce boyutu penceresi Tetikleyicileri saniye başına 1 MB Gönder. Her bölüm bağımsız olarak yakalar ve hangi yakalama aralığı karşılaşıldı süredir adlı yakalama, aynı anda tamamlanmış blok blobu yazar. Depolama adlandırma kuralı aşağıdaki gibidir:

```
{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}
```

Tarih değerlerini sıfırlarla doldurulur unutmayın; bir örnek filename olabilir:

```
https://mystorageaccount.blob.core.windows.net/mycontainer/mynamespace/myeventhub/0/2017/12/08/03/03/17.avro
```

### <a name="scaling-to-throughput-units"></a>Üretilen iş birimleri ölçeklendirme

Olay hub'ları trafiği tarafından denetlenir [üretilen iş birimleri](event-hubs-features.md#capacity). Tek bir işleme birimi, ikinci veya 1000 olayları giriş ve çıkış miktarı iki kez saniyede başına 1 MB sağlar. Standart olay hub'ları 1-20 işleme birimleri ile yapılandırılabilir ve daha fazla satın alma ile kota artırma [destek isteği][support request]. Kullanım, satın alınan işleme birimlerine ötesinde kısıtlanır. Olay hub'ları yakalama verileri doğrudan İç olay hub'ları depolama biriminden, işleme birimi çıkış kotaları atlayarak ve akış analizi veya Spark gibi diğer işleme okuyucular için çıkış kaydetme kopyalar.

Olay hub'ları yakalama yapılandırdıktan sonra ilk olay gönderdiğinizde, otomatik olarak çalışır ve çalışmaya devam eder. İşlemin çalıştığını öğrenmek, aşağı akış işleme kolaylaştırmak için hiçbir veri olduğunda olay hub'ları boş dosyaları yazar. Bu işlem, tahmin edilebilir tempoyla ve toplu işlemciler besleyebilecek işaret sunar.

## <a name="setting-up-event-hubs-capture"></a>Olay hub'ları yakalama ayarlama

Olay hub'ı oluşturma saati kullanarak yakalama yapılandırabilirsiniz [Azure portal](https://portal.azure.com), veya Azure Resource Manager şablonları kullanarak. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure portalını kullanarak olay hub'ları yakalama etkinleştir](event-hubs-capture-enable-through-portal.md)
- [Bir event hub ile Event Hubs ad alanı oluşturmak ve bir Azure Resource Manager şablonu kullanarak yakalamayı etkinleştirme](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-the-captured-files-and-working-with-avro"></a>Yakalanan dosyaları keşfetme ve Avro ile çalışma

Olay hub'ları yakalama yapılandırılmış zaman penceresi belirtildiği gibi Avro biçimi dosyaları oluşturur. Bu dosyaları gibi herhangi bir aracı görüntüleyebilirsiniz [Azure Storage Gezgini][Azure Storage Explorer]. Dosyaları yerel olarak çalışacak şekilde yükleyebilirsiniz.

Olay hub'ları yakalama tarafından üretilen dosyalar aşağıdaki Avro şeması vardır:

![][3]

Kullanarak avro dosyalarının keşfetmek için kolay bir yoludur [Avro Araçları] [ Avro Tools] Apache gelen jar. Bu jar indirdikten sonra aşağıdaki komutu çalıştırarak belirli bir Avro dosya şeması görebilirsiniz:

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

Bu komut döndürür

```
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

Avro araçları, dosyayı JSON biçimine dönüştürmek ve başka bir işlem gerçekleştirmek için de kullanabilirsiniz.

Daha gelişmiş işleme gerçekleştirmek için karşıdan yükleyip Avro için tercih ettiğiniz bir platform. Bu yazma zaman uygulamaları C, C++, C için kullanılabilir olduğundan\#, Java, NodeJS, Perl, PHP, Python ve Ruby.

Apache Avro için tam Başlarken kılavuzları sahip [Java] [ Java] ve [Python][Python]. Ayrıca, okuyabilirsiniz [olay hub'ları yakalama ile çalışmaya başlama](event-hubs-capture-python.md) makalesi.

## <a name="how-event-hubs-capture-is-charged"></a>Olay hub'ları yakalama nasıl doludur

Olay hub'ları yakalama ölçülen benzer şekilde üretilen iş birimleri: saatlik giderleri olarak. Ücret ad alanı için satın alınan işleme birimi sayısıyla orantılıdır. Üretilen iş birimleri artırılır ve azaltılır gibi olay hub'ları yakalama ölçümler artırın ve eşleşen performans sağlamak için azaltın. Metre dağıtımınızla oluşur. Fiyatlandırma ayrıntıları için bkz: [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/). 

## <a name="next-steps"></a>Sonraki adımlar

Olay hub'ları yakalama, Azure'da veri almanın en kolay yoludur. Azure Data Lake, Azure Data Factory ve Azure Hdınsight kullanarak toplu işleme gerçekleştirebilir ve herhangi bir ölçekte tanıdık Araçlar ve seçtiğiniz platformlar kullanarak diğer analytics gerekir.

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Gönderme ve alma olayları kullanmaya başlama](event-hubs-dotnet-framework-getstarted-send.md)
* [Event Hubs'a genel bakış][Event Hubs overview]

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
