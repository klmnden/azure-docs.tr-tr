---
title: Ağ güvenlik grubu akış Ağ İzleyicisi'ni ve Grafana günlüklerini kullanarak yönetme | Microsoft Docs
description: Yönetebilir ve Ağ İzleyicisi'ni ve Grafana kullanarak azure'daki ağ güvenlik grubu akış günlüklerini analiz edin.
services: network-watcher
documentationcenter: na
author: kumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.openlocfilehash: 44cf074223c88b8fa539144c0d948e68ae6cbd13
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23864094"
---
# <a name="manage-and-analyze-network-security-group-flow-logs-using-network-watcher-and-grafana"></a>Yönetmek ve Ağ İzleyicisi'ni ve Grafana kullanarak ağ güvenlik grubu akışı günlüklerini çözümleme

[Ağ güvenlik grubu (NSG) akış günlükleri](network-watcher-nsg-flow-logging-overview.md) , giriş ve çıkış IP trafiği ağ arabirimlerindeki anlamak için kullanılan bilgileri sağlar. Bu akış günlükleri giden ve gelen akışları Göster bir NSG kuralı ayrı, NIC akışı, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü), 5-tanımlama grubu bilgilerini uygular ve trafiğe izin verilen veya reddedilen.

Akış günlüğü etkin ağınızda birçok Nsg'ler olabilir. Bu günlük veri miktarını ayrıştırabilir ve günlüklerinizi ilişkin bilgiler elde etmek için kullanışsız hale getirir. Bu makalede bu NSG akış günlüklerini Grafana, aracı, ElasticSearch, bir dağıtılmış arama ve analizi altyapısı ve açık kaynak sunucu tarafı veri işleme ardışık düzeni Logstash Grafikleme bir açık kaynak kullanarak merkezi olarak yönetmek için bir çözüm sağlar.  

## <a name="scenario"></a>Senaryo

NSG akış günlükleri, Ağ İzleyicisi kullanılarak etkinleştirildi ve Azure blob depolama alanına depolanır. Logstash eklentisi bağlanmak ve blob depolamadan akış günlükleri işlemek ve bunları ElasticSearch için göndermek için kullanılır.  Akış günlükleri ElasticSearch depolandıktan sonra bunlar analiz ve değiştirebilirsiniz Grafana özelleştirilmiş panolarında içine görselleştirilen.

![NSG Ağ İzleyicisi Grafana](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig1.png)

## <a name="installation-steps"></a>Yükleme adımları

### <a name="enable-network-security-group-flow-logging"></a>Ağ güvenlik grubu etkinleştirme akışı günlüğe kaydetme

Bu senaryo için ağ güvenlik grubu akış hesabınızdaki en az bir ağ güvenlik grubu etkin günlüğü olması gerekir. Ağ güvenlik akış günlükleri etkinleştirme ile ilgili yönergeler için aşağıdaki makaleye bakın [akış günlüğü ağ güvenlik grupları için giriş](network-watcher-nsg-flow-logging-overview.md).

### <a name="setup-considerations"></a>Kurulum konuları

Bu örnekte, Grafana, ElasticSearch ve Logstash, Azure'da dağıtılan bir Ubuntu 16.04 LTS sunucusunda yapılandırılır. Bu en az Kurulum her üç bileşenin çalıştırmak için kullanılan – tüm aynı VM üzerinde çalıştıkları. Bu ayar yalnızca için kullanılması gereken sınama ve kritik olmayan iş yükleri. Logstash, Elasticsearch ve Grafana tüm bağımsız olarak birçok örneklerinde ölçeklendirmek için tasarlanmış. Daha fazla bilgi için bu bileşenlerin her birini belgelerine bakın.

### <a name="install-logstash"></a>Logstash yükleyin

Bir akış tanımlama grubu düzeyinde biçimlendirilmiş JSON akış günlüklerine düzleştirilecek Logstash kullanın.

