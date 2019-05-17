---
title: Azure Ağ İzleyicisi ve açık kaynak araçlarla ağ izinsiz giriş algılaması gerçekleştirme | Microsoft Docs
description: Bu makale Azure Ağ İzleyicisi ve açık kaynak Araçlar, ağ izinsiz giriş algılaması gerçekleştirmek için nasıl kullanılacağını açıklar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: b5dc885611c6654c1dc1d236e41ed75ef2717cc1
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65605709"
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Ağ İzleyicisi ve açık kaynak araçlarla ağ izinsiz giriş algılaması gerçekleştirme

Paket yakalama ağ yetkisiz giriş algılama sistemleri (Kimlikler) uygulama ve ağ güvenlik izleme (NSM) gerçekleştirmek için önemli bir bileşenidir. Paket yakalamaları işleyen ve olası ağ izinsiz girişi ve kötü amaçlı etkinlik imzaları Ara birçok açık kaynak Kimlikleri araç vardır. Ağ İzleyicisi tarafından ağınızda herhangi bir zararlı izinsiz girişi veya güvenlik açıklarını çözümleyebilirsiniz. sağlanan paket kullanılarak yakalar.

Suricata, ağ trafiğini izlemek için rulesets kullanan ve şüpheli olaylar meydana geldiğinde uyarılar tetikler Kimlikleri altyapısı bu açık kaynak araçlardan biridir. Suricata ağ trafiği analizinde daha yüksek hız ve verimliliği ile gerçekleştirebileceğiniz anlamına gelen çok iş parçacıklı bir altyapı sunar. Kendi Web sitesinde Suricata ve özellikleri hakkında ayrıntılı bilgi için ziyaret https://suricata-ids.org/.

## <a name="scenario"></a>Senaryo

Bu makalede, Ağ İzleyicisi, Suricata ve elastik yığın'ı kullanarak ağ izinsiz giriş algılaması gerçekleştirmek için ortamınızı ayarlama açıklanmaktadır. Ağ İzleyicisi paket yakalama işlemlerini ağ izinsiz giriş algılaması gerçekleştirmek için kullanılan sağlar. Suricata, belirli bir ruleset tehdit eşleşen paketlere dayalı uyarıları tetikleyin ve paket yakalama işlemlerini işler. Bu uyarılar bir günlük dosyası yerel makinenizde depolanır. Elastik yığın kullanma, Suricata tarafından oluşturulan günlükler ve değiştirilebilen ile günlükleri ve hızlı bir şekilde olası ağ güvenlik açıkları hakkında bilgi için bir yol görsel bir temsilini sağlayarak Kibana panoyu oluşturmak için kullanılır.  

![Basit web uygulaması senaryosu][1]

Her iki açık kaynak araçları bir Azure sanal makinesinde kendi Azure ağ ortamında bu analiz gerçekleştirmenize imkan tanıyan ayarlanabilir.

## <a name="steps"></a>Adımlar

### <a name="install-suricata"></a>Suricata yükleyin

Tüm diğer yöntemleri yüklemesi için ziyaret edin https://suricata.readthedocs.io/en/latest/install.html

1. Sanal makinenizin komut satırı terminalde aşağıdaki komutları çalıştırın:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. Yüklemenizi doğrulamak için komutu çalıştırın. `suricata -h` komutların tam listesini görmek için.

### <a name="download-the-emerging-threats-ruleset"></a>Gelişmekte olan tehditleri ruleset indirin

Bu aşamada, biz çalışmaya Suricata için herhangi bir kuralın yoktur. Sağlayıcılar, Gelişmekte olan tehditleri veya Snort VRT kuralları gibi çok sayıda kullanım geliştirilen kural kümeleri ayrıca veya belirli tehditleri algılamak istediğiniz ağınıza varsa kendi kuralları oluşturabilirsiniz. Özgürce erişilebilir Gelişmekte olan tehditleri ruleset burada kullanırız:

Kural kümesi indirin ve bunları dizine kopyalayın:

