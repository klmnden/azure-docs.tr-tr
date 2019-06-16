---
title: Azure sanal ağları - Azure HDInsight içinde HBase kümesi çoğaltma ayarlama
description: HBase çoğaltmanın dışında bir HDInsight sürüm başka bir Yük Dengeleme, yüksek kullanılabilirlik, sıfır kapalı kalma süresiyle geçiş ve güncelleştirmeleri ve olağanüstü durum kurtarma ayarlama konusunda bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/15/2018
ms.openlocfilehash: 95a1055df283765b24322f6f8efe3efcb9b19022
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64707981"
---
# <a name="set-up-apache-hbase-cluster-replication-in-azure-virtual-networks"></a>Azure sanal ağlarda bulunan Apache HBase kümesi çoğaltma ayarlama

Nasıl ayarlanacağını öğrenin [Apache HBase](https://hbase.apache.org/) çoğaltma sanal ağ içindeki veya azure'daki iki sanal ağ arasında.

Küme çoğaltma, bir kaynak itme yöntemini kullanır. Her iki rolde tek seferde gerçekleştirebilir veya bir kaynak veya hedef bir HBase kümesi olabilir. Çoğaltma zaman uyumsuzdur. Çoğaltma son tutarlılık hedeftir. Kaynak çoğaltma etkinleştirildiğinde bir sütun ailesi için bir düzenleme aldığında, düzenleme için tüm hedef küme dağıtılır. Veriler bir kümeden diğerine çoğaltılır, kaynak kümesi ve veri zaten kullanmışsa tüm kümeler, çoğaltma döngüleri önlemek için izlenir.

Bu öğreticide, bir kaynak-hedef çoğaltma kümesi. Diğer küme Topolojileri için bkz. [Apache HBase Başvuru Kılavuzu](https://hbase.apache.org/book.html#_cluster_replication).

Tek bir sanal ağ için HBase çoğaltma kullanım örnekleri şunlardır:

* Yük dengeleme. Örneğin, taramalar veya MapReduce işleri, hedef küme üzerinde çalıştırılır ve verilerin kaynak kümede alımı.
* Yüksek kullanılabilirlik ekleniyor.
* Bir HBase kümesi diğerine geçirilen verileri.
* Bir Azure HDInsight kümesi başka bir sürümünden yükseltme.

İki sanal ağ için HBase çoğaltma kullanım örnekleri şunlardır:

* Olağanüstü durum kurtarma ayarlama.
* Yük Dengeleme ve uygulama bölümleme.
* Yüksek kullanılabilirlik ekleniyor.

Kümeleri kullanarak çoğaltabilirsiniz [betik eylemi](../hdinsight-hadoop-customize-cluster-linux.md) komut dosyalarını [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz: [Azure ücretsiz deneme sürümü edinin](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="set-up-the-environments"></a>Ortamları ayarlama

Üç yapılandırma seçeneğiniz vardır:

- Bir Azure sanal ağı iki Apache HBase kümeleri.
- Aynı bölgede iki farklı sanal ağlardaki iki Apache HBase kümeleri.
- İki Apache HBase kümeleri iki farklı sanal ağlardaki iki farklı bölgelerde (coğrafi çoğaltma).

Bu makalede, coğrafi çoğaltma senaryosuna değiniyor.

Yardımcı olması için ortamları ayarlama, bazı oluşturduk [Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md). Başka yöntemler kullanarak ortamı ayarlamak isterseniz, bkz:

- [HDInsight Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
- [Azure sanal ağ içinde Apache HBase kümeleri oluşturma](apache-hbase-provision-vnet.md)

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>İki farklı bölgelerdeki iki sanal ağ ayarlama

İki sanal ağı iki farklı bölge ve sanal ağlar arasında VPN bağlantısı oluşturan bir şablonu kullanmak için aşağıdakileri seçin **azure'a Dağıt** düğmesi. Şablon tanımının içinde depolanan bir [ortak blob depolama](https://hditutorialdata.blob.core.windows.net/hbaseha/azuredeploy.json).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Şablonu sabit kodlanmış değerler bazıları:

**VNet 1**

| Özellik | Değer |
|----------|-------|
| Location | Batı ABD |
| VNet adı | &lt;ClusterNamePrevix >-vnet1 |
| Adres alanı ön eki | 10.1.0.0/16 |
| Alt ağ adı | alt ağ 1 |
| Alt ağ ön eki | 10.1.0.0/24 |
| (Ağ geçidi) alt ağ adı | GatewaySubnet (değiştirilemez) |
| (Ağ geçidi) alt ağ ön eki | 10.1.255.0/27 |
| Ağ geçidi adı | vnet1gw |
| Ağ geçidi türü | VPN |
| Ağ geçidi VPN türü | RouteBased |
| Ağ geçidi SKU'su | Temel |
| Ağ geçidi IP | vnet1gwıp |

**VNet 2**

| Özellik | Değer |
|----------|-------|
| Location | Doğu ABD |
| VNet adı | &lt;ClusterNamePrevix >-vnet2'den |
| Adres alanı ön eki | 10.2.0.0/16 |
| Alt ağ adı | alt ağ 1 |
| Alt ağ ön eki | 10.2.0.0/24 |
| (Ağ geçidi) alt ağ adı | GatewaySubnet (değiştirilemez) |
| (Ağ geçidi) alt ağ ön eki | 10.2.255.0/27 |
| Ağ geçidi adı | vnet2gw |
| Ağ geçidi türü | VPN |
| Ağ geçidi VPN türü | RouteBased |
| Ağ geçidi SKU'su | Temel |
| Ağ geçidi IP | vnet1gwıp |

## <a name="setup-dns"></a>DNS Kurulumu

Son bölümde, şablon bir Ubuntu sanal makinesi her iki sanal ağ oluşturur.  Bu bölümde iki DNS sanal makinelere bağlama yükleyin ve sonra iki sanal makinelerde DNS iletmeyi yapılandırın.

Bağlama yüklemek için yon iki DNS sanal makinelerin genel IP adresini bulmak gerekir.

1. [Azure portalı](https://portal.azure.com) açın.
2. DNS sanal makine seçerek açın **kaynak grupları > [kaynak grubu adı] > [vnet1DNS]** .  Kaynak grubu adı son yordamda oluşturduğunuz paroladır. Varsayılan DNS sanal makine adları *vnet1DNS* ve *vnet2NDS*.
3. Seçin **özellikleri** sanal ağın özellikleri sayfasını açın.
4. Not **genel IP adresi**ve ayrıca doğrulayın **özel IP adresi**.  Özel IP adresi olmalıdır **10.1.0.4** vnet1DNS için ve **10.2.0.4** vnet2DNS için.  
5. DNS sunucuları her iki sanal ağ için aşağıdaki adımlarda bağlama yüklenecek paketleri indirmek gelen ve giden erişime izin vermek için varsayılan (Azure tarafından sağlanan) DNS sunucuları kullanmak üzere değiştirin.

Bağlama yüklemek için aşağıdaki yordamı kullanın:

1. Bağlanmak için SSH kullanın __genel IP adresi__ DNS sanal makinenin. Aşağıdaki örnek bir sanal makinenin 40.68.254.142 bağlanır:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Değiştirin `sshuser` DNS sanal makine oluştururken belirttiğiniz SSH kullanıcı hesabı ile.

    > [!NOTE]  
    > Bir elde etmek için çeşitli yollar vardır `ssh` yardımcı programı. Linux, Unix ve macOS üzerinde işletim sisteminin bir parçası olarak sağlanır. Windows kullanıyorsanız, aşağıdaki seçeneklerden birini göz önünde bulundurun:
    >
    > * [Azure Cloud Shell](../../cloud-shell/quickstart.md)
    > * [Windows 10 üzerinde ubuntu'da bash](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git)https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH)https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Bağlama yüklemek için SSH oturumunda aşağıdaki komutları kullanın:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Bağlama üzerinde şirket içi DNS sunucunuzun adı çözümleme istekleri iletecek şekilde yapılandırın. Bunu yapmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.options` dosyası:

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
            168.63.129.16; #This is the Azure DNS server
        };

        dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };
    ```
    
    > [!IMPORTANT]  
    > Değerleri değiştirin `goodclients` bölümünde iki sanal ağ ile IP adresi aralığı. Bu bölümde, bu DNS sunucusu gelen istekleri kabul eder adresleri tanımlar.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__, ardından __Enter__.

4. SSH oturumunda aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

    Bu komut, aşağıdaki metne benzer bir değer döndürür:

        vnet1DNS.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` Metin __DNS soneki__ bu sanal ağ için. Bu değeri kaydedin çünkü daha sonra kullanılacaktır.

    Ayrıca bir DNS sunucusu DNS sonekini bulmalıdır. Sonraki adımda ihtiyacınız.

5. Sanal ağ içindeki kaynaklar için DNS adlarını çözümlemek için bağlama yapılandırmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.local` dosyası:

    ```
    // Replace the following with the DNS suffix for your virtual network
    zone "v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net" {
            type forward;
            forwarders {10.2.0.4;}; # The Azure recursive resolver
    };
    ```

    > [!IMPORTANT]  
    > Değiştirmeniz gereken `v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` diğer sanal ağın DNS sonekine sahip. Ve iletici IP DNS sunucusunun bir sanal ağ özel IP adresidir.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__, ardından __Enter__.

6. Bağlama başlatmak için aşağıdaki komutu kullanın:

    ```bash
    sudo service bind9 restart
    ```

7. Bu bağlama bir sanal ağdaki kaynakların adlarını çözümleyebilen doğrulamak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt install dnsutils
    nslookup vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    ```

    > [!IMPORTANT]  
    > Değiştirin `vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` diğer ağ DNS sanal makinenin tam etki alanı adıyla (FQDN).
    >
    > Değiştirin `10.2.0.4` ile __iç IP adresi__ özel DNS sunucunuzun bir sanal ağ.

    Yanıt aşağıdaki metne benzer görünür:

    ```
    Server:         10.2.0.4
    Address:        10.2.0.4#53
    
    Non-authoritative answer:
    Name:   vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    Address: 10.2.0.4
    ```

    Şimdiye kadar belirtilen DNS sunucusu IP adresi olmayan diğer ağ IP adresini aramak olamaz.

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Sanal ağ özel DNS sunucusunu kullanacak şekilde yapılandırma

Özel DNS sunucusu yerine Azure özyinelemeli çözümleyici kullanılacak sanal ağı yapılandırmak için aşağıdaki adımları kullanın:

1. İçinde [Azure portalında](https://portal.azure.com)sanal ağı seçin ve ardından __DNS sunucuları__.

2. Seçin __özel__girin __iç IP adresi__ özel DNS sunucusu. Son olarak, seçin __Kaydet__.

6. DNS sunucusu sanal makine vnet1 içinde açın ve tıklayın **yeniden**.  Etkili olması için DNS yapılandırmasını yapmak için sanal ağdaki tüm sanal makineleri yeniden başlatmanız gerekir.
7. Yineleme adımları ve özel bir DNS sunucusu vnet2 için yapılandırın.

DNS yapılandırmasını test etmek için iki DNS SSH kullanarak sanal makinelerde bağlanmak ve diğer sanal ağın DNS sunucusu ana bilgisayar adını kullanarak ping atın. Bu işe yaramazsa, DNS durumunu denetlemek için aşağıdaki komutu kullanın:

```bash
sudo service bind9 status
```

## <a name="create-apache-hbase-clusters"></a>Apache HBase kümeleri oluşturma

Oluşturma bir [Apache HBase](https://hbase.apache.org/) her iki sanal ağı aşağıdaki yapılandırma ile küme:

- **Kaynak grubu adı**: oluşturduğunuz sanal ağlar aynı kaynak grubu adı kullanın.
- **Küme türü**: HBase
- **Sürüm**: HBase 1.1.2 (HDI 3.6)
- **Konum**: Sanal makine olarak aynı konumu kullanın.  Varsayılan olarak, vnet1 olduğu *Batı ABD*, ve vnet2 *Doğu ABD*.
- **Depolama**: Küme için yeni bir depolama hesabı oluşturun.
- **Sanal ağ** (ayarlarından Gelişmiş portal üzerinde): Son yordamda oluşturduğunuz vnet1'i seçin.
- **Alt ağ**: Şablonda kullanılan varsayılan addır **subnet1**.

Ortamı doğru şekilde yapılandırıldığından emin olmak için iki küme arasında baş düğümüne ait FQDN ping mümkün olması gerekir.

## <a name="load-test-data"></a>Test verilerini yükleme

Bir küme çoğalttığınızda, çoğaltmak istediğiniz tabloları belirtmeniz gerekir. Bu bölümde, kaynak kümede bazı veriler yükleyin. Sonraki bölümde, iki küme arasında çoğaltmaya olanak sağlar.

Oluşturmak için bir **kişiler** tablo ve tabloya bazı veriler ekleyin, konumundaki yönergeleri [Apache HBase Öğreticisi: HDInsight Apache HBase kullanmaya başlama](apache-hbase-tutorial-get-started-linux.md).

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Aşağıdaki adımlar komut dosyası eylemi komut dosyası Azure portalından çağırın açıklanmaktadır. Azure PowerShell ve klasik Azure CLI kullanarak bir betik eylemi hakkında daha fazla bilgi için bkz [özelleştirme HDInsight kümelerini betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md).

**HBase çoğaltmasını Azure portalından etkinleştirmek için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kaynak HBase kümesi açın.
3. Küme menüde **betik eylemleri**.
4. Sayfanın üst kısmında seçin **yeni Gönder**.
5. Seçin veya aşağıdaki bilgileri girin:

   1. **Ad**: Girin **çoğaltmayı etkinleştir**.
   2. **Bash betiği URL'si**: Girin **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh** .
   3. **HEAD**: Bu seçili olduğundan emin olun. Bir düğüm türleri temizleyin.
   4. **Parametreleri**: Aşağıdaki örnek parametreleri mevcut tüm tablolar için çoğaltmayı etkinleştirin ve ardından tüm veri kaynağı kümeden hedef kümeye kopyalayın:

          -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata
    
      > [!NOTE]
      > Ana bilgisayar adı yerine FQDN'yi için hem kaynak hem de hedef küme DNS adını kullanın.

6. **Oluştur**’u seçin. Betik özellikle kullandığınızda çalıştırmak için biraz zaman **- copydata** bağımsız değişken.

Gerekli bağımsız değişkenleri:

|Ad|Açıklama|
|----|-----------|
|-s--src-küme | Kaynak HBase küme DNS adını belirtir. Örneğin: -s hbsrccluster, küme içi--src hbsrccluster = |
|-d, --dst-cluster | Hedef (Çoğaltma) HBase küme DNS adını belirtir. Örneğin: -s dsthbcluster, küme içi--src dsthbcluster = |
|-sp, --src-ambari-password | Yönetici parolası, Ambari için kaynak HBase kümesi üzerinde belirtir. |
|-dp, --dst-ambari-password | Yönetici parolası, Ambari için hedef HBase kümesi üzerinde belirtir.|

İsteğe bağlı bağımsız değişkenleri:

|Ad|Açıklama|
|----|-----------|
|-su, --src-ambari-user | Ambari için yönetici kullanıcı adı kaynak HBase kümede belirtir. Varsayılan değer **yönetici**. |
|-du, --dst-ambari-user | Ambari için yönetici kullanıcı adı hedef HBase kümede belirtir. Varsayılan değer **yönetici**. |
|t-,--Tablo listesi | Çoğaltılacak tabloları belirtir. Örneğin:--Tablo listesi = "table1; table2; Tablo3". Tabloları belirtmezseniz, tüm mevcut HBase tablolarını çoğaltılır.|
|-m, --machine | Betik eylemi çalıştığı baş düğüm belirtir. Değerin geçerli **hn0** veya **hn1** ve seçilen etkin baş düğümü olduğu bağlı olmalıdır. 0 ABD Doları betiği HDInsight portalı ya da Azure PowerShell betik eylemi çalıştırıyorsanız bu seçeneği kullanın.|
|-TP, - copydata | Çoğaltma etkin olduğu tablolarda mevcut verilerin geçişini sağlar. |
|-rpm, - çoğaltma-phoenix-meta | Phoenix tablolarındaki çoğaltmayı etkinleştirir. <br><br>*Bu seçeneği dikkatli kullanın.* Bu betik kullanmadan önce Phoenix tablolarda çoğaltma kümelerini yeniden oluşturmanızı öneririz. |
|-h, --help | Kullanım bilgilerini görüntüler. |

`print_usage()` Bölümünü [betik](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) parametrelerinin ayrıntılı bir açıklama sahiptir.

Betik eylemi başarıyla dağıtıldıktan sonra hedef HBase kümeye bağlanın ve ardından verilerin çoğaltıldığından emin olun. SSH kullanabilirsiniz.

### <a name="replication-scenarios"></a>Çoğaltma senaryoları

Aşağıdaki liste, bazı genel kullanım örneklerinize ve parametre ayarlarını gösterir:

- **Tüm tablolar iki küme arasında çoğaltmayı etkinleştir**. Bu senaryo, kopyalama veya tablodaki mevcut verileri geçirme gerektirmez ve Phoenix tabloları kullanmaz. Aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Belirli tablolar çoğaltmayı etkinleştir**. Table1, table2 ve Tablo3 çoğaltmayı etkinleştirmek için aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Belirli tablolar üzerinde çoğaltmayı etkinleştirir ve var olan verileri kopyalamak**. Table1, table2 ve Tablo3 çoğaltmayı etkinleştirmek için aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Tüm tablolarda çoğaltmayı etkinleştirir ve Phoenix meta verileri kaynaktan hedefe çoğaltmak**. Phoenix meta veri çoğaltma kusursuz değil. Bunu dikkatli kullanın. Aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Kopyalama ve veri geçirme

İki ayrı bir komut dosyası eylemi komut dosyalarını kopyalama veya çoğaltma etkinleştirildikten sonra verileri geçirmek için kullanılabilir:

- [Küçük tablolar için betik](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (bir saatten kısa bir süre içinde tamamlanması birkaç gigabayttan boyut ve toplam kopyalama olan tabloları beklenir)

- [Büyük tablolar için betik](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (kopyalamak için bir saatten daha uzun sürmesine beklenen tablolar)

Açıklanan aynı yordamı izleyebilirsiniz [çoğaltmayı etkinleştir](#enable-replication) betik eylemi çağırmak için. Aşağıdaki parametreleri kullanın:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

`print_usage()` Bölümünü [betik](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) parametrelerinin ayrıntılı açıklama bulunur.

### <a name="scenarios"></a>Senaryolar

- **Şimdiye kadar düzenlenen tüm satırlar için belirli tablolar (test1 test2 ve test3) kopyalayın (geçerli zaman damgası)** :

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  veya:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Belirtilen zaman aralığı ile belirli bir tabloyu kopyalama**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Çoğaltmayı devre dışı bırakma

Çoğaltma devre dışı bırakmak için başka bir betik eylemi betikten kullanın [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Açıklanan aynı yordamı izleyebilirsiniz [çoğaltmayı etkinleştir](#enable-replication) betik eylemi çağırmak için. Aşağıdaki parametreleri kullanın:

    -m hn1 -s <source hbase cluster name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

`print_usage()` Bölümünü [betik](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) parametrelerinin ayrıntılı bir açıklama sahiptir.

### <a name="scenarios"></a>Senaryolar

- **Tüm tabloları çoğaltmayı devre dışı bırak**:

        -m hn1 -s <source hbase cluster name> -sp Mypassword\!789 -all
  or

        --src-cluster=<source hbase cluster name> --dst-cluster=<destination hbase cluster name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Belirtilen tabloların (table1, table2 ve Tablo3) çoğaltmayı devre dışı bırak**:

        -m hn1 -s <source hbase cluster name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağdaki veya iki sanal ağ arasında Apache HBase çoğaltmayı ayarlama öğrendiniz. HDInsight ve Apache HBase hakkında daha fazla bilgi edinmek için şu makalelere bakın:

* [HDInsight, Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md)
* [HDInsight Apache Hbase'e genel bakış](./apache-hbase-overview.md)
* [Azure sanal ağ içinde Apache HBase kümeleri oluşturma](./apache-hbase-provision-vnet.md)

