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
ms.date: 05/11/2018
ms.author: jgao
ms.openlocfilehash: 56b2b5ae9d3e4a0e682ec3dd47cd5cc30ebf6d58
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
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

Bu makalede, coğrafi çoğaltma senaryo yer almaktadır.

Yardımcı olması için ortamları ayarlama, bazı oluşturduk [Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md). Diğer yöntemleri kullanarak ortamları ayarlama tercih ederseniz, bkz:

- [Hdınsight'ta Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
- [Azure sanal ağında HBase kümeleri oluşturma](apache-hbase-provision-vnet.md)

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>İki farklı bölgelerdeki iki sanal ağ ayarlama

İki sanal ağ iki farklı bölgeler ve sanal ağlar arasında VPN bağlantısı oluşturmak için oluşturmak için aşağıdaki görüntüyü seçin. Şablon [ortak blob depolama] saklanır] (https://hditutorialdata.blob.core.windows.net/hbaseha/azuredeploy.json).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

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

## <a name="setup-dns"></a>DNS Kurulumu

Son bölümünde her iki sanal ağların bir Ubuntu sanal makine şablonu oluşturur.  Bu bölümde, iki DNS sanal makinelerde bağlama yükleyin ve sonra iki sanal makinelerde DNS iletmeyi yapılandırın.

Bağ yüklemek için yon iki DNS sanal makinelerin genel IP adresi bulmanız gerekir.

1. [Azure portalı](https://portal.azure.com) açın.
2. DNS sanal makine seçerek açın **kaynak gruplarına > [kaynak grubu adı] > [vnet1DNS]**.  Kaynak grubu adı son yordamda oluşturduğunuz adrestir. Varsayılan DNS sanal makine adları *vnet1DNS* ve *vnet2NDS*.
3. Seçin **özellikleri** sanal ağ özellikleri sayfasını açın.
4. Yazma **genel IP adresi**ve ayrıca doğrulayın **özel IP adresi**.  Özel IP adresi tutulamaz **10.1.0.4** vnet1DNS için ve **10.2.0.4** vnet2DNS için.  

Bağ yüklemek için aşağıdaki yordamı kullanın:

1. SSH bağlanmak için kullandığı __genel IP adresi__ DNS sanal makinenin. Aşağıdaki örnek bir sanal makinenin 40.68.254.142 bağlanır:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Değiştir `sshuser` DNS sanal makine oluştururken belirttiğiniz SSH kullanıcı hesabıyla.

    > [!NOTE]
    > Bir elde etmek için çeşitli yollar vardır `ssh` yardımcı programı. Linux, Unix ve macOS, işletim sisteminin bir parçası sağlanır. Windows kullanıyorsanız, aşağıdaki seçeneklerden birini göz önünde bulundurun:
    >
    > * [Azure bulut Kabuğu](../../cloud-shell/quickstart.md)
    > * [Windows 10 Ubuntu bash](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git)https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH)https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Bağ yüklemek için SSH oturumundan aşağıdaki komutları kullanın:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Şirket içi DNS sunucunuz için ad çözümleme isteklerini iletmek için bağlama yapılandırmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.options` dosyası:

    ```
    acl goodclients {
        10.1.0.0/16; # Replace with the IP address range of the virtual network 1
        10.2.0.0/16; # Replace with the IP address range of the virtual network 2
        localhost;
        localhost;
    };
    
    options {
        directory "/var/cache/bind";
        recursion yes;
        allow-query { goodclients; };

        forwarders {
            168.63.129.16 #This is the Azure DNS server
        };

        dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };
    ```
    
    > [!IMPORTANT]
    > Değerleri değiştirin `goodclients` iki sanal ağ IP adresi aralığı ile bölüm. Bu bölümde, bu DNS sunucusu gelen istekleri kabul adreslerini tanımlar.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__ve ardından __Enter__.

4. SSH oturumunda aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

    Bu komutu aşağıdaki metni benzer bir değer döndürür:

        vnet1DNS.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` Metin __DNS soneki__ bu sanal ağ için. Bu değer daha sonra kullanılmak üzere kaydedin.

    Diğer DNS sunucusundaki DNS soneki kullanıma bulmanız gerekir. Sonraki adımda ihtiyaç.

5. Sanal ağ içindeki kaynaklar için DNS adlarını çözümlemek için bağ yapılandırmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.local` dosyası:

    ```
    // Replace the following with the DNS suffix for your virtual network
    zone "v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net" {
            type forward;
            forwarders {10.2.0.4;}; # The Azure recursive resolver
    };
    ```

    > [!IMPORTANT]
    > Değiştirmeniz gereken `v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` bir sanal ağın DNS soneki. Ve iletici IP DNS sunucusunun diğer sanal ağdaki özel IP adresidir.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__ve ardından __Enter__.

6. Bağ başlatmak için aşağıdaki komutu kullanın:

    ```bash
    sudo service bind9 restart
    ```

7. Bu bağlama diğer sanal ağ kaynaklarında adlarını çözümleyebildiğini doğrulamak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt install dnsutils
    nslookup vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net 10.2.0.4
    ```

    > [!IMPORTANT]
    > Değiştir `vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` diğer ağ DNS sanal makinenin tam etki alanı adını (FQDN).
    >
    > Değiştir `10.2.0.4` ile __iç IP adresi__ özel DNS sunucunuzun diğer sanal ağ.

    Yanıt aşağıdakine benzer görünür:

    ```
    Server:         10.2.0.4
    Address:        10.2.0.4#53
    
    Non-authoritative answer:
    Name:   vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    Address: 10.2.0.4
    ```

    Şimdiye kadar belirtilen DNS sunucusu IP adresi olmadan diğer ağdan IP adresini bakın olamaz.

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Sanal ağ özel DNS sunucusunu kullanacak şekilde yapılandırma

Sanal ağ yerine Azure özyinelemeli çözümleyici özel DNS sunucusunu kullanacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

1. İçinde [Azure portal](https://portal.azure.com)sanal ağı seçin ve ardından __DNS sunucuları__.

2. Seçin __özel__ve girin __iç IP adresi__ özel DNS sunucusu. Son olarak, seçin __kaydetmek__.

6. DNS sunucusu sanal makinesi vnet1 açın ve tıklayın **yeniden**.  Etkili olması için DNS yapılandırmasını yapmak için sanal ağ tüm sanal makineleri yeniden başlatmalısınız.
7. Adımları yineleyin vnet2'özel DNS sunucusunu yapılandırın.

DNS yapılandırmasını test etmek için iki DNS SSH kullanarak sanal makineleri için bağlanabilir ve ana bilgisayar adını kullanarak bir sanal ağın DNS sunucusu ping işlemi yapın. İşe yaramazsa, DNS durumunu denetlemek için aşağıdaki komutu kullanın:

```bash
sudo service bind9 status
```

## <a name="create-hbase-clusters"></a>HBase kümeleri oluşturma

Her iki sanal ağ aşağıdaki yapılandırma ile bir HBase kümesi oluşturun:

- **Kaynak grubu adı**: sanal ağlar oluşturuldu olarak aynı kaynak grubu adı kullanın.
- **Küme türü**: HBase
- **Sürüm**: HBase 1.1.2 (HDI 3.6)
- **Konum**: aynı konumda sanal ağ olarak kullanın.  Varsayılan olarak, vnet1 olan *Batı ABD*, ve vnet2 *Doğu ABD*.
- **Depolama**: küme için yeni bir depolama hesabı oluşturun.
- **Sanal ağ** (Gelişmiş ayarlar portalındaki)'dan: son yordamda oluşturduğunuz vnet1 seçin.
- **Alt ağ**: şablonda kullanılan varsayılan addır **subnet1**.

Ortam doğru yapılandırıldığından emin olmak için iki küme arasındaki headnode ait FQDN ping mümkün olması gerekir.

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

