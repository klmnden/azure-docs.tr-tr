---
title: Azure sanal ağlarda HBase kümesi çoğaltma ayarlama | Microsoft Docs
description: HBase çoğaltmadan bir Hdınsight sürüm başka bir Yük Dengeleme, yüksek kullanılabilirlik, sıfır kapalı kalma süresi geçiş ve güncelleştirmeleri ve olağanüstü durum kurtarma için ayarlanacağını öğrenin.
services: hdinsight,virtual-network
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: c28c48b5842deec9d9c3898c5742c3d4d473094e
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="set-up-hbase-cluster-replication-in-azure-virtual-networks"></a>Azure sanal ağlarda HBase kümesi çoğaltmayı ayarlama

Bir sanal ağ içinde veya azure'da iki sanal ağ arasında HBase çoğaltmayı ayarlayın öğrenin.

Küme çoğaltma, bir kaynak itme Metodoloji kullanır. Her iki rolleri aynı anda gerçekleştirebilir veya bir kaynak veya hedef bir HBase kümesi olabilir. Çoğaltma zaman uyumsuz olarak çağrılır. Nihai tutarlılık çoğaltma hedefidir. Kaynak çoğaltma etkinleştirildiğinde, bir sütun ailesi için bir düzen aldığında, tüm hedef kümeler için düzenleme yayılır. Verileri bir kümeden diğerine çoğaltıldığında kaynak kümesi ve veri zaten tüketilen tüm kümeleri, çoğaltma döngüleri önlemek için izlenir.

