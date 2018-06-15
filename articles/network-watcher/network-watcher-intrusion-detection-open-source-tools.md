---
title: Azure Ağ İzleyicisi'ni ve açık kaynaklı araçları ile ağ izinsiz giriş algılama gerçekleştirme | Microsoft Docs
description: Bu makalede Azure Ağ İzleyicisi'ni kullanın ve ağ izinsiz giriş algılama gerçekleştirmek için kaynak araçlarını açmak açıklar
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: aff1b5f9e8860d3b8dc09b37684bb8a4ac2bf134
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "23864227"
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Ağ İzleyicisi'ni ve açık kaynaklı araçları ile ağ izinsiz giriş algılama gerçekleştirme

Paket yakalama ağ izinsiz giriş algılama sistemleri (Kimlikler) uygulama ve ağ güvenlik izleme (NSM) gerçekleştirmek için önemli bir bileşeni var. Paket yakalama işleyen ve olası ağ yetkisiz erişim ve kötü amaçlı etkinliği imzaları Ara birkaç açık kaynak Kimlikleri araç vardır. Ağ İzleyicisi tarafından ağınız zararlı yetkisiz veya güvenlik açıkları için çözümleyebilirsiniz. sağlanan paket kullanarak yakalar.

Bu tür bir açık kaynak aracı Suricata, ağ trafiğini izlemek için rulesets kullanır ve şüpheli olaylar gerçekleştiğinde uyarıları tetikler Kimlikleri altyapının değil. Suricata yüksek hızda ve verimlilik ağ trafiğini analiz gerçekleştirebilir anlamına çok iş parçacıklı bir altyapı sunar. Suricata ve özelliklerini hakkında daha fazla ayrıntı için kendi Web sitesini ziyaret https://suricata-ids.org/.

## <a name="scenario"></a>Senaryo

Bu makalede, Ağ İzleyicisi, Suricata ve esnek yığını kullanarak ağ izinsiz giriş algılama gerçekleştirmek için ortamınızı ayarlayın açıklanmaktadır. Ağ İzleyicisi ağ izinsiz giriş algılama gerçekleştirmek için kullanılan paket yakalamaları ile sağlar. Suricata paket yakalar ve tehditleri verilen kendi ruleset eşleşen paketlere dayalı tetikleyici uyarıları işler. Bu uyarılar yerel makinenizde bir günlük dosyasında depolanır. Esnek yığını kullanarak Suricata tarafından oluşturulan günlükleri dizine ve değiştirebilirsiniz görsel bir günlükleri ve hızlı bir şekilde olası ağ güvenlik açıkları için erişim sahibi için bir yol sağlayan bir Kibana Pano oluşturmak için kullanılır.  

![Basit web uygulaması senaryosu][1]

Her iki açık kaynaklı araçları bir Azure VM kendi Azure ağ ortamında bu analiz gerçekleştirmenize olanak sağlayan ayarlanabilir.

## <a name="steps"></a>Adımlar

### <a name="install-suricata"></a>Suricata yükleyin

Tüm diğer yöntemleri yüklemesinin için ziyaret edin http://suricata.readthedocs.io/en/latest/install.html

1. Vm'nizin komut satırı terminale aşağıdaki komutları çalıştırın:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. Kurulumunuzu doğrulamak için komutu çalıştırmak `suricata -h` komutların tam listesini görmek için.

### <a name="download-the-emerging-threats-ruleset"></a>Gelişmekte olan tehditleri ruleset indirin

Bu aşamada, biz çalışmaya Suricata için herhangi bir kuralın sahip değilsiniz. Geliştirilmiş kullanımı kural kümeleri sağlayıcıları, Gelişmekte olan tehditleri veya Snort VRT kurallardan gibi bir dizi de olabilir veya algılamak istediğiniz belirli tehditlerin ağınız varsa, kendi kurallarınızı oluşturabilirsiniz. Ücretsiz olarak erişilebilir Gelişmekte olan tehditleri ruleset burada kullanırız:

Kural kümesi indirin ve dizine kopyalayın:

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>İşlem paket ile Suricata yakalar.

Paket işlemek için şu komutu çalıştırın Suricata kullanarak yakalar:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
Sonuçta elde edilen uyarıları denetlemek için fast.log dosya okuyun:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-the-elastic-stack"></a>Esnek yığını ayarlayın

Bizim ağda olup bitenler hakkında değerli bilgiler Suricata üreten günlükleri bulunmasına karşın, bu günlük dosyaları en kolay okuduğunuzdan ve anladığınızdan değil. Esnek yığın ile Suricata bağlanarak arama, grafik, analiz etmek ve bizim günlüklerinden Öngörüler türetilen kurmamızı sağlayan bir Kibana Pano oluşturabilir.

#### <a name="install-elasticsearch"></a>Install Elasticsearch

