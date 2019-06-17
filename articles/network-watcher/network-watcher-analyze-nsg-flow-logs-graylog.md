---
title: Azure ağ güvenlik grubu akış günlüklerini - Graylog çözümleme | Microsoft Docs
description: Yönetme ve Ağ İzleyicisi ile Graylog kullanarak azure'da ağ güvenliği grubu akış günlüklerini analiz etme hakkında bilgi edinin.
services: network-watcher
documentationcenter: na
author: mattreatMSFT
manager: vitinnan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: mareat
ms.openlocfilehash: a5fadcfce154740a79a8764f44f08b21ad18f4d8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60625280"
---
# <a name="manage-and-analyze-network-security-group-flow-logs-in-azure-using-network-watcher-and-graylog"></a>Ağ İzleyicisi ile Graylog kullanarak azure'da ağ güvenlik grubu akış günlüklerini analiz etme ve yönetme

[Ağ güvenlik grubu akış günlüklerini](network-watcher-nsg-flow-logging-overview.md) Azure ağ arabirimleri için giriş ve çıkış IP trafiğini anlamak için kullanabileceğiniz bilgileri sağlar. Akış günlüklerini göster giden ve gelen akışlar bir ağ güvenlik grubu kuralı temelinde hesaplamaz ağ arabirimini akışı, akış 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) uygular ve trafiğin izin verilen veya reddedilen .

Birçok ağ güvenlik grubu akış günlüğe kaydetme etkin ağınızdaki olabilir. Akış günlüğe kaydetme etkin birden fazla ağ güvenlik gruplarıyla ayrıştırılamadı ve günlüklerinizi verilerden Öngörüler elde etmeye hantal yapabilirsiniz. Bu makalede Graylog, bir açık kaynak günlük yönetimi ve analiz aracı ve Logstash, bir açık kaynak sunucu tarafı veri işleme ardışık düzeni'ni kullanarak bu ağ güvenlik grubu akış günlüklerini merkezi olarak yönetmek için bir çözüm sağlar.

> [!Warning]
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için bkz [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md). Aşağıdaki yönergeler, değişiklik yapmadan günlük dosyalarının sürüm 2 ile çalışmaz.

## <a name="scenario"></a>Senaryo

Ağ İzleyicisi'ni kullanarak ağ güvenlik grubu akış günlüklerini etkinleştirilir. Azure blob depolama alanına akış günlükleri akış içinde. Logstash eklentisi, bağlanma ve blob depolama alanından akış günlüklerini işlemek ve bunları Graylog için göndermek için kullanılır. Akış günlüklerini içinde Graylog depolandıktan sonra edilebilmeleri analiz ve özelleştirilmiş panolar görselleştirilir.

![Graylog iş akışı](./media/network-watcher-analyze-nsg-flow-logs-graylog/workflow.png)

## <a name="installation-steps"></a>Yükleme Adımları

### <a name="enable-network-security-group-flow-logging"></a>Ağ güvenlik grubu akış günlüğe kaydetme etkinleştir

Bu senaryo için en az bir ağ güvenlik grubu, hesabınızdaki etkin ağ güvenlik grubu akış günlüğe kaydetme olması gerekir. Güvenlik grubu akış günlüklerini etkinleştirme hakkında yönergeler ağ için aşağıdaki makaleye başvurun [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md).

### <a name="setting-up-graylog"></a>Graylog ayarlama

Bu örnekte, Graylog hem Logstash, Azure'da dağıtılan bir Ubuntu 14.04 sunucusunda yapılandırılır.

