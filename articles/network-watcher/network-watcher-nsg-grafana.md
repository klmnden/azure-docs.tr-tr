---
title: Ağ güvenlik grubu akış Network Watcher ve Grafana kullanarak günlükleri yönetme | Microsoft Docs
description: Yönetme ve Network Watcher ve Grafana kullanarak azure'da ağ güvenlik grubu akış günlüklerini analiz edin.
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
ms.date: 09/15/2017
ms.author: mareat
ms.openlocfilehash: 73173c144f979d4a10b90a16aec783fe51a3f90e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62116262"
---
# <a name="manage-and-analyze-network-security-group-flow-logs-using-network-watcher-and-grafana"></a>Network Watcher ve Grafana kullanarak ağ güvenlik grubu akış günlüklerini analiz etme ve yönetme

[Ağ güvenlik grubu (NSG) akış günlüklerini](network-watcher-nsg-flow-logging-overview.md) , giriş ve çıkış IP trafiğini ağ arabirimleri üzerinde anlamak için kullanılan bilgileri sağlar. Akışlar giden ve gelen bu akış günlüklerini göster bir NSG kuralı temelinde hesaplamaz NIC akışı, 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) akışla ilgili uygular ve trafiğin izin verilen veya reddedilen.

> [!Warning]  
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için bkz [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md). Aşağıdaki yönergeler, değişiklik yapmadan günlük dosyalarının sürüm 2 ile çalışmaz.

Ağınızda birçok Nsg akış günlüğe kaydetme etkin olabilir. Bu günlük veri miktarı ayrıştırılamadı ve günlüklerinizi verilerden Öngörüler elde etmeye zahmetli hale getirir. Bu makalede, bu NSG akış günlüklerini Grafana, açık kaynaklı bir aracı, ElasticSearch, bir dağıtılmış arama ve analiz altyapısı ve bir açık kaynak sunucu tarafı veri işleme işlem hattı olan Logstash grafiklerini kullanarak merkezi olarak yönetmek için bir çözüm sağlar.  

## <a name="scenario"></a>Senaryo

NSG akış günlükleri, Ağ İzleyicisi kullanılarak etkinleştirilir ve Azure blob depolama alanında depolanır. Logstash eklentisi bağlanmak ve bunları Elasticsearch'e gönderin. akış günlüklerini blob depolamadan işlemek için kullanılır.  Akış günlüklerini Elasticsearch'e içinde depolandıktan sonra edilebilmeleri analiz ve Grafana, özelleştirilmiş panolarla görselleştirilir.

![NSG Network Watcher Grafana](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig1.png)

## <a name="installation-steps"></a>Yükleme adımları

### <a name="enable-network-security-group-flow-logging"></a>Etkinleştirme ağ güvenlik grubu akış günlüğe kaydetme

Bu senaryo için ağ güvenlik grubu akış hesabınızda en az bir ağ güvenlik grubu etkin günlüğü olması gerekir. Ağ güvenlik akış günlüklerini etkinleştirme hakkında yönergeler için şu makaleye başvurun [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md).

### <a name="setup-considerations"></a>Kurulumda dikkat edilecek noktalar

Bu örnekte, Grafana, ElasticSearch ve Logstash, Azure'da dağıtılan bir Ubuntu 16.04 LTS Server üzerinde yapılandırılır. En az Bu kurulum, üç bileşen çalıştırmak için kullanılır: tümü aynı sanal makinede çalışıyor. Bu ayar yalnızca için kullanılması gereken test ve kritik olmayan iş yükleri. Logstash, Elasticsearch ve Grafana tüm birçok örneklerinde bağımsız olarak ölçeklendirme desteklemesi. Daha fazla bilgi için bu bileşenlerin her birinin belgelerine bakın.

### <a name="install-logstash"></a>Logstash'i yükleyin

Bir akış tanımlama grubu düzeyinde JSON biçimli akış günlüklerini düzleştirilecek Logstash kullanın.