1. Sürüm 5.0 ve yukarıdaki esnek yığın, Java 8 gerektirir. Komutu çalıştırın `java -version` sürümünüzü denetlemek için. Yükleme, üzerinde belgelerine başvurun java yoksa [Oracle'nın Web sitesi](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
1. Sisteminiz için doğru ikili paketini indirin:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Diğer yükleme yöntemleri bulunabilir [Elasticsearch yükleme](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Elasticsearch komutu ile çalıştığından emin olun:

    ```
    curl http://127.0.0.1:9200
    ```

    Aşağıdakine benzer bir yanıt görmeniz gerekir:

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

Yükleme esnek arama hakkında daha ayrıntılı yönergeler için sayfaya bakın [yükleme](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Logstash yükleyin

1. Aşağıdaki komutları çalıştırın Logstash yüklemek için:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Sonraki biz Logstash eve.json dosyası çıktısından okumak için yapılandırmanız gerekir. Kullanarak bir logstash.conf dosyası oluşturun:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Aşağıdaki içeriği dosyaya ekleyin (eve.json dosya yolunun doğru olduğundan emin olun):

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

1. Böylece Logstash dosya işleyebilen eve.json dosyanın doğru izinleri verdiğinizden emin olun.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. Logstash başlatmak için komutu çalıştırın:

    ```
    sudo /etc/init.d/logstash start
    ```

Logstash yükleme hakkında daha ayrıntılı yönergeler için bkz [resmi belge](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-kibana"></a>Kibana yükleyin

1. Kibana yüklemek için aşağıdaki komutları çalıştırın:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. Kibana çalıştırmak için komutları kullanın:

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. Kibana web arabirimi görüntülemek için gidin `http://localhost:5601`
1. Bu senaryo için Suricata günlükleri için kullanılan dizin düzeni olan "logstash-*"

1. Kibana Pano uzaktan görüntülemek istiyorsanız, erişim izni veren bir gelen NSG kuralı oluşturma **bağlantı noktası 5601**.

### <a name="create-a-kibana-dashboard"></a>Kibana bir pano oluşturun

Bu makalede, eğilimleri ve Ayrıntılar, uyarıları görüntülemek örnek Pano sağladık.

1. Pano dosya indirme [burada](https://aka.ms/networkwatchersuricatadashboard), görselleştirme dosya [burada](https://aka.ms/networkwatchersuricatavisualization)ve kaydedilen arama dosyasını [burada](https://aka.ms/networkwatchersuricatasavedsearch).

1. Altında **Yönetim** sekmesini Kibana gidin **kaydedilmiş nesneleri** ve tüm üç dosyalarını içeri aktarın. Öğesinden sonra **Pano** sekmesini açın ve örnek Pano yükleyin.

Ayrıca, kendi Görselleştirme ve kendi ilgi ölçümleri doğru özel panolar oluşturabilirsiniz. Kibana 's Kibana görselleştirmeleri oluşturma hakkında daha fazla okuma [resmi belge](https://www.elastic.co/guide/en/kibana/current/visualize.html).

![kibana Panosu][2]

### <a name="visualize-ids-alert-logs"></a>Kimlikleri uyarı günlükleri Görselleştirme

Örnek Pano Suricata uyarı günlüklerinin birkaç görselleştirmeleri sağlar:

1. Uyarıları GeoIP – coğrafi konum (IP tarafından belirlenir) göre kaynak kendi ülkeye göre uyarıları dağıtımını gösteren bir harita göre

    ![geo ip][3]

1. 10 uyarılar – 10 en sık tetiklenen uyarıları ve bunların açıklaması özetini top. Pano belirli Bu uyarıyla ilgili bilgi için aşağıya bir bireysel uyarı tıklatarak filtreler.

    ![Görüntü 4][4]

1. Uyarılar – sayısı ruleset tarafından tetiklenen uyarıların toplam sayısı

    ![Görüntü 5][5]

1. İlk 20 kaynak/hedef IP/bağlantı noktaları - üst 20 IP ve bağlantı noktalarını bu uyarıları gösteren pasta grafikler üzerinde tetiklenen. Filtre uygulayabilirsiniz kaç tane ve hangi tür uyarılar görmek için belirli IP/bağlantı noktalarında aşağı tetiklendiğinde.

    ![Görüntü 6][6]

1. Uyarı Özeti – her bireysel uyarı belirli ayrıntılarını özetleyen bir tablo. Her uyarı için ilgi diğer parametrelerini göstermek için bu tabloyu özelleştirebilirsiniz.

    ![Görüntü 7][7]

Özel görsel ve panolar oluşturma hakkında daha fazla belgeler için bkz: [Kibana'nin resmi belge](https://www.elastic.co/guide/en/kibana/current/introduction.html).

## <a name="conclusion"></a>Sonuç

Ağ İzleyicisi'ni ve açık kaynak Kimlikleri araçları Suricata gibi ağ izinsiz giriş algılama çeşitli tehditler için gerçekleştirebileceğiniz sağlanan paket birleştirerek yakalar. Bu panoları, hızlı bir şekilde veri uyarıları gibi kötü amaçlı kullanıcı aracıları veya açık bağlantı noktalarını kök nedeni bulmak için iyi dıg olarak eğilimleri ve ağınızdaki, anormallikleri nokta olanak tanır. Ayıklanan bu verilerle tepki ve ağınızdaki tüm zararlı yetkisiz erişim saldırılara karşı koruma ve ağınıza gelecekteki yetkisiz erişimi engellemeye kuralları oluşturma konusunda bilinçli kararlar yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Paket yakalama ziyaret ederek uyarılar temelinde tetiklemek öğrenin [paket yakalama Azure işlevleriyle öngörülü ağ izleme yapmak için kullanın](network-watcher-alert-triggered-packet-capture.md)

Ziyaret ederek NSG akış günlüklerinizi Power BI ile görselleştirme öğrenin [görselleştirmek NSG akar Power BI ile günlükleri](network-watcher-visualize-nsg-flow-logs-power-bi.md)



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
