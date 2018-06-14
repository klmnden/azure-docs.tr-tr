---
title: Açık kaynaklı Araçları'nı kullanarak Azure Ağ İzleyicisi NSG akış günlükleri Görselleştirme | Microsoft Docs
description: Bu sayfa, NSG akış günlükleri görselleştirmek için açık kaynaklı araçları kullanmayı açıklar.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: f7d51352aa8411e36f4224804c90c2554d4ef9e6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29394179"
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Açık kaynaklı Araçları'nı kullanarak Azure Ağ İzleyicisi NSG akış günlükleri Görselleştirme

Ağ güvenlik grubu akış günlükleri kullanılabilir bilgi sağlar; giriş ve çıkış IP trafiği ağ güvenlik grupları anlama. Bu akış günlükleri giden ve gelen akışları başına kural olarak, akış uygulandığı NIC, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol), 5 tanımlama bilgilerini ve trafiğe izin verilen veya reddedilen varsa üzerinde gösterir.

Bu akış günlükleri el ile ayrıştırma ve ilişkin bilgiler elde zor olabilir. Ancak, bu verileri görselleştirin yardımcı olabilecek birçok açık kaynak araçları vardır. Bu makalede Esnek hızla dizini oluşturmak ve akışınız görselleştirmek olanak tanıyan yığınının kullanarak bu günlükler görselleştirmek için bir çözüm Kibana panosunda oturum sağlar.

## <a name="scenario"></a>Senaryo

Bu makalede, biz esnek yığını kullanarak ağ güvenlik grubu akışı günlüklerini görselleştirmek sağlayacak bir çözümü ayarlama.  Logstash giriş eklentisi akış günlükleri doğrudan akış günlükleri içeren için yapılandırılan depolama blobu elde eder. Ardından, esnek yığını kullanarak, akış günlükleri dizine ve olması bilgileri görselleştirmek için bir Kibana Pano oluşturmak için kullanılır.

![senaryo][scenario]

## <a name="steps"></a>Adımlar

### <a name="enable-network-security-group-flow-logging"></a>Ağ güvenlik grubu etkinleştirme akışı günlüğe kaydetme
Bu senaryo için ağ güvenlik grubu akış hesabınızdaki en az bir ağ güvenlik grubu etkin günlüğü olması gerekir. Ağ güvenlik akış günlükleri etkinleştirme ile ilgili yönergeler için aşağıdaki makaleye bakın [akış günlüğü ağ güvenlik grupları için giriş](network-watcher-nsg-flow-logging-overview.md).


### <a name="set-up-the-elastic-stack"></a>Esnek yığını ayarlayın
NSG akış günlükleri esnek yığın ile bağlayarak, arama, grafik, analiz etmek ve bizim günlüklerinden Öngörüler türetilen kurmamızı sağlayan bir Kibana Pano oluşturabilir.

#### <a name="install-elasticsearch"></a>Install Elasticsearch

