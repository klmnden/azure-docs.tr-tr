---
title: Azure'da bir geliştirme sanal makinesi üzerinde ElasticSearch dağıtma
description: Öğretici - Azure’da bir geliştirme Linux VM'si üzerine Elastik Yığın yükleme
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rloutlaw
manager: justhe
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 10/11/2017
ms.author: routlaw
ms.openlocfilehash: 4d6dce952eca3d528a310685106a017dd7e3b80f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66166051"
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a>Azure VM üzerine Elastik Yığın yükleme

Bu makale, Azure’da bir Ubuntu VM üzerinde nasıl [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash) ve [Kibana](https://www.elastic.co/products/kibana) dağıtılacağını gösterir. Elastik Yığını uygulamaya konulurken görmek için, isteğe bağlı olarak Kibana’ya bağlanabilir ve birkaç örnek günlük verisiyle çalışabilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure kaynak grubunda Ubuntu VM oluşturma
> * Elasticsearch, Logstash ve Kibana'yı VM'ye yükleme
> * Örnek verileri Logstash ile Elasticsearch’e gönderme 
> * Kibana konsolunda bağlantı noktalarını açma ve verilerle çalışma


 Bu dağıtım, Elastik Yığın ile temel geliştirmeye uygundur. Elastik Yığın hakkında, üretim ortamı için önerilerin de dahil olduğu daha fazla bilgi için bkz. [Elastik belgeleri](https://www.elastic.co/guide/index.html) ve [Azure Mimari Merkezi](/azure/architecture/elasticsearch/).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Bir sanal makine oluştur

[az vm create](/cli/azure/vm) komutuyla bir sanal makine oluşturun. 

Aşağıdaki örnekte *myVM* adlı bir VM oluşturulur ve varsayılan anahtar konumunda henüz yoksa SSH anahtarları oluşturulur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir. `publicIpAddress` değerini not edin. Bu adres, VM’ye erişmek için kullanılır.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="ssh-into-your-vm"></a>VM’ye SSH uygulama

VM’nizin genel IP adresini bilmiyorsanız, [az network public-ip list](/cli/azure/network/public-ip) komutunu çalıştırın:

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. Sanal makinenizin doğru genel IP adresi ile değiştirdiğinizden emin olun. Bu örnekte IP adresi *40.68.254.142*’dir.

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a>Elastik Yığın’ı yükleme

Elastik paket deposunu eklemek için Elasticsearch imzalama anahtarını içeri aktarın ve APT kaynaklarınızı güncelleştirin:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

Java Sanal’ı VM’ye yükleyin ve JAVA_HOME değişkenini yapılandırın-Bu, Elastik Yığın bileşenlerinin çalışması için gereklidir.

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Ubuntu paket kaynaklarını güncelleştirmek için aşağıdaki komutları çalıştırın ve Elasticsearch, Kibana ve Logstash’i yükleyin.

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> Dizin düzenleri ve başlangıç yapılandırmasının da dahil olduğu ayrıntılı yükleme yönergeleri, [Elastik belgelerinde](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html) tutulur

## <a name="start-elasticsearch"></a>Elasticsearch’ü başlatma 

Aşağıdaki komutla Elasticsearch’ü VM’nizde başlatın:

```bash
sudo systemctl start elasticsearch.service
```

Bu komut herhangi bir çıktı üretmez, bu nedenle Elasticsearch’ün VM’de bu `curl` komutuyla çalıştığını doğrulayın:

```bash
sudo curl -XGET 'localhost:9200/'
```

Elasticsearch çalışıyorsa, aşağıdaki gibi bir çıktı görürsünüz:

```json
{
  "name" : "w6Z4NwR",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "SDzCajBoSK2EkXmHvJVaDQ",
  "version" : {
    "number" : "5.6.3",
    "build_hash" : "1a2f265",
    "build_date" : "2017-10-06T20:33:39.012Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```

## <a name="start-logstash-and-add-data-to-elasticsearch"></a>Logstash’i başlatma ve Elasticsearch’e veri ekleme

Logstash’i aşağıdaki komutla başlatın:

```bash 
sudo systemctl start logstash.service
```

Logstash’in düzgün çalıştığından emin olmak için etkileşimli modda test edin:

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

Bu, standart girişi standart çıktıya yansıtan temel bir logstash [işlem hattı](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html)’dır. 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

Çekirdek iletilerini bu VM’den Elasticsearch’e iletmek için Logstash’i ayarlayın. `vm-syslog-logstash.conf` adlı boş bir dizinde yeni bir dosya oluşturun ve aşağıdaki Logstash yapılandırmasını içine yapıştırın:

```Logstash
input {
    stdin {
        type => "stdin-type"
    }

    file {
        type => "syslog"
        path => [ "/var/log/*.log", "/var/log/*/*.log", "/var/log/messages", "/var/log/syslog" ]
        start_position => "beginning"
    }
}

output {

    stdout {
        codec => rubydebug
    }
    elasticsearch {
        hosts  => "localhost:9200"
    }
}
```

Bu yapılandırmayı test edin ve syslog verilerini Elasticsearch’e gönderin:

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

Terminalinizdeki syslog girişlerinin Elasticsearch’e gönderilirken yansıtıldığını görürsünüz. Birkaç veri gönderdikten sonra Logstash’ten çıkmak için `CTRL+C` tuşunu kullanın.

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a>Kibana’yı başlatma ve Elasticsearch’te verileri görselleştirme

Web tarayıcınızdan erişmek için `/etc/kibana/kibana.yml` öğesini düzenleyin ve Kibana’nın dinlediği IP adresini değiştirin.

```bash
server.host:"0.0.0.0"
```

Kibana’yı aşağıdaki komutla başlatın:

```bash
sudo systemctl start kibana.service
```

Kibana konsoluna uzaktan erişime izin vermek için Azure CLI'dan 5601 numaralı bağlantı noktasını açın:

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

Kibana konsolunu açın ve daha önce Elasticsearch’e gönderdiğiniz syslog verilerine göre varsayılan bir dizin oluşturmak için **Oluştur**’u seçin. 

![Kibana’da Syslog olaylarına göz atın](media/elasticsearch-install/kibana-index.png)

syslog olaylarını aramak, göz atmak ve filtrelemek için Kibana konsolunda **Keşfet**’i seçin.

![Kibana’da Syslog olaylarına göz atın](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Elastik Yığını Azure’da bir geliştirme VM’sine dağıttınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure kaynak grubunda Ubuntu VM oluşturma
> * Elasticsearch, Logstash ve Kibana'yı VM'ye yükleme
> * Örnek verileri Logstash’ten Elasticsearch’e gönderme 
> * Kibana konsolunda bağlantı noktalarını açma ve verilerle çalışma
