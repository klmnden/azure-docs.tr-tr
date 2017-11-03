---
title: "Azure ağ güvenlik grubu akış günlükleri - Graylog çözümleme | Microsoft Docs"
description: "Yönetmek ve ağ güvenlik grubu akış günlükleri Ağ İzleyicisi'ni ve Graylog kullanarak azure'da analiz öğrenin."
services: network-watcher
documentationcenter: na
author: mareat
manager: vitinnan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: mareat
ms.openlocfilehash: 1d79b775e97765a48be48a96cf10bc9435b4539b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-and-analyze-network-security-group-flow-logs-in-azure-using-network-watcher-and-graylog"></a>Yönetmek ve Ağ İzleyicisi'ni ve Graylog kullanarak azure'daki ağ güvenlik grubu akışı günlüklerini analiz edin

[Ağ güvenlik grubu akış günlükleri](network-watcher-nsg-flow-logging-overview.md) Azure ağ arabirimleri için giriş ve çıkış IP trafiği anlamak için kullanabileceğiniz bilgiler sağlar. Akış günlükleri göster giden ve gelen akışlarının bir ağ güvenlik grubu kural temeli ağ arabirimi, 5-tanımlama grubu (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü) ilgili bilgilere Akış akış uygular ve trafiğe izin verilen veya reddedilen .

Akış günlüğü etkin ağınızda, birçok ağ güvenlik grupları olabilir. Akış günlüğü etkin çeşitli ağ güvenlik gruplarıyla ayrıştırabilir ve günlüklerinizi ilişkin bilgiler elde etmek için sıkıcı yapabilirsiniz. Bu makalede Graylog bir açık kaynak günlüğü yönetimi ve çözümleme aracı ve Logstash, bir açık kaynak sunucu tarafı veri işleme işlem hattı kullanarak bu ağ güvenlik grubu akışı günlüklerini merkezi olarak yönetmek için bir çözüm sağlar.

## <a name="scenario"></a>Senaryo

Ağ güvenlik grubu akış günlükleri, Ağ İzleyicisi'ni kullanarak etkinleştirilir. Akış günlükleri akışı Azure blob depolama. Logstash eklentisi bağlanmak ve blob depolamadan akış günlükleri işlemek ve bunları Graylog için göndermek için kullanılır. Akış günlükleri Graylog depolandıktan sonra bunlar analiz ve değiştirebilirsiniz özelleştirilmiş panolarında oturum görselleştirilen.

! [Graylog iş akışı]] (. / media/network-watcher-analyze-nsg-flow-logs-graylog/workflow.png)

## <a name="installation-steps"></a>Yükleme Adımları

### <a name="enable-network-security-group-flow-logging"></a>Ağ güvenlik grubu akışı günlük kaydını etkinleştir

Bu senaryo için ağ güvenlik grubu akışı oturumu hesabınızdaki en az bir ağ güvenlik grubu etkin olması gerekir. Güvenlik grubu akış günlükleri etkinleştirme yönergeleri ağ için aşağıdaki makaleye bakın [akış günlüğü ağ güvenlik grupları için giriş](network-watcher-nsg-flow-logging-overview.md).

### <a name="setting-up-graylog"></a>Graylog ayarlama

Bu örnekte, Graylog ve Logstash, Azure'da dağıtılan bir Ubuntu 14.04 sunucusunda yapılandırılır.