```
wget https://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>İşlem paketi ile Suricata yakalar.

Paket işlemek için aşağıdaki komutu çalıştırın, Suricata kullanarak yakalar:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
Elde edilen uyarılar denetlemek için fast.log dosya okuyun:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-the-elastic-stack"></a>Elastik yığını ayarlayın

Bizim ağda olup bitenler hakkında değerli bilgiler Suricata üreten günlükleri içerirken, bu günlük dosyaları okumanız ve anlamanız kolay değildir. Elastik yığın ile Suricata bağlanarak Kibana panonuza arama, grafik, analiz ve bizim günlüklerinden içgörülere sahip olun kurmamızı sağlayan oluşturabiliriz.

#### <a name="install-elasticsearch"></a>Elasticsearch'ü yükleme

1. Elastik yığın sürüm 5.0 ve üzeri Java 8 gerektirir. Komutunu çalıştırın `java -version` sürümünüzü denetlemek için. Java yüklü değilse, şirket belgelerine başvurun [Azure bloklarını koruma jdk](https://aka.ms/azure-jdks).

1. Sisteminiz için doğru ikili paket yükleyin:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Diğer yükleme yöntemleri şu yolda bulunabilir: [elasticsearch'ü yükleme](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Elasticsearch komutuyla çalıştığını doğrulayın:

    ```
    curl http://127.0.0.1:9200
    ```

    Şuna benzer bir yanıt görmeniz gerekir:

    ```
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