1. Logstash yüklemek için aşağıdaki komutları çalıştırın:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```

2. Akış günlüklerini ayrıştırmanıza ve Elasticsearch'e gönderin için Logstash'i yapılandırma. Kullanarak bir Logstash.conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Aşağıdaki içeriği dosyaya ekleyin. Depolama hesabınızın ayrıntıları yansıtmak için depolama hesabı adını ve erişim anahtarını değiştirin:

   ```bash
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

      mutate {
        split => { "[records][resourceId]" => "/"}
        add_field => { "Subscription" => "%{[records][resourceId][2]}"
          "ResourceGroup" => "%{[records][resourceId][4]}"
          "NetworkSecurityGroup" => "%{[records][resourceId][8]}" 
        }
        convert => {"Subscription" => "string"}
        convert => {"ResourceGroup" => "string"}
        convert => {"NetworkSecurityGroup" => "string"}
        split => { "[records][properties][flows][flows][flowTuples]" => "," }
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
      elasticsearch {
        hosts => "localhost"
        index => "nsg-flow-logs"
      }
    }
   ```

Sağlanan Logstash yapılandırma dosyası, üç bölümden oluşur: Giriş, filtre ve çıktı.
Giriş bölümü Logstash işleyecek – bu durumda, bize blob depolamada depolanan NSG akış günlüğü JSON dosyalara erişmesine izin verir (sonraki adımlarda) bir "azureblob" giriş eklentisinin yüklü kullanacağız günlükler giriş kaynağını belirtir. 

Ayrı bir Logstash olay her tek tek akış tanımlama grubu ve ilişkili özellikleri haline gelebilmesi filtre bölümünde her akış günlük dosyası ardından düzleştirir.

Son olarak, çıkış bölümüne her Logstash olay ElasticSearch sunucusuna iletir. Belirli ihtiyaçlarınıza uyacak şekilde Logstash yapılandırma dosyası değiştirmekten çekinmeyin.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Azure Blob Depolama için Logstash giriş eklentisini yükleme

Bu Logstash eklentisi, doğrudan akış günlüklerini kullanıcıların belirtilen blob depolama hesabından erişmenizi sağlar. Dizini (büyük/küçük harf bu /usr/share/logstash/bin) bu eklentisi varsayılan Logstash yükleme için komutu çalıştırın:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Bu Tak hakkında daha fazla bilgi için bkz: [giriş Logstash eklentisi için Azure depolama BLOB'ları](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="install-elasticsearch"></a>Elasticsearch'ü yükleme

Elasticsearch'ü yüklemek için aşağıdaki betiği kullanabilirsiniz. Elasticsearch'ü yükleme hakkında daha fazla bilgi için bkz: [Elastic Stack](https://www.elastic.co/guide/en/elastic-stack/current/index.html).

```bash
apt-get install apt-transport-https openjdk-8-jre-headless uuid-runtime pwgen -y
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | apt-key add -
echo "deb https://packages.elastic.co/elasticsearch/5.x/debian stable main" | tee -a /etc/apt/sources.list.d/elasticsearch-5.x.list
apt-get update && apt-get install elasticsearch
sed -i s/#cluster.name:.*/cluster.name:\ grafana/ /etc/elasticsearch/elasticsearch.yml
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
```

### <a name="install-grafana"></a>Grafana yükleyin

Yükleme ve Grafana çalıştırmak için aşağıdaki komutları çalıştırın:

```bash
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.1_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_4.5.1_amd64.deb
sudo service grafana-server start
```

Ek yükleme için bilgi [Debian üzerinde yükleme / Ubuntu](https://docs.grafana.org/installation/debian/).

#### <a name="add-the-elasticsearch-server-as-a-data-source"></a>ElasticSearch sunucu veri kaynağı olarak ekleyin.

Ardından, akış günlüklerini içeren bir veri kaynağı olarak ElasticSearch dizin eklemeniz gerekir. Seçerek bir veri kaynağı ekleyebilirsiniz **veri kaynağı Ekle** ve ilgili bilgilerle formu tamamlayın. Bu yapılandırmanın bir örneği aşağıdaki ekran görüntüsünde bulunabilir:

![Veri kaynağı ekleme](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig2.png)

#### <a name="create-a-dashboard"></a>Bir pano oluşturma

NSG akış günlüklerini içeren ElasticSearch dizinden okumaya Grafana başarıyla yapılandırıldı, oluşturma ve panoları kişiselleştirin. Yeni bir Pano oluşturmak için Seç **ilk panonuzu oluşturun**. Aşağıdaki örnek graph yapılandırma, NSG kuralı tarafından bölümlenmiş akışları gösterir:

![Pano grafiği](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig3.png)

Aşağıdaki ekran görüntüsünde, graph ve üst akışlar ve bunların sıklık gösteren grafiği gösterilmektedir. Akışlar da NSG kuralı tarafından gösterilir ve göre akışlar karar. Grafana üst düzeyde özelleştirilebilir olduğundan izleme gereksinimlerinize uyacak şekilde panolar oluşturmak önerilir. Aşağıdaki örnek, tipik bir Pano gösterilmektedir:

![Pano grafiği](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig4.png)

## <a name="conclusion"></a>Sonuç

Ağ İzleyicisi ElasticSearch ve Grafana ile tümleştirerek, artık yönetmek ve diğer verilerin yanı sıra, NSG akış günlüklerini görselleştirme için kullanışlı ve merkezi bir yolu var. Grafana birkaç daha fazla akış günlüklerini yönetmek ve ağ trafiğinizi daha iyi anlamak için kullanılabilir diğer güçlü grafik özellik vardır. Ayarlama ve Azure'a bağlı Grafana örnek olduğuna göre sunduğu diğer işlevleri keşfetmeye devam çekinmeyin.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Ağ İzleyicisi](network-watcher-monitoring-overview.md).

