---
title: Ağ İzleyicisi ve Elastic Stack kullanarak ağ güvenlik grubu akış günlüklerini analiz etme ve yönetme | Microsoft Docs
description: Yönetme ve Ağ İzleyicisi ve Elastic Stack kullanarak azure'da ağ güvenlik grubu akış günlüklerini analiz edin.
services: network-watcher
documentationcenter: na
author: mattreatMSFT
manager: vitinnan
editor: ''
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: mareat
ms.openlocfilehash: 7361eff0f76271564fd5a0e9b8a18221ec4138e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860149"
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Açık kaynak araçlar kullanarak Azure Ağ İzleyicisi NSG akış günlüklerini Görselleştirme

Ağ güvenlik grubu akış günlüklerini kullanılabilir bilgi sağlayan ağ güvenlik gruplarında giriş ve çıkış IP trafiğini anlayın. Bu akış günlüklerini giden ve gelen akışlar başına kural olarak, akış uygulandığı NIC, 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) akışla ilgili trafiklere izin verildiğini veya, gösterin.

Bu akış günlüklerini el ile ayrıştırma ve Öngörüler elde etmek zor olabilir. Ancak, bu verileri görselleştirmenize yardımcı olabilecek birçok açık kaynak araçları vardır. Bu makale elastik hızla dizin ve akışınız görselleştirmenize olanak tanıyan yığını kullanarak bu günlükleri görselleştirmek için bir çözüm Kibana panosunda oturum sağlar.

> [!Warning]  
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için bkz [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md). Aşağıdaki yönergeler, değişiklik yapmadan günlük dosyalarının sürüm 2 ile çalışmaz.

## <a name="scenario"></a>Senaryo

Bu makalede, elastik yığın'ı kullanarak ağ güvenlik grubu akış günlüklerini görselleştirme sağlayacak bir çözüm ayarlayacağız.  Logstash giriş eklentisi akış günlüklerini içeren akış günlüklerini yapılandırılmış depolama blobu doğrudan elde edersiniz. Ardından, Elastic Stack kullanarak akış günlüklerini dizine ve bilgileri görselleştirmek için Kibana Pano oluşturmak için kullanılan.

![senaryo][scenario]

## <a name="steps"></a>Adımlar

### <a name="enable-network-security-group-flow-logging"></a>Etkinleştirme ağ güvenlik grubu akış günlüğe kaydetme
Bu senaryo için ağ güvenlik grubu akış hesabınızda en az bir ağ güvenlik grubu etkin günlüğü olması gerekir. Ağ güvenlik akış günlüklerini etkinleştirme hakkında yönergeler için şu makaleye başvurun [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md).

### <a name="set-up-the-elastic-stack"></a>Elastik yığını ayarlayın
NSG akış günlüklerini Elastic Stack ile bağlanarak, Kibana panonuza arama, grafik, analiz ve bizim günlüklerinden içgörülere sahip olun kurmamızı sağlayan oluşturabiliriz.

#### <a name="install-elasticsearch"></a>Elasticsearch'ü yükleme