Yükleme esnek arama hakkında ek yönergeler sayfasına bakın [yükleme](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Logstash'i yükleyin

1. Aşağıdaki komutları çalıştırın Logstash yüklemek için:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Sonraki eve.json dosya çıktısı okumak için Logstash'i yapılandırma gerekir. Kullanarak bir logstash.conf dosyası oluşturun:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Aşağıdaki içeriği dosyaya ekleyin (eve.json dosyasının yolu doğru olduğundan emin olun):

    ```ruby
    input {
    file {
        path => ["/var/log/suricata/eve.json"]
        codec =>  "json"
        type => "SuricataIDPS"
    }

    }

    filter {
    if [type] == "SuricataIDPS" {
        date {
        match => [ "timestamp", "ISO8601" ]
        }
        ruby {
        code => "
            if event.get('[event_type]') == 'fileinfo'
            event.set('[fileinfo][type]', event.get('[fileinfo][magic]').to_s.split(',')[0])
            end
        "
        }

        ruby{
        code => "
            if event.get('[event_type]') == 'alert'
            sp = event.get('[alert][signature]').to_s.split(' group ')
            if (sp.length == 2) and /\A\d+\z/.match(sp[1])
                event.set('[alert][signature]', sp[0])
            end
            end
            "
        }
    }

    if [src_ip]  {
        geoip {
        source => "src_ip"
        target => "geoip"
        #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
        add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
        add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
        }
        mutate {
        convert => [ "[geoip][coordinates]", "float" ]
        }
        if ![geoip.ip] {
        if [dest_ip]  {
            geoip {
            source => "dest_ip"
            target => "geoip"
            #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
            add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
            add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
            }
            mutate {
            convert => [ "[geoip][coordinates]", "float" ]
            }
        }
        }
    }
    }

    output {
    elasticsearch {
        hosts => "localhost"
    }
    }
    ```

1. Logstash dosya alabilen eve.json dosyanın doğru izinleri verdiğinizden emin olun.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. Logstash başlangıç için komutu çalıştırın:

    ```
    sudo /etc/init.d/logstash start
    ```

Logstash yükleme hakkında daha fazla yönerge için başvurmak [resmi belgeleri](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-kibana"></a>Kibana'yı yükleme

1. Kibana'yı yüklemek için aşağıdaki komutları çalıştırın:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. Kibana'yı çalıştırma için komutları kullanın:

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. Kibana'yı web Arabiriminizin görüntülemek için gidin `http://localhost:5601`
1. Bu senaryo için Suricata günlükleri için kullanılan dizini desendir "logstash-*"

1. Kibana panonuza uzaktan görüntülemek istiyorsanız, erişim izni veren gelen NSG kuralı oluşturma **bağlantı noktası 5601**.

### <a name="create-a-kibana-dashboard"></a>Kibana Pano oluşturma

Bu makalede, uyarılarınızı eğilimleri ve ayrıntılarını görüntülemek bir örnek Pano sağladık.

1. Pano dosya indirme [burada](https://aka.ms/networkwatchersuricatadashboard), görselleştirme dosyası [burada](https://aka.ms/networkwatchersuricatavisualization)ve kaydedilen arama dosyasını [burada](https://aka.ms/networkwatchersuricatasavedsearch).

1. Altında **Yönetim** sekmesine gidin Kibana, **kaydedilen nesneleri** ve üç dosya içeri aktarma. Öğesinden sonra **Pano** sekmesini açın ve örnek panoyu yükleyebilirsiniz.

Ayrıca, kendi görselleştirmeler ve kendi ilgilendiğiniz ölçümleri uyarlanmış panolar oluşturabilirsiniz. Kibana 's Kibana görselleştirmeler oluşturma hakkında daha fazla [resmi belgelerine](https://www.elastic.co/guide/en/kibana/current/visualize.html).

![kibana panonuza][2]

### <a name="visualize-ids-alert-logs"></a>Uyarı Kimlikleri günlüklerini Görselleştirme

Örnek Pano Suricata uyarı günlükler çeşitli görselleştirmeler sağlar:

1. GeoIP – kaynak coğrafi konum (IP tarafından belirlenir) göre ülke/bölgeye göre uyarıları dağılımını gösteren bir harita göre uyarıları

    ![coğrafi IP][3]

1. En iyi 10 uyarılar – 10 en sık tetiklenen uyarıları ve bunların açıklaması özeti. Bir bireysel uyarı'ı tıklatarak belirli bu uyarıya ilişkin bilgiler panoya aşağı filtreler.

    ![Görüntü 4][4]

1. Uyarılar – sayısı ruleset tarafından tetiklenen uyarıların toplam sayısı

    ![Görüntü 5][5]

1. Üst 20 kaynak/hedef IP/bağlantı noktaları - üst 20 IP ve bağlantı noktalarını bu uyarıları gösteren pasta grafikler üzerinde tetiklenen. Filtre uygulayabilirsiniz kaç tane ve hangi tür uyarılar görmek için belirli IP/bağlantı noktalarında aşağı tetiklenir.

    ![Görüntü 6][6]

1. Uyarı Özeti – her bir uyarının belirli ayrıntıları özetleyen bir tablo. Her uyarı için ilgilendiğiniz diğer parametreleri göstermek için bu tabloyu özelleştirebilirsiniz.

    ![Görüntü 7][7]

Özel görselleştirmeler ve Pano oluştururken veya daha fazla bilgi için bkz [Kibana'nın resmi belgelerine](https://www.elastic.co/guide/en/kibana/current/introduction.html).

## <a name="conclusion"></a>Sonuç

Ağ İzleyicisi ve Suricata gibi açık kaynak Kimlikleri araçları, çok çeşitli tehditleri için ağ izinsiz giriş algılama gerçekleştirebilirsiniz. sağlanan paket birleştirerek yakalar. Bu panolar, verilerin kötü amaçlı kullanıcı aracıları veya açık bağlantı noktaları gibi uyarı kök nedenlerini bulmak için iyi dıg olarak eğilimleri ve anormallikleri ağınızın içinde hızla nokta olanak tanır. Ayıklanan bu verilerle ağınıza gelecekteki yetkisiz erişimi önlemek için kurallar oluşturma tepki verin ve ağınızdaki tüm zararlı yetkisiz erişim saldırılara karşı koruma konusunda bilinçli kararlar yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Paket yakalamaları ederek uyarılara göre tetikleme hakkında bilgi edinin [Azure işlevleri ile öngörülü ağ izleme için paket yakalamayı kullanma](network-watcher-alert-triggered-packet-capture.md)

Ziyaret ederek, NSG akış günlüklerini Power BI ile görselleştirmeyi öğrenmek [görselleştirme NSG akış günlükleri Power BI ile](network-watcher-visualize-nsg-flow-logs-power-bi.md)



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