1. Sürüm 5.0 ve yukarıdaki esnek yığın, Java 8 gerektirir. Komutu çalıştırın `java -version` sürümünüzü denetlemek için. Yükleme, üzerinde belgelerine başvurun java yoksa [Oracle'nın Web sitesi](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
1. Sisteminiz için doğru ikili paketini indirin:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Diğer yükleme yöntemleri bulunabilir [Elasticsearch yükleme](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Elasticsearch komutu ile çalıştığından emin olun:

    ```bash
    curl http://127.0.0.1:9200
    ```

    Aşağıdakine benzer bir yanıt görmeniz gerekir:

    ```json
    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",
    "version" : {
        "number" : "5.2.0",
        "build_hash" : "8ff36d139e16f8720f2947ef62c8167a888992fe",
        "build_timestamp" : "2016-01-27T13:32:39Z",
        "build_snapshot" : false,
        "lucene_version" : "6.1.0"
    },
    "tagline" : "You Know, for Search"
    }
    ```

Yükleme esnek arama hakkında daha ayrıntılı yönergeler için sayfaya bakın [yükleme](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Logstash yükleyin

1. Aşağıdaki komutları çalıştırın Logstash yüklemek için:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Sonraki biz Logstash erişmek ve akış günlükleri ayrıştırmak için yapılandırmanız gerekir. Kullanarak bir logstash.conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Aşağıdaki içeriği dosyaya ekleyin:

  ```
input {
   azureblob
     {
         storage_account_name => "mystorageaccount"
         storage_access_key => "VGhpcyBpcyBhIGZha2Uga2V5Lg=="
         container => "insights-logs-networksecuritygroupflowevent"
         codec => "json"
         # Refer https://docs.microsoft.com/azure/network-watcher/network-watcher-read-nsg-flow-logs
         # Typical numbers could be 21/9 or 12/2 depends on the nsg log file types
         file_head_bytes => 12
         file_tail_bytes => 2
         # Enable / tweak these settings when event is too big for codec to handle.
         # break_json_down_policy => "with_head_tail"
         # break_json_batch_count => 2
     }
   }

   filter {
     split { field => "[records]" }
     split { field => "[records][properties][flows]"}
     split { field => "[records][properties][flows][flows]"}
     split { field => "[records][properties][flows][flows][flowTuples]"}

  mutate{
   split => { "[records][resourceId]" => "/"}
   add_field => {"Subscription" => "%{[records][resourceId][2]}"
                 "ResourceGroup" => "%{[records][resourceId][4]}"
                 "NetworkSecurityGroup" => "%{[records][resourceId][8]}"}
   convert => {"Subscription" => "string"}
   convert => {"ResourceGroup" => "string"}
   convert => {"NetworkSecurityGroup" => "string"}
   split => { "[records][properties][flows][flows][flowTuples]" => ","}
   add_field => {
               "unixtimestamp" => "%{[records][properties][flows][flows][flowTuples][0]}"
               "srcIp" => "%{[records][properties][flows][flows][flowTuples][1]}"
               "destIp" => "%{[records][properties][flows][flows][flowTuples][2]}"
               "srcPort" => "%{[records][properties][flows][flows][flowTuples][3]}"
               "destPort" => "%{[records][properties][flows][flows][flowTuples][4]}"
               "protocol" => "%{[records][properties][flows][flows][flowTuples][5]}"
               "trafficflow" => "%{[records][properties][flows][flows][flowTuples][6]}"
               "traffic" => "%{[records][properties][flows][flows][flowTuples][7]}"
                }
   convert => {"unixtimestamp" => "integer"}
   convert => {"srcPort" => "integer"}
   convert => {"destPort" => "integer"}        
  }

  date{
    match => ["unixtimestamp" , "UNIX"]
  }
 }
output {
  stdout { codec => rubydebug }
  elasticsearch {
    hosts => "localhost"
    index => "nsg-flow-logs"
  }
}  
  ```

Logstash yükleme hakkında daha ayrıntılı yönergeler için bkz [resmi belge](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Azure blob depolama için Logstash giriş eklentisini yükleme

Bu Logstash eklentisi doğrudan kendi atanmış depolama hesabından akış günlüklerine erişmesine olanak sağlar. Bu eklentiyi yüklemek üzere varsayılan Logstash yükleme dizininden (Bu örnek /usr/share/logstash/bin) komutu çalıştırın:

```bash
logstash-plugin install logstash-input-azureblob
```

Logstash başlatmak için komutu çalıştırın:

```bash
sudo /etc/init.d/logstash start
```

Bu eklenti hakkında daha fazla bilgi için belgelerine başvurun [burada](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)

### <a name="install-kibana"></a>Kibana yükleyin

1. Kibana yüklemek için aşağıdaki komutları çalıştırın:

  ```bash
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. Kibana çalıştırmak için komutları kullanın:

  ```bash
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. Kibana web arabirimi görüntülemek için gidin `http://localhost:5601`
1. Bu senaryo için akış günlükleri için kullanılan dizin Düzen "nsg akış günlüklerini" ' dir. "Çıktı" bölümünde logstash.conf dosyanızın dizin düzeni değişebilir.

1. Kibana Pano uzaktan görüntülemek istiyorsanız, erişim izni veren bir gelen NSG kuralı oluşturma **bağlantı noktası 5601**.

### <a name="create-a-kibana-dashboard"></a>Kibana bir pano oluşturun

Bu makalede, eğilimleri ve Ayrıntılar, uyarıları görüntülemek örnek Pano sağladık.

![Şekil 1][1]

1. Pano dosya indirme [burada](https://aka.ms/networkwatchernsgflowlogdashboard), görselleştirme dosya [burada](https://aka.ms/networkwatchernsgflowlogvisualizations)ve kaydedilen arama dosyasını [burada](https://aka.ms/networkwatchernsgflowlogsearch).

1. Altında **Yönetim** sekmesini Kibana gidin **kaydedilmiş nesneleri** ve tüm üç dosyalarını içeri aktarın. Öğesinden sonra **Pano** sekmesini açın ve örnek Pano yükleyin.

Ayrıca, kendi Görselleştirme ve kendi ilgi ölçümleri doğru özel panolar oluşturabilirsiniz. Kibana 's Kibana görselleştirmeleri oluşturma hakkında daha fazla okuma [resmi belge](https://www.elastic.co/guide/en/kibana/current/visualize.html).

### <a name="visualize-nsg-flow-logs"></a>NSG akış günlükleri Görselleştirme

Örnek Pano akışı günlüklerinin birkaç görselleştirmeleri sağlar:

1. -Zamanlı karar/yön ile süre boyunca akışlarının sayısı gösteren zaman serisi grafikleri akar. Zaman birimi ve her iki bu görselleştirmeleri aralığını düzenleyebilirsiniz. Akışlar karar tarafından izin ver veya Reddet akış yönü tarafından gelen ve giden trafik oranını gösterirken, kararlara oranını gösterir. Bu görsellerle trafiği eğilimleri zamanla inceleyin ve tüm ani veya olağan dışı desenleri arayın.

  ![Şekil 2][2]

1. Hedef/kaynak bağlantı noktası tarafından – ilgili bağlantı noktaları akışlarına dökümünü gösteren pasta grafikler akar. Bu görünüm ile en sık kullanılan bağlantı noktaları görebilirsiniz. Pasta grafik içinde belirli bir bağlantı noktasında tıklarsanız, Pano rest Bu bağlantı noktasının doğru akar filtreleyin.

  ![figure3][3]

1. Akışlar ve erken günlüğü süresi – kaydedilen akışlarının sayısı ve yakalanan erken günlük tarihini gösteren ölçümleri sayısı.

  ![Şekil 4][4]

1. Akışlar NSG ve kural – her NSG içinde kurallar dağıtımını yanı sıra, her NSG içinde akışları dağıtımını gösteren bir çubuk grafik. Buradan, hangi NSG ve kuralları en trafiği oluşturulan görebilirsiniz.

  ![figure5][5]

1. 10 kaynak/hedef IP – çubuk ilk 10 kaynak ve hedef IP'leri gösteren grafikleri top. Daha az veya üst IP'leri göstermek için bu grafikler ayarlayabilirsiniz. Buradan trafiği karar yanı sıra IP'leri en sık oluşan görebilirsiniz (izin verme veya reddetme) her IP yapılan.

  ![figure6][6]

1. Akış Başlıkları – Bu tablo her akış tanımlama grubu yanı sıra, karşılık gelen NGS ve kural içinde yer alan bilgileri gösterir.

  ![figure7][7]

Sorgu çubuğu Pano üstündeki kullanarak, abonelik kimliği, kaynak grupları, kural veya herhangi bir değişken ilgi gibi akışlarının herhangi bir parametre göre Pano aşağı filtre uygulayabilirsiniz. Kibana'nın sorgular ve filtreleri hakkında daha fazla bilgi için başvurmak [resmi belge](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

## <a name="conclusion"></a>Sonuç

Ağ güvenlik grubu akış günlükleri esnek yığınını ile birleştirerek, biz bizim ağ trafiğini görselleştirmek için güçlü ve özelleştirilebilir işlemleriyle gündeme. Bu panoları, hızlı bir şekilde elde, ağ trafiğini yanı sıra filtre hakkında Öngörüler aşağı paylaşabilir ve tüm olası anormallikleri üzerinde araştırmanıza olanak tanır. Kibana kullanarak, bu panoları uyarlamanıza ve güvenlik, denetleme ve uyumluluk gereksinimlerini karşılamak için belirli görselleştirmeler oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek NSG akış günlüklerinizi Power BI ile görselleştirme öğrenin [görselleştirmek NSG akar Power BI ile günlükleri](network-watcher-visualize-nsg-flow-logs-power-bi.md)


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