- Başvurmak [belgelerine](http://docs.graylog.org/en/2.2/pages/installation/os/ubuntu.html) Graylog hakkında adım adım yönergeler için yükleme Ubuntu.
- Ayrıca izleyerek Graylog web arabirimi yapılandırdığınızdan emin olun [belgelerine](http://docs.graylog.org/en/2.2/pages/configuration/web_interface.html#configuring-webif).

Bu örnek, minimum Graylog Kurulum (yani kullanır. tek bir örneğini bir Graylog), ancak Graylog tasarlanmış kaynaklar sistem ve üretim bağlı olarak ihtiyaçları ölçeklendirmek için. Graylog'ın mimari konuları veya derin Mimari Kılavuzu ile ilgili daha fazla bilgi için bkz [belgelerine](http://docs.graylog.org/en/2.2/pages/architecture.html) ve [Mimari Kılavuzu](https://www.slideshare.net/Graylog/graylog-engineering-design-your-architecture).

Graylog platform ve tercihlerine bağlı olarak birçok yolla yüklenebilir. Olası yükleme yöntemleri tam bir listesi için Graylog'in resmi bakın [belgelerine](http://docs.graylog.org/en/2.2/pages/installation.html). Graylog sunucu uygulaması Linux dağıtımları üzerinde çalışır ve önkoşullar şunlardır:

-   Oracle Java SE 8 veya sonraki sürümünü – [Oracle'nın yükleme belgeleri](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
-   Esnek 2.x arama (2.1.0 veya sonrası)- [Elasticsearch yükleme belgeleri](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/_installation.html)
-   MongoDB 2.4 veya sonraki – [MongoDB yükleme belgeleri](https://docs.mongodb.com/manual/administration/install-on-linux/)

### <a name="install-logstash"></a>Logstash yükleyin

Logstash akış tanımlama grubu düzeyinde biçimlendirilmiş JSON akış günlüklerine düzleştirmek için kullanılır. Akış günlükleri düzleştirme günlükleri düzenlemek ve Graylog içinde arama yapmak kolaylaştırır.

1.  Logstash yüklemek için aşağıdaki komutları çalıştırın:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```

2.  Akış günlükleri ayrıştırma ve bunları Graylog için göndermek için Logstash yapılandırın. Bir Logstash.conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3.  Aşağıdaki içeriği dosyaya ekleyin. Depolama hesabı ayrıntılarınızı yansıtmak üzere vurgulanan değerleri değiştirin:

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
Sağlanan Logstash yapılandırma dosyası üç bölümden oluşur: Giriş, filtre ve çıkış. Giriş bölümü Logstash işleyecek günlüklerinin giriş kaynağı belirler – bu durumda biz bize ağ güvenlik grubu akışı erişmesine izin veren (sonraki adımlarda yüklü) bir Azure blogu giriş eklentisi kullanacaksanız günlük JSON dosyaları blob depolama alanına depolanır.

    The filter section then flattens each flow log file so that each individual flow tuple and its associated properties becomes a separate Logstash event.

    Finally, the output section forwards each Logstash event to the Graylog server. To suit your specific needs, modify the Logstash config file, as required.

    > [!NOTE]
    > The previous config file assumes that the Graylog server has been configured on the local host loopback IP address 127.0.0.1. If not, be sure to change the host parameter in the output section to the correct IP address.

Logstash Logstash yükleme hakkında daha ayrıntılı yönergeler için bkz: [belgelerine](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html).

### <a name="install-the-logstash-input-plug-in-for-azure-blob-storage"></a>Azure blob depolama için eklenti Logstash giriş yükleyin

Logstash eklentisi doğrudan kendi atanmış blob depolama hesabından akış günlüklerine erişmesine izin verir. Varsayılan Logstash yükleme dizininde (Bu örnek /usr/share/logstash/bin) eklentisi, yüklemek için aşağıdaki komutu çalıştırın:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Bu bağlama hakkında daha fazla bilgi için bkz: [belgelerine](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="set-up-connection-from-logstash-to-graylog"></a>Logstash için Graylog bağlantısını Ayarla

Biz Logstash kullanarak akış günlükleri bağlantı ve Graylog sunucu ayarlama göre biz Graylog gelen günlük dosyalarını kabul edecek şekilde yapılandırmanız gerekir.

1.  Bunun için yapılandırılan URL, Graylog sunucu web Arabirimi'ni gidin. Tarayıcınıza yönlendirerek arabirimi erişebilirsiniz`http://<graylog-server-ip>:9000/`

2.  Yapılandırma sayfasına gitmek için seçin **sistem** açılır menüde üst gezinti çubuğu sağ tarafta ve ardından **girişleri**.
    Alternatif olarak, gidin`http://<graylog-server-ip>:9000/system/inputs`

    ![Başlarken](./media/network-watcher-analyze-nsg-flow-logs-graylog/getting-started.png)

3.  Yeni giriş başlatmak için seçin *GELF UDP* içinde **seçin giriş** aşağı açılır ve ardından formu doldurun. GELF Graylog Genişletilmiş günlük biçimi için anlamına gelir. GELF biçimi Graylog tarafından geliştirilmiştir. Kendi avantajları hakkında daha fazla bilgi için Graylog bkz [belgelerine](http://docs.graylog.org/en/2.2/pages/gelf.html).

    Giriş Graylog sunucunuz üzerinde yapılandırılmış IP bağlamak emin olun. IP adresi eşleşmelidir **konak** Logstash yapılandırma dosyasının UDP çıkış alanı. Varsayılan bağlantı noktası olmalıdır *12201*. Eşleşen olun **bağlantı noktası** UDP alanında çıktı Logstash yapılandırma dosyasında belirtilen.

    ![Girişleri](./media/network-watcher-analyze-nsg-flow-logs-graylog/inputs.png)

    Giriş başlatma sonra altında görünmesini görmelisiniz **yerel girişleri** bölümünde, aşağıdaki resimde gösterildiği gibi:

    ![](./media/network-watcher-analyze-nsg-flow-logs-graylog/local-inputs.png)

    Graylog ileti girişleri hakkında daha fazla bilgi için bkz [belgelerine](http://docs.graylog.org/en/2.2/pages/sending_data.html#what-are-graylog-message-inputs).

4.  Bu yapılandırmaları yaptıktan sonra aşağıdaki komutu kullanarak akış günlüklerine okumanın başlatılacağı Logstash başlatabilirsiniz:

    `sudo systemctl start logstash.service`

### <a name="search-through-graylog-messages"></a>Graylog iletileri üzerinden arama

İzin verme iletileri toplamak Graylog sunucunuz için bir süre sonra üzerinden ileti arayabilirsiniz. Graylog sunucunuza gönderilmekte iletileri denetlemek için **girişleri** yapılandırma sayfasını tıklatın "**Göster alınan iletileri**" GELF UDP düğmesine oluşturduğunuz giriş. Aşağıdaki resme benzer bir ekrana yönlendirilir: 

![Çubuk grafik](./media/network-watcher-analyze-nsg-flow-logs-graylog/histogram.png)

Mavi "% {Message}" bağlantıyı tıklatmak, aşağıdaki resimde gösterildiği gibi her akış tanımlama grubu parametrelerini göstermek için her ileti genişletir:

![İletiler](./media/network-watcher-analyze-nsg-flow-logs-graylog/messages.png)

Aramak için belirli bir ileti alanı seçmezseniz, varsayılan olarak, tüm ileti alanları aramada dahil edilir. Belirli iletileri (yani arama yapmak istiyorsanız – belirli bir gelen akış diziler kaynak IP) Graylog arama sorgu dili olarak kullanabileceğiniz [belgelenen](http://docs.graylog.org/en/2.2/pages/queries.html)


## <a name="analyze-network-security-group-flow-logs-using-graylog"></a>Graylog kullanarak ağ güvenlik grubu akışı günlüklerini çözümleme

Graylog çalıştıran ayarlamanız, biz kendi işlevlerin bazıları akış günlük verilerimizi daha iyi anlamak için kullanabilirsiniz. Bir gibi belirli veri görünümleri oluşturmak için panoları kullanarak yoludur.

### <a name="create-a-dashboard"></a>Bir pano oluşturun

1.  Üst gezinti çubuğunda seçin **panolar** veya gidin`http://<graylog-server-ip>:9000/dashboards/`

2.  Buradan, yeşil tıklatın **oluşturma Pano** düğmesini tıklatın ve kısa formu, başlığı ve panonuz açıklamasını doldurun. İsabet **kaydetmek** düğmesi yeni bir pano oluşturun. Aşağıdaki resme benzer bir Pano bakın:

    ![Panolar](./media/network-watcher-analyze-nsg-flow-logs-graylog/dashboards.png)

### <a name="add-widgets"></a>Pencere öğeleri ekleme

Panoyu görmek, ancak hiçbir pencere öğeleri eklediğimiz henüz bu yana, boş, şu anda başlığı tıklatabilirsiniz. Panoya eklemek için bir kolay ve kullanışlı türü pencere öğesi olan **hızlı değerleri** değerlerini seçili alanı ve bunların dağıtım listesini görüntülemek grafikleri.

1.  Akış günlükleri seçerek alıyor UDP giriş arama sonuçlarını geri gidin **arama** üst gezinti çubuğunda.

2.  Altında **arama sonucu** panel ekranın sol tarafındaki, Bul **alanları** her gelen akış tanımlama grubu iletisinin çeşitli alanları listeler sekmesi.

3.  Görselleştirmek istenen tüm parametresi seçin (Bu örnekte, biz kaynak IP seçtiğiniz). Olası pencere öğeleri listesinde göstermek için mavi alanın solundaki açılan oku tıklatın, ardından seçin **hızlı değerleri** pencere öğesi oluşturmak için. Aşağıdaki resme benzer bir şey görmeniz gerekir:

    ![Kaynak IP](./media/network-watcher-analyze-nsg-flow-logs-graylog/srcip.png)

4.  Buradan, seçtiğiniz **eklemek için Pano** pencere öğesini sağ üst köşesinde düğmesini tıklatın ve eklemek için karşılık gelen Pano seçin.

5.  Yeni eklediğiniz pencere öğesi görmek için panosuna geri gidin.

    Panonuza örneği Panosu aşağıdaki resimde gösterildiği gibi önemli ölçümleri izlemek için çeşitli çubuk grafikler ve sayısı gibi diğer pencere öğeleri ekleyebilirsiniz:

    ![Flowlogs Panosu](./media/network-watcher-analyze-nsg-flow-logs-graylog/flowlogs-dashboard.png)

    Panolar ve pencere öğeleri diğer türleri hakkında daha fazla açıklama için Graylog için 's başvuran [belgelerine](http://docs.graylog.org/en/2.2/pages/dashboards.html).

Ağ İzleyicisi Graylog ile tümleştirerek, biz şimdi yönetmek ve ağ güvenlik grubu akış günlükleri görselleştirmek için kullanışlı ve merkezi bir yolu yoktur. Graylog akışlar ve daha fazla akış günlüklerini yönetmek ve ağ trafiğinizi daha iyi anlamak için de kullanılabilir uyarıları gibi diğer güçlü özelliklere sahiptir. Ayarlama ve Azure'a bağlı, sunduğu diğer işlevleri keşfetmeye devam çekinmeyin Graylog sahip olduğunuza göre.

## <a name="next-steps"></a>Sonraki adımlar

Ağ güvenlik grubu akışı günlüklerinizi Power BI ile görselleştirme ziyaret ederek öğrenin [Görselleştir ağ güvenlik grubu akar günlükleri Power BI ile](network-watcher-visualize-nsg-flow-logs-power-bi.md).
