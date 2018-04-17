---
title: Hadoop Linux tabanlı Hdınsight üzerinde - Azure kullanma ipuçları | Microsoft Docs
description: Azure bulutta çalışan tanıdık bir Linux ortamda Linux tabanlı Hdınsight (Hadoop) kümeleri kullanarak uygulama ipuçları alın.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: 3ad7aa01200bf2bf4a63a380b2b883983c8622d6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="information-about-using-hdinsight-on-linux"></a>Linux’ta HDInsight kullanma ile ilgili bilgiler

Azure Hdınsight kümeleri Hadoop Azure bulutta çalışan bir bilinen Linux ortamı sağlar. Çoğu işlemler için tam olarak herhangi diğer Linux üzerinde Hadoop yükleme çalışması gerekir. Bu belge farkında olmanız gereken belirli farklılıkları çağırır.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Önkoşullar

Bu belgede yer alan adımlar birçoğu, sisteminizde yüklü gereken aşağıdaki yardımcı programlar, kullanın.

* [cURL](https://curl.haxx.se/) - web tabanlı Hizmetleri ile iletişim kurmak için kullanılan
* [jq](https://stedolan.github.io/jq/) - JSON belgeleri ayrıştırmak için kullanılan
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) - Azure hizmetlerini uzaktan yönetmek için kullanılan

## <a name="users"></a>Kullanıcılar

Sürece [etki alanına katılmış](./domain-joined/apache-domain-joined-introduction.md), Hdınsight sayılacağı bir **tek kullanıcı** sistem. Tek bir SSH kullanıcı hesabı, yönetici düzeyi izinleri olan bir küme ile oluşturulur. Ek SSH hesapları oluşturulabilir, ancak bunlar ayrıca küme için yönetici erişimine sahip.

Etki alanına katılmış Hdınsight birden çok kullanıcı ve daha ayrıntılı izni ve rol ayarlarını destekler. Daha fazla bilgi için bkz: [yönetmek etki alanına katılmış Hdınsight kümeleri](./domain-joined/apache-domain-joined-manage.md).

## <a name="domain-names"></a>Etki alanı adları

İnternet'ten kümeye bağlanırken kullanmak için tam etki alanı adı (FQDN)  **&lt;clustername >. azurehdinsight.net** veya (yalnızca SSH)  **&lt;clustername-ssh >. azurehdinsight.NET**.

Dahili olarak, kümedeki her düğümün küme yapılandırması sırasında atanmış bir adı vardır. Küme adlarını bulmak için bkz: **ana** Ambari Web kullanıcı arabirimini sayfasında. Ambari REST API konaklar listesini almak için aşağıdakileri de kullanabilirsiniz:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

**CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, yönetici hesabı için parolayı girin. Bu komut, kümedeki ana bilgisayarların listesini içeren bir JSON belgesi döndürür. Jq ayıklamak için kullanılan `host_name` her ana bilgisayar için öğe değeri.

Düğümün adı için belirli bir hizmet bulmanız gerekiyorsa, bu bileşen için Ambari sorgulayabilirsiniz. Örneğin, HDFS adı düğümünü ana bilgisayarları bulmak için aşağıdaki komutu kullanın:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Bu komut hizmet açıklayan bir JSON belgesi döndürür ve ardından jq yalnızca çeken `host_name` konakları için değer.

## <a name="remote-access-to-services"></a>Uzaktan Erişim Hizmetleri

* **Ambari (web)** -https://&lt;clustername >. azurehdinsight.net

    Küme Yöneticisi kullanıcı adı ve parola kullanarak kimlik doğrulaması ve Ambari için oturum açın.

    Kimlik doğrulama düz metin - her zaman bağlantı güvenliğini sağlamaya yardımcı olmak için HTTPS kullanır.

    > [!IMPORTANT]
    > Bazı web Uı'lar Ambari kullanılabilir bir iç etki alanı adını kullanarak düğümler erişin. İç etki alanı adları internet üzerinden genel olarak erişilebilir değildir. Bazı özelliklerin Internet üzerinden erişmeye çalışırken, "Sunucu bulunamadı" hataları alabilirsiniz.
    >
    > Ambari web kullanıcı Arabirimi tam işlevselliğini kullanmak için bir SSH tüneli küme baş düğümüne proxy web trafiği için kullanın. Bkz: [kullanım SSH Ambari web kullanıcı Arabirimi, ResourceManager, kaynak, iş, Oozie ve diğer web Uı'lar erişmek için tünel](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;clustername >.azurehdinsight.net/ambari

    > [!NOTE]
    > Küme Yöneticisi kullanıcı adı ve parola kullanarak kimlik doğrulaması.
    >
    > Kimlik doğrulama düz metin - her zaman bağlantı güvenliğini sağlamaya yardımcı olmak için HTTPS kullanır.

* **WebHCat (Templeton)** -https://&lt;clustername >.azurehdinsight.net/templeton

    > [!NOTE]
    > Küme Yöneticisi kullanıcı adı ve parola kullanarak kimlik doğrulaması.
    >
    > Kimlik doğrulama düz metin - her zaman bağlantı güvenliğini sağlamaya yardımcı olmak için HTTPS kullanır.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net bağlantı noktası 22 veya 23. Bağlantı noktası 22 23 ikincil bağlanmak için kullanılırken birincil headnode için bağlanmak için kullanılır. Baş düğümler hakkında daha fazla bilgi için bkz. [HDInsight’ta Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md).

    > [!NOTE]
    > Yalnızca bir istemci makinesinden SSH küme baş düğümler erişebilir. Bağlandıktan sonra daha sonra bir headnode SSH kullanarak çalışan düğümleri erişebilirsiniz.

Daha fazla bilgi için bkz: [hdınsight'ta Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](hdinsight-hadoop-port-settings-for-services.md) belge.

## <a name="file-locations"></a>Dosya konumları

Hadoop ilgili dosyaları küme düğümlerinde bulunabilir `/usr/hdp`. Bu dizin, aşağıdaki alt dizinleri içerir:

* **2.2.4.9-1**: dizin adı Hdınsight tarafından kullanılan Hortonworks veri platformu sürümüdür. Kümenizde sayı burada listelenen olandan farklı olabilir.
* **Geçerli**: Bu dizin altında alt dizinleri bağlantılar içeren **2.2.4.9-1** dizini. Sürüm numarasını hatırlamanız gerekmez, bu dizin zaten var.

Hadoop dağıtılmış dosya sistemi üzerinde örnek veriler ve JAR dosyalarını bulunabilir `/example` ve `/HdiSamples`

## <a name="hdfs-azure-storage-and-data-lake-store"></a>HDFS, Azure depolama ve veri Gölü deposu

Çoğu Hadoop dağıtımları içinde HDFS kümedeki makinelerde yerel depolama birimi tarafından desteklenmektedir. Kullanarak yerel depolama burada saat veya dakika işlem kaynakları için ücretlendirilirsiniz için bir bulut tabanlı çözüm pahalı olabilir.

Hdınsight Azure Storage blobları ya da Azure Data Lake Store varsayılan deposu olarak kullanır. Bu hizmetler aşağıdaki faydaları sağlar:

* Ucuz uzun vadeli depolama
* Web siteleri, dosya karşıya yükleme/indirme yardımcı programlar, çeşitli dil SDK'lar ve web tarayıcıları gibi dış hizmetler erişilebilirlik

Tek tek bloblar (veya bir Hdınsight açısından dosyaları) 195 GB'a kadar yalnızca gidebilirsiniz rağmen bir Azure Storage hesabı 4.75 TB'ye kadar basılı tutabilirsiniz. Azure Data Lake Store, dinamik olarak tek tek dosyaların bir petabyte büyük dosyalarla trilyonlarca tutmak için büyüyebilir. Daha fazla bilgi için bkz: [anlama BLOB'ları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) ve [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).

Azure Storage veya Data Lake Store kullanırken Hdınsight verilere erişmek için özel bir şey yapmanız gerekmez. Örneğin, aşağıdaki komut dosyalarında listeler `/example/data` olup Azure Storage veya Data Lake Store üzerinde depolandığı bağımsız olarak klasörü:

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a>URI ve düzeni

Bazı komutlar düzeni URI bir parçası olarak bir dosya erişirken belirlemenizi gerektirebilir. Örneğin, Storm HDFS bileşen düzeni belirtmenizi gerektirir. Varsayılan olmayan depolama ("ek" depolama alanı olarak kümeye eklenen depolama alanı) kullanırken, URI bir parçası olarak her zaman şeması kullanması gerekir.

Kullanırken __Azure Storage__, aşağıdaki URI şemaları birini kullanın:

* `wasb:///`: Şifrelenmemiş iletişimi kullanarak erişim varsayılan depolama.

* `wasbs:///`: Şifreli iletişim kullanarak erişim varsayılan depolama.  Wasbs düzeni yalnızca Hdınsight sürüm 3.6 sonraki sürümlerde desteklenir.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Bir varsayılan olmayan depolama hesabı ile iletişim kurarken kullanılır. Örneğin, bir ek depolama alanı hesabı veya ne zaman verilere genel olarak erişilebilir depolama hesabında depolanan olduğunda.

Kullanırken __Data Lake Store__, aşağıdaki URI şemaları birini kullanın:

* `adl:///`: Küme için varsayılan Data Lake Store erişin.

* `adl://<storage-name>.azuredatalakestore.net/`: Varsayılan olmayan Data Lake Store ile iletişim kurarken kullanılır. Hdınsight kümenizin kök dizini dışındaki verilere erişmek için de kullanılır.

> [!IMPORTANT]
> Data Lake Store Hdınsight için varsayılan deposu olarak kullanırken, Hdınsight depolama kökü olarak kullanılacak deposundaki bir yol belirtmeniz gerekir. Varsayılan yol `/clusters/<cluster-name>/`.
>
> Kullanırken `/` veya `adl:///` verilere erişmek için yalnızca kök dizininde depolanan verilere erişebilirsiniz (örneğin, `/clusters/<cluster-name>/`) kümenin. Veri deposu başka bir yerindeki erişmek için `adl://<storage-name>.azuredatalakestore.net/` biçimi.

### <a name="what-storage-is-the-cluster-using"></a>Hangi depolama küme kullanıyor mu

Ambari, kümenin varsayılan depolama yapılandırması almak için kullanabilirsiniz. Curl kullanarak HDFS yapılandırma bilgilerini almak için aşağıdaki komutu kullanın ve kullanarak filtre [jq](https://stedolan.github.io/jq/):

```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> Bu komut sunucuya uygulanan ilk yapılandırmasını döndürür (`service_config_version=1`), bu bilgiler içerir. En son bulmak için tüm yapılandırma sürümlerini listesinde gerekebilir.

Bu komutu aşağıdaki URI'ler benzer bir değer döndürür:

* `wasb://<container-name>@<account-name>.blob.core.windows.net` bir Azure depolama hesabı kullanıyorsanız.

    Hesap adı Azure depolama hesabının adıdır. Küme depolama köküdür blob kapsayıcısı kapsayıcı adıdır.

* `adl://home` Azure Data Lake Store kullanıyorsanız. Data Lake Store adını almak için aşağıdaki REST çağrısı kullanın:

    ```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    Bu komut aşağıdaki ana bilgisayar adını döndürür: `<data-lake-store-account-name>.azuredatalakestore.net`.

    Hdınsight için kök deposunda dizine almak için aşağıdaki REST çağrısı kullanın:

    ```curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    Bu komut bir yolu aşağıdaki yola benzer döndürür: `/clusters/<hdinsight-cluster-name>/`.

Ayrıca, aşağıdaki adımları kullanarak Azure Portalı'nı kullanarak depolama bilgi bulabilirsiniz:

1. İçinde [Azure portal](https://portal.azure.com/), Hdınsight kümenize seçin.

2. Gelen **özellikleri** bölümünde, select **depolama hesapları**. Küme için depolama bilgiler görüntülenir.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Dış Hdınsight'ta dosyaları nasıl erişirim

Hdınsight küme dışından verileri erişmek için çeşitli yolları vardır. Yardımcı programlar ve verilerinizle çalışmaya kullanılan SDK'ları birkaç bağlantılar verilmiştir:

Kullanıyorsanız __Azure Storage__, yolları verilerinize erişmek için aşağıdaki bağlantılara bakın:

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): komut satırı arabirimi komutları Azure ile çalışmak için. Yükledikten sonra kullanmak `az storage` depolama kullanma hakkında Yardım için komut veya `az storage blob` blob özgü komutlar için.
* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): bir python komut dosyası Azure depolama alanında BLOB'lar ile çalışmak için.
* Çeşitli SDK'lar:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Depolama REST API’si](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Kullanıyorsanız __Azure Data Lake Store__, yolları verilerinize erişmek için aşağıdaki bağlantılara bakın:

* [Web tarayıcısı](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI 2.0](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS REST API](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Visual Studio için Data Lake araçları](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>Kümenizi ölçeklendirme

Özellik ölçeklendirme kümesi, dinamik olarak küme tarafından kullanılan veri düğüm sayısını değiştirmenize olanak sağlar. Diğer işleri sırasında ölçeklendirme işlemleri gerçekleştirebilir veya işlemleri bir kümede çalışmıyor.

Farklı küme türü gibi ölçeklendirme tarafından etkilenir:

* **Hadoop**: bir kümedeki düğüm sayısını aşağı Ölçeklendirmesi, bazı kümedeki hizmetleri yeniden başlatılır. Operations ölçeklendirme işleri çalıştırma veya bekleyen ölçeklendirme işlemi tamamlandığında başarısız olmasına neden olabilir. İşlem tamamlandıktan sonra işleri yeniden gönderebilirsiniz.
* **HBase**: bölgesel sunucular otomatik olarak ölçeklendirme işlemi tamamlandıktan sonra birkaç dakika içinde dengeli. El ile bölgesel sunucular dengelemek için aşağıdaki adımları kullanın:

    1. SSH kullanarak Hdınsight kümesine bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

    2. HBase Kabuğu'nu başlatmak için aşağıdakileri kullanın:

            hbase shell

    3. HBase kabuğunu yüklendikten sonra el ile bölgesel sunucular dengelemek için aşağıdakileri kullanın:

            balancer

* **Storm**: bir ölçeklendirme işlemi gerçekleştirildikten sonra tüm çalışan Storm topolojileri yeniden dengelemeniz gerekir. Dengelenmesi yeni kümedeki düğüm sayısını temel paralellik ayarlarını yeniden ayarlama gereksinimini imkan tanır. Çalışan topolojileri yeniden dengelemeniz için aşağıdaki seçeneklerden birini kullanın:

    * **SSH**: sunucuya bağlanıp bir topoloji yeniden dengelemeniz için aşağıdaki komutu kullanın:

            storm rebalance TOPOLOGYNAME

        İlk olarak topolojisi tarafından sağlanan paralellik ipuçları geçersiz kılmak için parametreleri de belirtebilirsiniz. Örneğin, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` 5 çalışan işlemleri, mavi spout bileşeni için 3 yürütücüler ve sarı Cıvata bileşeni için 10 yürütücüler topolojiye yeniden yapılandırır.

    * **Storm kullanıcı Arabirimi**: Storm kullanıcı arabirimini kullanarak bir topoloji yeniden dengelemeniz için aşağıdaki adımları kullanın.

        1. Açık **https://CLUSTERNAME.azurehdinsight.net/stormui** web tarayıcısında, burada CLUSTERNAME Storm kümenizin adıdır. İstenirse, Hdınsight Küme Yöneticisi (Yönetici) adını ve küme oluştururken belirttiğiniz parolayı girin.
        2. Yeniden dengelemeniz ve ardından istediğiniz topolojiyi seçin **yeniden dengelemeniz** düğmesi. Yeniden dengeleyin işlem gerçekleştirilmeden önce gecikme girin.

* **Kafka**: işlemleri ölçeklendirme sonra çoğaltmalarını yeniden dengelemeniz gerekir. Daha fazla bilgi için bkz: [hdınsight'ta Kafka verilerle yüksek kullanılabilirliğini](./kafka/apache-kafka-high-availability.md) belge.

Hdınsight kümenize ölçeklendirme ile ilgili ayrıntılı bilgi için bkz:

* [Azure portalını kullanarak hdınsight'ta Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Azure PowerShell kullanarak hdınsight'ta Hadoop kümelerini yönetme](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hue (veya başka bir Hadoop bileşeni) nasıl yüklerim?

Hdınsight yönetilen bir hizmettir. Azure kümesi ile ilgili bir sorun algılarsa, başarısız olan düğüm silmek ve değiştirmek için bir düğüm oluşturabilirsiniz. Kümede şeyler el ile yüklerseniz, bu işlemi oluştuğunda bunlar kalıcı değildir. Bunun yerine, kullanın [Hdınsight betik eylemleri](hdinsight-hadoop-customize-cluster.md). Betik eylemi aşağıdaki değişiklikleri yapmak için kullanılabilir:

* Yükleyin ve bir hizmeti veya web sitesini yapılandırın.
* Yükleme ve yapılandırma değişiklikleri kümedeki birden çok düğümde gerektiren bir bileşen yapılandırın.

Betik eylemleri Bash betikleridir. Komut dosyaları, küme oluşturma sırasında çalıştırın ve yüklemek ve ek bileşenleri yapılandırmak için kullanılır. Aşağıdaki bileşenleri yüklemek için örnek komut verilmiştir:

* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Kendi Betik Eylemlerinizi geliştirme hakkında daha fazla bilgi için bkz. [HDInsight ile Betik Eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>Jar dosyaları

Bazı Hadoop teknolojiler bir MapReduce işi ya da bir parçası olarak kullanılan işlevler içeren müstakil jar dosyalarını sağlanan Pig veya Hive içinde. Bunlar genellikle yok herhangi bir ayar gerektirir ve Küme oluşturulduktan sonra karşıya ve değiştirebilirsiniz doğrudan kullanılır. Bileşen kümesini yeniden görüntüsünü oluşturuyor şekilde kalmıştır emin olmak istiyorsanız, kümeniz (WASB veya ADL) için varsayılan depolama alanında jar dosyasını depolayabilirsiniz.

Örneğin, en son sürümünü kullanmak isterseniz [DataFu](http://datafu.incubator.apache.org/), projeyi içeren jar indirin ve Hdınsight kümesine yükleyin. Pig veya Hive kullanma hakkında daha sonra DataFu belgelerine izleyin.

> [!IMPORTANT]
> Tek başına jar dosyalarını olan bazı bileşenleri Hdınsight ile sağlanır, ancak yolu değildir. Belirli bir bileşen için arıyorsanız, kümenizde aramak için izlemeyi kullanabilirsiniz:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Bu komut tüm eşleşen jar dosyalarının yolunu döndürür.

Bir bileşenin farklı bir sürümünü kullanmak için gereken ve işlerinizde kullanabilirsiniz sürüm karşıya yükleyin.

> [!WARNING]
> Hdınsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve yalıtmak ve bu bileşenleri ilgili sorunları gidermek için Microsoft Support yardımcı olur.
>
> Özel bileşenler, daha fazla sorun gidermenize yardımcı olması için ticari koşulların elverdiği oranda makul destek alırsınız. Bu sorunu çözmek veya bu teknoloji derin uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları devreye isteyen neden olabilir. Örneğin, olduğu gibi kullanılabilecek birçok topluluk siteleri vardır: [Hdınsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Sonraki adımlar

* [Windows tabanlı Hdınsight'ta Linux tabanlı geçirme](hdinsight-migrate-from-windows-to-linux.md)
* [HDInsight ile Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hadoop/hdinsight-use-mapreduce.md)