1. Logstash yüklemek için aşağıdaki komutları çalıştırın:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```

2. Akış günlükleri ayrıştırma ve bunları ElasticSearch için göndermek için Logstash yapılandırın. Kullanarak bir Logstash.conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Aşağıdaki içeriği dosyaya ekleyin. Depolama hesabı ayrıntılarınızı yansıtmak için depolama hesabı adı ve erişim anahtarını değiştirin:

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

Sağlanan Logstash yapılandırma dosyası üç bölümden oluşur: Giriş, filtre ve çıkış. Giriş bölümü Logstash işleyecek – bu durumda biz bize blob storage'da depolanan NSG akış günlük JSON dosyalara erişmesine izin veren (sonraki adımlarda yüklü) bir "azureblob" giriş eklentisi kullanacaksanız günlükler giriş kaynağı belirtir. 

Ayrı bir Logstash olay böylece her bireysel akış tanımlama grubu ve ilişkili özellikleri alır filtre bölümünde sonra her akış günlük dosyası düzleştirir.

Son olarak, çıktı bölümündeki her Logstash olay ElasticSearch sunucusuna iletir. Belirli gereksinimlerinize uygun olarak Logstash yapılandırma dosyası değiştirmekten çekinmeyin.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Azure Blob Depolama için Logstash giriş eklentisini yükleme

Bu Logstash eklentisi, kendi atanmış blob depolama hesabından akış günlükleri doğrudan erişmesini sağlar. Dizini (Bu örnek /usr/share/logstash/bin) bu eklentisi varsayılan Logstash yükleme için komutu çalıştırın:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Bu bağlama hakkında daha fazla bilgi için bkz: [Logstash giriş eklentisi için Azure Storage Bloblarında](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="install-elasticsearch"></a>ElasticSearch yükleyin

ElasticSearch yüklemek için aşağıdaki komut dosyasını kullanabilirsiniz. ElasticSearch yükleme hakkında daha fazla bilgi için bkz: [esnek yığını](https://www.elastic.co/guide/en/elastic-stack/current/index.html).

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

Yüklemek ve Grafana çalıştırmak için aşağıdaki komutları çalıştırın:

```bash
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.1_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_4.5.1_amd64.deb
sudo service grafana-server start
```

Ek yükleme için bilgi [Debian üzerinde yükleme / Ubuntu](http://docs.grafana.org/installation/debian/).

#### <a name="add-the-elasticsearch-server-as-a-data-source"></a>Bir veri kaynağı olarak ElasticSearch sunucu ekleme

Ardından, bir veri kaynağı olarak akış günlükleri içeren ElasticSearch dizin eklemeniz gerekir. Bir veri kaynağını seçerek ekleyebileceğiniz **veri kaynağı Ekle** ve ilgili bilgilerle form tamamlanıyor. Bu yapılandırmanın bir örneği aşağıdaki ekran görüntüsünde bulunabilir:

![Veri kaynağı ekleme](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig2.png)

#### <a name="create-a-dashboard"></a>Bir pano oluşturun

NSG akış günlükleri içeren ElasticSearch dizinden okumak için Grafana başarıyla yapılandırdınız, oluşturma ve panolar kişiselleştirme. Yeni bir Pano oluşturmak için seçin **ilk panonuzu oluşturma**. Aşağıdaki örnek grafik yapılandırmasını NSG kuralla kesimli akışları gösterir:

![Pano grafiği](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig3.png)

Aşağıdaki ekran görüntüsünde bir grafik ve üst akışları ve bunların sıklığı gösteren grafik gösterir. Akışlar aynı zamanda NSG kural tarafından gösterilir ve akışları tarafından karar. Grafana büyük ölçüde özelleştirilebilir olduğu belirli izleme gereksinimlerinize uygun olarak panoları oluşturmak önerilir. Aşağıdaki örnek, tipik bir Pano gösterir:

![Pano grafiği](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig4.png)

## <a name="conclusion"></a>Sonuç

Ağ İzleyicisi ElasticSearch ve Grafana ile tümleştirerek, yönetmek ve NSG akış günlükleri gibi diğer verileri görselleştirmek için kullanışlı ve merkezi bir yolu artık yoktur. Grafana daha fazla akış günlüklerini yönetmek ve ağ trafiğinizi daha iyi anlamak için de kullanılabilir diğer güçlü grafik özellikler sayısına sahip. Ayarlama ve Azure'a bağlı Grafana örneğine sahip olduğunuza göre sunduğu diğer işlevleri keşfetmeye devam çekinmeyin.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Ağ İzleyicisi](network-watcher-monitoring-overview.md).