Bu öğreticide, bir kaynak hedef çoğaltma kümesi. Diğer küme Topolojileri için bkz: [Apache HBase Başvuru Kılavuzu](http://hbase.apache.org/book.html#_cluster_replication).

Tek bir sanal ağı için HBase çoğaltma kullanım durumları verilmiştir:

* Yük dengeleme. Örneğin, tarama veya MapReduce işleri hedef kümede çalışan ve kaynak kümesi veri alma.
* Yüksek kullanılabilirlik ekleniyor.
* Bir HBase kümesi geçirme verileri başka bir.
* Azure Hdınsight kümesi başka bir sürümünden yükseltme.

İki sanal ağlar için HBase çoğaltma kullanım durumları verilmiştir:

* Olağanüstü durum kurtarma ayarlama.
* Yük Dengeleme ve uygulama bölümleme.
* Yüksek kullanılabilirlik ekleniyor.

Kümeleri kullanarak çoğaltabilirsiniz [betik eylemi](../hdinsight-hadoop-customize-cluster-linux.md) betikten [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz: [Azure ücretsiz deneme sürümünü edinin](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="set-up-the-environments"></a>Ortamları ayarlama

Üç yapılandırma seçeneğiniz vardır:

- Bir Azure sanal ağı iki HBase kümesi.
- İki farklı sanal ağlar aynı bölgede iki HBase kümesi.
- İki farklı bölgelerde (coğrafi çoğaltma) iki farklı sanal ağlar iki HBase kümesi.

Yardımcı olması için ortamları ayarlama, bazı oluşturduk [Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md). Diğer yöntemleri kullanarak ortamları ayarlama tercih ederseniz, bkz:

- [Hdınsight'ta Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
- [Azure sanal ağında HBase kümeleri oluşturma](apache-hbase-provision-vnet.md)

### <a name="set-up-one-virtual-network"></a>Bir sanal ağı ayarlama

Aynı sanal ağda iki HBase kümesi oluşturmak için aşağıdaki resimde seçin. Şablon konumunda depolanan [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

### <a name="set-up-two-virtual-networks-in-the-same-region"></a>İki sanal ağ aynı bölgede ayarlama

Sanal Ağ eşlemesi aynı bölgedeki iki HBase kümeleri ve iki sanal ağ oluşturmak için aşağıdaki resimde seçin. Şablon konumunda depolanan [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>



Bu senaryo gerektirir [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md). Sanal Ağ eşlemesi şablonu sağlar.   

HBase çoğaltmayı ZooKeeper vm'lerden IP adreslerini kullanır. Statik IP adresleri için hedef HBase ZooKeeper düğümleri ayarlamanız gerekir.

**Statik IP adresleri yapılandırmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüden seçin **kaynak grupları**.
3. Hedef HBase kümesi olan kaynak grubunu seçin. Bu ortamı oluşturmak için Resource Manager şablonu kullanıldığında, belirttiğiniz kaynak grubudur. Filtre listesini daraltmak için kullanabilirsiniz. İki sanal ağları içeren kaynakların bir listesini görebilirsiniz.
4. Hedef HBase kümesi içeren sanal ağı seçin. Örneğin, seçin **xxxx-vnet2'yi**. İle başlayan üç aygıtlarla adları **NIC-zookeepermode -** listelenir. Bu üç ZooKeeper VM'ler aygıtlardır.
5. ZooKeeper VM'ler birini seçin.
6. Seçin **IP yapılandırmaları**.
7. Listeden seçin **ipConfig1**.
8. Seçin **statik**, kopyalamak ve gerçek IP adresini yazın. Çoğaltmayı etkinleştirmek için betik eylemi çalıştırdığınızda, IP adresi gerekir.

  ![Hdınsight HBase çoğaltmayı ZooKeeper statik IP adresi](./media/apache-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Diğer iki ZooKeeper düğümleri için statik IP adresi ayarlamak için 6. adımı yineleyin.

Çapraz sanal ağı senaryosu için kullanmanız gerekir **- IP** çağırdığınızda geçiş `hdi_enable_replication.sh` betik eylemi.

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>İki farklı bölgelerdeki iki sanal ağ ayarlama

İki farklı bölgelerde ve sanal ağlar arasında VPN bağlantısı iki sanal ağ oluşturmak için aşağıdaki resimde'ı tıklatın. Şablon depolanan [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-geo%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Bazı şablonu sabit kodlanmış değerler:

**Sanal ağ 1**

| Özellik | Değer |
|----------|-------|
| Konum | Batı ABD |
| VNet adı | &lt;ClusterNamePrevix >-vnet1 |
| Adres alanı ön eki | 10.1.0.0/16 |
| Alt ağ adı | alt ağ 1 |
| Alt ağ öneki | 10.1.0.0/24 |
| Alt ağ (ağ geçidi) adı | GatewaySubnet (değiştirilemez) |
| (Ağ geçidi) alt ağ öneki | 10.1.255.0/27 |
| Ağ geçidi adı | vnet1gw |
| Geçit türü | VPN |
| Ağ geçidi VPN türü | RouteBased |
| Ağ geçidi SKU'su | Temel |
| ağ geçidi IP | vnet1gwip |
| Küme Adı | &lt;ClusterNamePrefix > 1 |
| Küme sürümü | 3.6 |
| Küme türü | hbase |
| Küme çalışan düğüm sayısı | 2 |


**VNet 2**

| Özellik | Değer |
|----------|-------|
| Konum | Doğu ABD |
| VNet adı | &lt;ClusterNamePrevix >-vnet2'ye |
| Adres alanı ön eki | 10.2.0.0/16 |
| Alt ağ adı | alt ağ 1 |
| Alt ağ öneki | 10.2.0.0/24 |
| Alt ağ (ağ geçidi) adı | GatewaySubnet (değiştirilemez) |
| (Ağ geçidi) alt ağ öneki | 10.2.255.0/27 |
| Ağ geçidi adı | vnet2gw |
| Geçit türü | VPN |
| Ağ geçidi VPN türü | RouteBased |
| Ağ geçidi SKU'su | Temel |
| ağ geçidi IP | vnet1gwip |
| Küme Adı | &lt;ClusterNamePrefix > 2 |
| Küme sürümü | 3.6 |
| Küme türü | hbase |
| Küme çalışan düğüm sayısı | 2 |

HBase çoğaltmayı ZooKeeper vm'lerden IP adreslerini kullanır. Statik IP adresleri için hedef HBase ZooKeeper düğümleri ayarlamanız gerekir. Statik IP ayarlamak için bkz: [iki sanal ağ aynı bölgede ayarlama](#set-up-two-virtual-networks-in-the-same-region) bu makalede.

Çapraz sanal ağı senaryosu için kullanmanız gerekir **- IP** çağırdığınızda geçiş `hdi_enable_replication.sh` betik eylemi.

## <a name="load-test-data"></a>Yük testi verileri

Bir küme çoğalttığınızda, çoğaltmak istediğiniz tabloları belirtmeniz gerekir. Bu bölümde, bazı veri kaynağı kümesine yükleyin. Sonraki bölümde, iki küme arasında çoğaltmayı etkinleştirir.

Oluşturmak için bir **kişiler** tablo ve bazı veri tablosuna eklemek, yönergeleri [HBase Öğreticisi: Hdınsight'ta Apache HBase kullanmaya başlamanıza](apache-hbase-tutorial-get-started-linux.md).

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Aşağıdaki adımlar komut dosyası eylemi komut dosyası Azure portalından çağırmak nasıl açıklar. Azure PowerShell ve Azure komut satırı aracı (Azure CLI) kullanarak bir komut dosyası eylemi çalıştırma hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md).

**HBase çoğaltmayı Azure portalından etkinleştirmek için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kaynak HBase kümesi'ni açın.
3. Küme menüde seçin **betik eylemleri**.
4. Sayfanın en üstünde seçin **gönderme yeni**.
5. Aşağıdaki bilgileri girin veya seçin:

  1. **Ad**: girin **çoğaltmasını etkinleştir**.
  2. **Komut dosyası URL'si bash**: girin **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  3.  **HEAD**: Bu seçildiğinde emin olun. Bir düğüm türleri temizleyin.
  4. **Parametreleri**: Aşağıdaki örnek parametreleri tüm mevcut tablolar için çoğaltmayı etkinleştirme ve tüm veriler kaynak kümeden hedef kümeye kopyalayın:

          -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata
    
    >[!note]
    >
    > Ana bilgisayar adı yerine FQDN'yi için hem kaynak hem de hedef küme DNS adına kullanın.

6. **Oluştur**’u seçin. Komut dosyası özellikle kullandığınızda, çalıştırmak için biraz zaman alabilir **- copydata** bağımsız değişkeni.

Gerekli bağımsız değişkenler:

|Ad|Açıklama|
|----|-----------|
|-s--src-küme | Kaynak HBase kümesi DNS adını belirtir. Örneğin: -s hbsrccluster,--src küme hbsrccluster = |
|-d--dst küme | Hedef (Çoğaltma) HBase kümesi DNS adını belirtir. Örneğin: -s dsthbcluster,--src küme dsthbcluster = |
|-sp,--src ambari parolası | Yönetici parolası için Ambari kaynak HBase kümede belirtir. |
|-dp,--dst ambari parolası | Yönetici parolası, Ambari için hedef HBase kümesi üzerinde belirtir.|

İsteğe bağlı bağımsız değişkenler:

|Ad|Açıklama|
|----|-----------|
|-su,--src ambari kullanıcı | Ambari için yönetici kullanıcı adı kaynak HBase kümede belirtir. Varsayılan değer **yönetici**. |
|-du,--dst ambari kullanıcı | Ambari için yönetici kullanıcı adı hedef HBase kümede belirtir. Varsayılan değer **yönetici**. |
|-t,--Tablo listesi | Çoğaltılacak tabloları belirtir. Örneğin: – Tablo listesi = "tablo1; tablo2; Tablo3". Tabloları belirtmezseniz, tüm var olan HBase tablolarını çoğaltılır.|
|-m,--makine | Betik eylemi çalıştığı baş düğüm belirtir. Ya da değerdir **hn1** veya **hn0**. Çünkü **hn0** baş düğüm genellikle yoğun, kullanmanızı öneririz **hn1**. 0 betiği Hdınsight portal ya da Azure PowerShell betik eylemi çalıştırıyorsanız bu seçeneği kullanın.|
|-IP | İki sanal ağ arasında çoğaltmayı etkinleştirirken gereklidir. Bu bağımsız değişken FQDN adları yerine çoğaltma küme ZooKeeper düğümü statik IP adreslerini kullanmak üzere bir geçiş olarak görev yapar. Çoğaltma etkinleştirmeden önce statik IP adresleri önceden gerekir. |
|-TP, - copydata | Çoğaltma etkinleştirdiğiniz tablolarda mevcut verilerin geçişini sağlar. |
|-rpm, - replicate-phoenix-meta | Phoenix sistem tablolarda çoğaltmayı etkinleştirir. <br><br>*Bu seçeneği dikkatli kullanın.* Bu komut dosyasını kullanmadan önce çoğaltma kümeleri Phoenix tablolarda yeniden oluşturmanızı öneririz. |
|-h,--Yardım | Kullanım bilgilerini görüntüler. |

`print_usage()` Bölümünü [betik](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) ayrıntılı bir açıklama parametreleri vardır.

Betik eylemi başarıyla dağıtıldıktan sonra hedef HBase kümeye bağlanın ve verilerin çoğaltıldığından emin olun için SSH kullanabilirsiniz.

### <a name="replication-scenarios"></a>Çoğaltma senaryoları

Aşağıdaki listede, bazı genel kullanım örnekleri ve parametre ayarlarına gösterir:

- **Tüm tablolarda iki küme arasında çoğaltmayı etkinleştirmek**. Bu senaryo, kopyalama veya tablolarda mevcut verileri geçirme gerektirmez ve Phoenix tabloları kullanmaz. Aşağıdaki parametreleri kullanın:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Belirli tablolarda çoğaltmayı etkinleştirme**. Tablo1 ve tablo2 Tablo3 çoğaltmayı etkinleştirmek için aşağıdaki parametreleri kullanın:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Belirli tablolarda çoğaltmayı etkinleştirmek ve var olan verileri kopyalamak**. Tablo1 ve tablo2 Tablo3 çoğaltmayı etkinleştirmek için aşağıdaki parametreleri kullanın:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Tüm tablolarda çoğaltmayı etkinleştirmek ve Phoenix meta veri kaynağından hedefe çoğaltmak**. Phoenix meta veri çoğaltma kusursuz değil. Dikkatli kullanın. Aşağıdaki parametreleri kullanın:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Kopyalama ve verileri geçirme

İki ayrı komut dosyası eylemi komut dosyalarını kopyalama veya çoğaltma etkinleştirildikten sonra veri geçirmek için kullanılabilir:

- [Küçük tablolar için komut dosyası](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (bir saatten kısa bir süre içinde tamamlanması birkaç gigabayttan tabloları boyut ve genel kopyalama bekleniyor)

- [Büyük tablolar için komut dosyası](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (kopyalamak için bir saatten daha uzun sürer beklenen tablolar)

Açıklanan aynı yordamı izleyin [çoğaltmasını etkinleştir](#enable-replication) betik eylemi çağırmak için. Aşağıdaki parametreleri kullanın:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

`print_usage()` Bölümünü [betik](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) parametrelerinin ayrıntılı açıklama bulunur.

### <a name="scenarios"></a>Senaryolar

- **Şimdiye kadar düzenlenen tüm satırlar için belirli tabloları (test1, test2 ve test3) kopyalayın (geçerli zaman damgası)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  Ya da:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Belirtilen zaman aralığı belirli tablolarla kopyalama**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Çoğaltmayı devre dışı bırakma

Çoğaltma devre dışı bırakmak için başka bir komut dosyası eylemi betikten kullanmak [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Açıklanan aynı yordamı izleyin [çoğaltmasını etkinleştir](#enable-replication) betik eylemi çağırmak için. Aşağıdaki parametreleri kullanın:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

`print_usage()` Bölümünü [betik](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) ayrıntılı bir açıklama parametreleri vardır.

### <a name="scenarios"></a>Senaryolar

- **Çoğaltma tüm tablolarda devre dışı**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  or

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Çoğaltmayı belirtilen tablolarda (tablo1, tablo2 ve Tablo3) devre dışı**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağ içinde ya da iki sanal ağ arasında HBase çoğaltmayı ayarlayın öğrendiniz. Hdınsight ve HBase hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight'ta Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md)
* [Hdınsight Hbase'e genel bakış](./apache-hbase-overview.md)
* [Azure sanal ağında HBase kümeleri oluşturma](./apache-hbase-provision-vnet.md)

