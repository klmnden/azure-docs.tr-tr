---
title: "Azure'da bir geliştirme sanal makinede ElasticSearch dağıtma"
description: "Öğretici - Azure Linux VM'de geliştirme esnek yığına yükleme"
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
ms.openlocfilehash: 5b0b51504478cc0d501a89760ccd60808a69ccbd
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a>Esnek yığını bir Azure VM olanağına yükle

Bu makalede aracılığıyla nasıl dağıtılacağı anlatılmaktadır [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), ve [Kibana](https://www.elastic.co/products/kibana), azure'da bir Ubuntu VM üzerinde. Eylem esnek yığınında görmek için isteğe bağlı olarak Kibana için bağlanabilir ve bazı örnek veri günlüğü ile çalışır. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir Azure kaynak grubu bir Ubuntu VM oluşturma
> * Elasticsearch, Logstash ve Kibana'nı VM'e yükleyin
> * Örnek verileri Elasticsearch Logstash ile Gönder 
> * Bağlantı noktalarını açmak ve Kibana konsolunda verilerle çalışma


 Bu dağıtım, esnek yığını ile temel geliştirme için uygundur. Yığında esnek bir üretim ortamı için öneriler dahil olmak üzere daha fazla bilgi için bkz: [esnek belgelerine](https://www.elastic.co/guide/index.html) ve [Azure Mimari Merkezi](/azure/architecture/elasticsearch/).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#create) komutuyla bir sanal makine oluşturun. 

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

VM genel IP adresi zaten bilmiyorsanız, çalıştırmak [az ağ ortak IP listesi](/cli/azure/network/public-ip#list) komutu:

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. Sanal makineniz doğru ortak IP adresini değiştirin. Bu örnekte IP adresidir *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a>Esnek yığını yükleyin

Elasticsearch imzalama anahtarı içeri aktar ve esnek bir paket deposu içerecek şekilde APT kaynakları listenize güncelleştir:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

Java sanal VM yükleme ve yapılandırma değişkeni bu JAVA_HOME çalıştırmak esnek yığını bileşenleri için gereklidir.

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Ubuntu paket kaynaklarını güncelleştirmek ve Elasticsearch, Kibana ve Logstash yüklemek için aşağıdaki komutları çalıştırın.

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> Dizin düzenleri ve başlangıç yapılandırmasına dahil olmak üzere ayrıntılı yükleme yönergeleri korunur [esnek'ın belgeleri](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)

## <a name="start-elasticsearch"></a>Elasticsearch Başlat 

VM şu komutla Elasticsearch başlangıcı:

```bash
sudo systemctl start elasticsearch.service
```

Bu komut herhangi bir çıktı oluşturmaz, bu nedenle Elasticsearch bu VM üzerinde çalıştığını doğrulayın `curl` komutu:

```bash
curl -XGET 'localhost:9200/'
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

## <a name="start-logstash-and-add-data-to-elasticsearch"></a>Logstash başlatın ve Elasticsearch için veri ekleme

Logstash aşağıdaki komutla başlatın:

```bash 
sudo systemctl start logstash.service
```

Logstash düzgün çalıştığından emin olmak için etkileşimli modda test edin:

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

Temel logstash budur [ardışık düzen](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) standart çıktı için standart giriş görüntülemektedir. 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

Logstash çekirdek iletileri bu VM için Elasticsearch iletecek şekilde ayarlayın. Yeni bir dosya adlı boş dizin oluşturma `vm-syslog-logstash.conf` ve aşağıdaki Logstash yapılandırmasında yapıştırın:

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

Bu yapılandırmayı test etme ve Elasticsearch için syslog verileri gönder:

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

Terminalinizde Elasticsearch için gönderilen echoed syslog girişlerinden bakın. Kullanım `CTRL+C` bazı veriler gönderdik sonra dışında Logstash çıkmak için.

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a>Kibana başlatın ve Elasticsearch verileri görselleştirmek

Düzenleme `/etc/kibana/kibana.yml` ve Kibana zaman dinlediği web tarayıcınızdan erişebilmesi için IP adresini değiştirin.

```bash
server.host:"0.0.0.0"
```

Kibana aşağıdaki komutla başlatın:

```bash
sudo systemctl start kibana.service
```

Bağlantı noktası 5601 Kibana Konsolu'na uzaktan erişime izin vermek için Azure clı'dan açın:

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

Kibana konsolu açın ve seçin **oluşturma** gönderdiğiniz Elasticsearch için daha önce syslog verileri temel alan bir varsayılan dizini oluşturmak için. 

![Syslog olayları Kibana Gözat](media/elasticsearch-install/kibana-index.png)

Seçin **bulma** aramak için Kibana konsolda göz atın ve aracılığıyla syslog olaylarını filtreleyin.

![Syslog olayları Kibana Gözat](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir geliştirme azure'da VM içine esnek yığını dağıtıldı. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir Azure kaynak grubu bir Ubuntu VM oluşturma
> * Elasticsearch, Logstash ve Kibana'nı VM'e yükleyin
> * Örnek veriler için Elasticsearch Logstash Gönder 
> * Bağlantı noktalarını açmak ve Kibana konsolunda verilerle çalışma