1. Elastik yığın sürüm 5.0 ve üzeri Java 8 gerektirir. Komutunu çalıştırın `java -version` sürümünüzü denetlemek için. Java yüklü değilse, şirket belgelerine başvurun [Azure bloklarını koruma jdk](https://aka.ms/azure-jdks).
2. Sisteminiz için doğru ikili paket yükleyin:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
   sudo dpkg -i elasticsearch-5.2.0.deb
   sudo /etc/init.d/elasticsearch start
   ```

   Diğer yükleme yöntemleri şu yolda bulunabilir: [elasticsearch'ü yükleme](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

3. Elasticsearch komutuyla çalıştığını doğrulayın:

    ```bash
    curl http://127.0.0.1:9200
    ```

    Şuna benzer bir yanıt görmeniz gerekir:

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

Yükleme esnek arama ile ilgili diğer yönergeleri için başvurmak [yükleme yönergeleri](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html).

### <a name="install-logstash"></a>Logstash'i yükleyin

1. Aşağıdaki komutları çalıştırın Logstash yüklemek için:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
2. Sonraki erişme ve akış günlüklerini ayrıştırmanıza Logstash gerekir. Kullanarak bir logstash.conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Aşağıdaki içeriği dosyaya ekleyin:

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

Logstash yükleme hakkında daha fazla yönerge için başvurmak [resmi belgelerine](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html).

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Azure blob depolama için Logstash giriş eklentisini yükleme

Bu Logstash eklentisi, kendi atanmış depolama hesabından akış günlüklerini doğrudan bağlanmasına izin verir. Bu eklentiyi yüklemek için komut varsayılan Logstash yükleme dizini (büyük/küçük harf bu /usr/share/logstash/bin) çalıştırın:

```bash
logstash-plugin install logstash-input-azureblob
```

Logstash başlangıç için komutu çalıştırın:

```bash
sudo /etc/init.d/logstash start
```

Bu eklenti hakkında daha fazla bilgi için [belgeleri](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="install-kibana"></a>Kibana'yı yükleme

1. Kibana'yı yüklemek için aşağıdaki komutları çalıştırın:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
   tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
   ```

2. Kibana'yı çalıştırma için komutları kullanın:

   ```bash
   cd kibana-5.2.0-linux-x86_64/
   ./bin/kibana
   ```

3. Kibana'yı web Arabiriminizin görüntülemek için gidin `http://localhost:5601`
4. Bu senaryoda, akış günlükleri için kullanılan dizin "nsg akış günlüklerini" modelidir. Dizin düzeni logstash.conf dosyanızın "çıkış" bölümündeki değişebilir.
5. Kibana panonuza uzaktan görüntülemek istiyorsanız, erişim izni veren gelen NSG kuralı oluşturma **bağlantı noktası 5601**.

### <a name="create-a-kibana-dashboard"></a>Kibana Pano oluşturma

Örnek Pano uyarılarınızı eğilimleri ve ayrıntılarını görüntülemek için aşağıdaki resimde gösterilmektedir:

![Şekil 1][1]

İndirme [Pano dosya](https://aka.ms/networkwatchernsgflowlogdashboard), [görselleştirme dosyası](https://aka.ms/networkwatchernsgflowlogvisualizations)ve [arama kaydettiğiniz](https://aka.ms/networkwatchernsgflowlogsearch).

Altında **Yönetim** sekmesine gidin Kibana, **kaydedilen nesneleri** ve üç dosya içeri aktarma. Öğesinden sonra **Pano** sekmesini açın ve örnek panoyu yükleyebilirsiniz.

Ayrıca, kendi görselleştirmeler ve kendi ilgilendiğiniz ölçümleri uyarlanmış panolar oluşturabilirsiniz. Kibana 's Kibana görselleştirmeler oluşturma hakkında daha fazla [resmi belgelerine](https://www.elastic.co/guide/en/kibana/current/visualize.html).

### <a name="visualize-nsg-flow-logs"></a>NSG akış günlüklerini Görselleştirme

Akış günlüklerini çeşitli görselleştirmelerle ilişkin örnek panoyu sağlar:

1. -Zamana karar/yön ile zaman serisi grafikleri süre boyunca akışlarının sayısını gösteren akar. Zaman birimi ve her iki bu görselleştirmeler span düzenleyebilirsiniz. Karar göre akışlar izin ver veya Reddet yönüne göre akışlar geliş ve gidiş trafiğinin oranını gösterirken, kararlara oranını gösterir. Bu görsellerle zaman içinde trafik eğilimleri inceleyebilir ve herhangi bir ani veya olağandışı desenleri için bakın.

   ![Şekil 2][2]

2. Hedef/kaynak bağlantı noktası göre– akışlar ilgili bağlantı noktaları için akışlar dökümünü gösteren pasta grafikler. Bu görünümle, en sık kullanılan bağlantı noktaları görebilirsiniz. Pasta grafik içinde belirli bir bağlantı noktasında tıklarsanız Pano rest Bu bağlantı noktasının doğru akar filtre uygular.

   ![figure3][3]

3. Akışlar ve erken günlüğü süresi – kaydedilen akışlarının sayısı ve yakalanan erken günlük tarihini gösteren ölçümleri sayısı.

   ![Şekil 4][4]

4. Akışlar NSG ve kural – dağıtım her NSG kuralların yanı sıra, her bir NSG içinde akış dağılımı gösteren bir çubuk grafiği. Burada, hangi NSG ve kuralları en çok trafiği oluşturulan görebilirsiniz.

   ![figure5][5]

5. İlk 10 kaynak ve hedef Ip'lerini gösteren çubuk grafikler 10 kaynak/hedef IP'ler – üst. Daha az veya üst IP'ler göstermek için bu grafikleri ayarlayabilirsiniz. Buradan IP'ler ve bunun yanı sıra trafik karar en sık gerçekleşen görebilirsiniz (izin ver veya Reddet) her IP yapılan.

   ![figure6][6]

6. Akış diziler – Bu tablo her akış demet yanı sıra, karşılık gelen NGS ve kural içinde yer alan bilgileri gösterir.

   ![figure7][7]

Panonun üst kısmında sorgu çubuğunu kullanarak, abonelik kimliği, kaynak grupları, kuralı veya ilgilendiğiniz herhangi bir değişken gibi bir akışı herhangi bir parametre bağlı Pano aşağı filtreleyebilirsiniz. Kibana'nın sorgular ve filtreleri hakkında daha fazla bilgi için bkz [resmi belgeleri](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

## <a name="conclusion"></a>Sonuç

Ağ güvenlik grubu akış günlüklerini Elastic Stack ile birleştirerek, biz bizim ağ trafiğini görselleştirmek için güçlü ve özelleştirilebilir yol gündeme. Bu pano, hızlı bir şekilde elde edin ve aşağı, ağ trafiğini, aynı zamanda filtre ilgili öngörüleri paylaşın ve olası anomalileri araştırın olanak tanır. Kibana'yı kullanarak, bu panoları uyumlu hale getirmenizi ve tüm güvenlik, Denetim ve uyumluluk gereksinimlerini karşılamak için özel görselleştirmeler oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek, NSG akış günlüklerini Power BI ile görselleştirmeyi öğrenmek [görselleştirme NSG akış günlükleri Power BI ile](network-watcher-visualize-nsg-flow-logs-power-bi.md)

<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