- Başvurmak [belgeleri](https://docs.graylog.org/en/2.2/pages/installation/os/ubuntu.html) Graylog adım adım yönergeler için yükleme Ubuntu.
- İzleyerek Graylog web arabirimini de yapılandırdığınızdan emin olun [belgeleri](https://docs.graylog.org/en/2.2/pages/configuration/web_interface.html#configuring-webif).

Bu örnek, minimum Graylog Kurulum (yani kullanır. tek bir örneği bir Graylog), ancak Graylog desteklemesi için sistem ve üretim bağlı olarak kaynakları arasında ihtiyaçları ölçeklendirmek için. Graylog'ın mimari konuları veya derin Mimari Kılavuzu hakkında daha fazla bilgi için bkz. [belgeleri](https://docs.graylog.org/en/2.2/pages/architecture.html) ve [Mimari Kılavuzu](https://www.slideshare.net/Graylog/graylog-engineering-design-your-architecture).

Graylog, platform ve tercihlerine bağlı olarak birçok yolla yüklenebilir. Graylog'ın resmi olası yükleme yöntemlerinin tam listesi için bkz [belgeleri](https://docs.graylog.org/en/2.2/pages/installation.html). Graylog sunucu uygulaması, Linux dağıtımlarında çalıştırır ve aşağıdaki önkoşullara sahiptir:

-  Java SE 8 veya üzeri – [Azul Azure JDK belgeleri](https://aka.ms/azure-jdks)
-  Esnek 2.x arama (2.1.0 veya üzeri)- [Elasticsearch yükleme belgeleri](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/_installation.html)
-  MongoDB 2.4 veya sonraki bir sürümü – [MongoDB yükleme belgeleri](https://docs.mongodb.com/manual/administration/install-on-linux/)

### <a name="install-logstash"></a>Logstash'i yükleyin

Logstash bir akış tanımlama grubu düzeyinde JSON biçimli akış günlüklerini düzleştirmek için kullanılır. Akış günlüklerini düzleştirme günlükleri düzenlemek ve Graylog içinde arama yapmak daha kolay getirir.

1. Logstash yüklemek için aşağıdaki komutları çalıştırın:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
   sudo dpkg -i logstash-5.2.0.deb
   ```

2. Akış günlüklerini ayrıştırmanıza ve bunları için Graylog göndermek için Logstash'i yapılandırma. Logstash.conf dosyası oluşturun:

   ```bash
   sudo touch /etc/logstash/conf.d/logstash.conf
   ```

3. Aşağıdaki içeriği dosyaya ekleyin. Depolama hesabınızın ayrıntıları yansıtmak üzere vurgulanan değerleri değiştirin:

   ```
    input {
        azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "NrUZmx7pJSKaRJzvQbeiZWi5nBRWOTr7Wwr9DrvK7YtDBrADYxT1y0oEExtSlkDnGRt7qcRiZzEBCCyRYND8SxSt"
            container => "insights-logs-networksecuritygroupflowevent"
            registry_create_policy => "start_over"
            codec => "json"
            file_head_bytes => 21
            file_tail_bytes => 9
            # Possible options: `do_not_break`, `with_head_tail`, `without_head_tail`
            break_json_down_policy  => 'with_head_tail'
            break_json_batch_count => 2
            interval => 5
        }
    }
    
    filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"
    }
    
     mutate {
        split => { "[records][resourceId]" => "/"}
        add_field =>{
                    "Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"
        }
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
        add_field => {
                    "time" => "%{[records][time]}"
                    "systemId" => "%{[records][systemId]}"
                    "category" => "%{[records][category]}"
                    "resourceId" => "%{[records][resourceId]}"
                    "operationName" => "%{[records][operationName}}"
                    "Version" => "%{[records][properties][Version}}"
                    "rule" => "%{[records][properties][flows][rule]}"
                    "mac" => "%{[records][properties][flows][flows][mac]}"
        }
        convert => {"unixtimestamp" => "integer"}
        convert => {"srcPort" => "integer"}
        convert => {"destPort" => "integer"}
        add_field => { "message" => "%{Message}" }
    }
        date {
            match => ["unixtimestamp" , "UNIX"]
        }
    }
    output {
        stdout { codec => rubydebug }
        udp {
            host => "127.0.0.1"
            port => 12201
        }
    }
    ```
   Sağlanan Logstash yapılandırma dosyası, üç bölümden oluşur: Giriş, filtre ve çıktı. Giriş bölümü Logstash işlem günlükleri giriş kaynağını belirler: Bu durumda, ağ güvenlik grubu akış erişmesine olanak sağlayan (sonraki adımlarda) bir Azure blogu giriş eklentisinin yüklü kullanacaksanız günlük JSON dosyaları blob depolama alanında depolanır.

Ayrı bir Logstash olay her tek tek akış tanımlama grubu ve ilişkili özellikleri haline gelebilmesi filtre bölümünde her akış günlük dosyası ardından düzleştirir.

Son olarak, çıkış bölümüne her Logstash olay Graylog sunucusuna iletir. Kendi belirli gereksinimlerine uyacak şekilde, Logstash yapılandırma dosyası, gerektiği gibi değiştirin.

   > [!NOTE]
   > Önceki yapılandırma dosyası Graylog sunucunun yerel konak geri döngü IP adresinde 127.0.0.1 yapılandırıldığını varsayar. Aksi durumda, çıkış bölümünde konak parametresi için doğru IP adresi değiştirdiğinizden emin olun.

Logstash Logstash yükleme hakkında daha fazla yönerge için bkz. [belgeleri](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html).

### <a name="install-the-logstash-input-plug-in-for-azure-blob-storage"></a>Azure blob depolama için eklenti Logstash giriş yükleyin

Logstash eklentisini akış günlüklerini kullanıcıların belirtilen blob depolama hesabından doğrudan erişmesini sağlar. Eklentinin varsayılan Logstash yükleme dizini (büyük/küçük harf bu /usr/share/logstash/bin) yüklemek için aşağıdaki komutu çalıştırın:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Bu Tak hakkında daha fazla bilgi için bkz: [belgeleri](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="set-up-connection-from-logstash-to-graylog"></a>Bağlantı kurmak için Graylog Logstash'ten elasticsearch'e ayarlayın

Logstash kullanarak akış günlüklerini bağlantı ve Graylog sunucu ayarlama göre Graylog gelen günlük dosyalarını kabul edecek şekilde yapılandırmanız gerekir.

1. İçin yapılandırılan URL kullanan, Graylog sunucu web arabirimine gidin. Tarayıcınıza yönlendirerek arabirimi erişebilirsiniz. `http://<graylog-server-ip>:9000/`

2. Yapılandırma sayfasına gitmek için **sistem** açılır menüde, üst gezinti çubuğunda sağ ve ardından **girişleri**.
   Alternatif olarak, gidin `http://<graylog-server-ip>:9000/system/inputs`

   ![Başlarken](./media/network-watcher-analyze-nsg-flow-logs-graylog/getting-started.png)

3. Yeni giriş başlatmak için *GELF UDP* içinde **seçin giriş** açılır menüsünü ve ardından formu doldurun. GELF Graylog Genişletilmiş günlük biçimini anlamına gelir. GELF biçimi Graylog tarafından geliştirilmiştir. Graylog kendine göre avantajları hakkında daha fazla bilgi için bkz [belgeleri](https://docs.graylog.org/en/2.2/pages/gelf.html).

   Graylog sunucunuz üzerinde yapılandırılmış IP girişi bağlamak emin olun. IP adresi eşleşmelidir **konak** Logstash yapılandırma dosyasının UDP çıkış alanı. Varsayılan bağlantı noktası olmalıdır *12201*. Eşleşen bağlantı sağlamak **bağlantı noktası** UDP alanında çıkış Logstash yapılandırma dosyasında belirtilen.

   ![Girişler](./media/network-watcher-analyze-nsg-flow-logs-graylog/inputs.png)

   Giriş başlatma sonra altında görünmesini görmeniz gerekir **yerel girişleri** aşağıdaki resimde gösterildiği gibi bölüm:

   ![](./media/network-watcher-analyze-nsg-flow-logs-graylog/local-inputs.png)

   Graylog ileti girişleri hakkında daha fazla bilgi için bkz [belgeleri](https://docs.graylog.org/en/2.2/pages/sending_data.html#what-are-graylog-message-inputs).

4. Bu yapılandırmaları yaptıktan sonra Logstash'i aşağıdaki komutla akış günlüklerini okuma başlamak için başlatabilirsiniz: `sudo systemctl start logstash.service`.

### <a name="search-through-graylog-messages"></a>Graylog iletiler arasında arama

İletileri toplanacak Graylog sunucunuz için bir süre sonra izin, iletiler arasında arama yapabilir. Graylog sunucunuza gönderilmekte olan iletileri kontrol etmek için **girişleri** yapılandırma sayfasını tıklatın "**Show alınan iletileri**" düğmesini GELF UDP oluşturduğunuz giriş. Aşağıdaki resme benzer bir ekran yönlendirilirsiniz: 

![Çubuk grafik](./media/network-watcher-analyze-nsg-flow-logs-graylog/histogram.png)

Mavi "% {Message}" bağlantısına tıklayarak her ileti her akış demet parametreleri göstermek için aşağıdaki resimde gösterilen şekilde genişletir:

![İletiler](./media/network-watcher-analyze-nsg-flow-logs-graylog/messages.png)

Aramak için bir özel ileti alan seçmezseniz, varsayılan olarak, tüm ileti alanlar aramaya eklenir. (Yani belirli iletileri arama yapmak istiyorsanız – belirli bir gelen akış diziler kaynak IP) Graylog arama sorgu dili olarak kullanabileceğiniz [belgelenen](https://docs.graylog.org/en/2.2/pages/queries.html)

## <a name="analyze-network-security-group-flow-logs-using-graylog"></a>Graylog kullanarak ağ güvenlik grubu akış günlüklerini analiz edin

Graylog çalıştıran ayarlamanız, akış günlük verilerinizi daha iyi anlamak için bazı işlevlerini kullanabilirsiniz. Verilerinizin belirli görünümler oluşturmak için panoları kullanarak bu yollardan biridir.

### <a name="create-a-dashboard"></a>Bir pano oluşturma

1. Üst gezinti çubuğunda **panolar** veya gidin `http://<graylog-server-ip>:9000/dashboards/`

2. Burada, yeşil tıklayın **Oluştur Pano** düğmesine tıklayın ve başlık ve açıklama panonuzun ile kısa formunu doldurun. İsabet **Kaydet** düğmesini yeni bir pano oluşturun. Aşağıdaki resme benzer bir Pano görürsünüz:

    ![Panolar](./media/network-watcher-analyze-nsg-flow-logs-graylog/dashboards.png)

### <a name="add-widgets"></a>Pencere öğesi ekleme

Başlığı görmek, ancak herhangi bir pencere öğeleri ekledik henüz bu yana, boş, şu anda panonun tıklayabilirsiniz. Panoya eklemek için bir kolay ve kullanışlıdır türü pencere öğesi olan **hızlı değerleri** grafikleri, seçili alanı ve bunların dağıtım değerlerin bir listesini görüntüler.

1. Arama sonuçlarını seçerek akış günlüklerini alma UDP giriş geri gidin **arama** üst gezinti çubuğundan.

2. Altında **arama sonucu** panel için ekranın sol tarafında, Bul **alanları** sekmesini her gelen akış demet ileti çeşitli alanlarını listeler.

3. Burada görselleştirmek istenen herhangi bir parametre seçin (Bu örnekte, IP kaynak seçilir). Olası pencere öğeleri listesinde gösterilecek mavi alanın solundaki aşağı açılan oka tıklayın ve ardından **hızlı değerleri** pencere öğesi oluşturmak için. Aşağıdaki resme benzer bir şey görmeniz gerekir:

   ![Kaynak IP](./media/network-watcher-analyze-nsg-flow-logs-graylog/srcip.png)

4. Burada, seçtiğiniz **panoya ekleme** pencere sağ üst köşesine düğmesini tıklatın ve eklemek için ilgili panoyu seçin.

5. Yeni eklediğiniz pencere öğesi görmek için panosuna geri gidin.

   Diğer pencere öğeleri gibi çubuk grafikler ve sayıları çeşitli ilişkin örnek panoyu aşağıdaki resimde gösterildiği gibi önemli ölçümleri izlemek için panonuza ekleyebilirsiniz:

   ![Flowlogs Panosu](./media/network-watcher-analyze-nsg-flow-logs-graylog/flowlogs-dashboard.png)

    Graylog için ın panolar ve pencere öğeleri diğer türleri hakkında daha fazla açıklama için başvuru [belgeleri](https://docs.graylog.org/en/2.2/pages/dashboards.html).

Ağ İzleyicisi Graylog ile tümleştirerek, artık yönetmek ve ağ güvenlik grubu akış günlüklerini görselleştirme için kullanışlı ve merkezi bir yolu var. Graylog akışları ve daha fazla akış günlüklerini yönetmek ve ağ trafiğinizi daha iyi anlamak için de kullanılabilir uyarılar gibi diğer güçlü özelliklere sahiptir. Graylog olduğuna göre ayarlama ve Azure'a bağlı, sunduğu diğer işlevleri keşfetmeye devam çekinmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek, ağ güvenlik grubu akış günlüklerini Power BI ile görselleştirmeyi öğrenmek [Görselleştir ağ güvenlik grubu akış günlüklerini Power BI ile](network-watcher-visualize-nsg-flow-logs-power-bi.md).
