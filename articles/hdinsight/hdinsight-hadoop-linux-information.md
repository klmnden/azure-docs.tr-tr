---
title: Linux tabanlı HDInsight üzerinde - Azure Hadoop kullanmaya yönelik ipuçları
description: Tanıdık bir Linux ortamı Azure bulutunda çalışan Linux tabanlı HDInsight (Hadoop) kümeleri kullanarak uygulama ipuçları alın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 2a7af59495966c76a47ea84311ab073eb594f82e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707561"
---
# <a name="information-about-using-hdinsight-on-linux"></a>Linux’ta HDInsight kullanma ile ilgili bilgiler

Azure HDInsight kümeleri Apache Hadoop Azure bulutunda çalışan tanıdık bir Linux ortamı sağlar. Çoğu işlemler için tam olarak diğer Linux üzerinde Hadoop yükleme çalışması gerekir. Bu belge farkında olmanız gereken belirli farklılıkları çağırır.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Önkoşullar

Bu belgedeki adımlarda birçoğu, sisteminizde yüklü olması gereken aşağıdaki yardımcı programlar, kullanın.

* [cURL](https://curl.haxx.se/) - web tabanlı hizmetler ile iletişim kurmak için kullanılır.
* **jq**, bir komut satırı JSON işlemcisine giden.  Bkz: [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/).
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) - uzaktan Azure hizmetlerini yönetmek için kullanılır.
* **Bir SSH istemcisi**. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="users"></a>Kullanıcılar

Sürece [etki alanına katılmış](./domain-joined/apache-domain-joined-introduction.md), HDInsight sayılacağı bir **tek kullanıcı** sistem. Tek bir SSH kullanıcı hesabı, yönetici düzeyinde izinlere sahip bir küme ile oluşturulur. Ek SSH hesaplarının oluşturulabilir ancak ayrıca küme yönetici erişimine sahiptirler.

Etki alanına katılmış HDInsight birden çok kullanıcı ve daha ayrıntılı izni ve rol ayarlarını destekler. Daha fazla bilgi için [yönetme etki alanına katılmış HDInsight kümeleri](./domain-joined/apache-domain-joined-manage.md).

## <a name="domain-names"></a>Etki alanı adları

İnternet'ten kümeye bağlanırken kullanması için tam etki alanı adı (FQDN) `CLUSTERNAME.azurehdinsight.net` veya `CLUSTERNAME-ssh.azurehdinsight.net` (için yalnızca SSH).

Dahili olarak, kümedeki her düğümün küme yapılandırması sırasında atanmış bir adı vardır. Küme adlarını bulmak için bkz: **konakları** Ambari Web kullanıcı arabirimini sayfasında. Ambari REST API'SİNDEN konaklar listesini döndürmek için aşağıdaki'ni de kullanabilirsiniz:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

`CLUSTERNAME` değerini kümenizin adıyla değiştirin. İstendiğinde, yönetici hesabı için parolayı girin. Bu komut, kümedeki konakların listesini içeren bir JSON belgesini döndürür. [jq](https://stedolan.github.io/jq/) çıkarmak için kullanılan `host_name` her konak için öğe değeri.

Düğümün adı için belirli bir hizmet bulmanız gerekiyorsa, söz konusu bileşen için Ambari sorgulayabilirsiniz. Örneğin, HDFS adı düğümünü konakları bulmak için aşağıdaki komutu kullanın:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Bu komut service açıklayan bir JSON belgesini döndürür ve ardından [jq](https://stedolan.github.io/jq/) yalnızca çeken `host_name` konaklar için değer.

## <a name="remote-access-to-services"></a>Uzaktan Erişim Hizmetleri

* **Ambari (web)** - https://CLUSTERNAME.azurehdinsight.net

    Küme Yöneticisi kullanıcı adı ve parola kullanarak kimlik doğrulaması ve Ambari için oturum açın.

    Düz metin kimlik doğrulama - bağlantının güvenli olduğundan emin olmak için her zaman HTTPS kullanın.

    > [!IMPORTANT]  
    > Bazı web kullanıcı arabirimleri Ambari kullanılabilir iç etki alanı adını kullanarak düğümlerin içerik modelini erişin. İç etki alanı adları internet üzerinden genel olarak erişilebilir değildir. Bazı özellikler Internet üzerinden erişmeye çalışırken "Sunucu bulunamadı" hatası alabilirsiniz.
    >
    > Ambari web kullanıcı Arabirimi tam işlevselliğini kullanmak için bir SSH tüneli küme baş düğümü için proxy web trafiği için kullanın. Bkz: [kullanım Apache Ambari web kullanıcı Arabirimi, ResourceManager, JobHistory, NameNode, Oozie ve diğer web kullanıcı arabirimlerine erişim için SSH tünel](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://CLUSTERNAME.azurehdinsight.net/ambari

    > [!NOTE]  
    > Küme Yöneticisi kullanıcı adı ve parola kullanarak kimlik doğrulaması.
    >
    > Düz metin kimlik doğrulama - bağlantının güvenli olduğundan emin olmak için her zaman HTTPS kullanın.

* **WebHCat (templeton olarak da)** - https://CLUSTERNAME.azurehdinsight.net/templeton

    > [!NOTE]  
    > Küme Yöneticisi kullanıcı adı ve parola kullanarak kimlik doğrulaması.
    >
    > Düz metin kimlik doğrulama - bağlantının güvenli olduğundan emin olmak için her zaman HTTPS kullanın.

* **SSH** -CLUSTERNAME-ssh.azurehdinsight.net bağlantı noktası 22 veya 23. Bağlantı noktası 22 ikincil veritabanına bağlanmak için 23 kullanılırken birincil baş düğüme bağlanmak için kullanılır. Baş düğümler hakkında daha fazla bilgi için bkz. [içinde HDInsight kümelerinin kullanılabilirliği ve güvenilirliği Apache hadoop'un](hdinsight-high-availability-linux.md).

    > [!NOTE]  
    > Yalnızca bir istemci makinesinden SSH küme baş düğümleri erişebilir. Bağlantı kurulduktan sonra bir baş düğümüne SSH kullanarak çalışan düğümlerinin ardından erişebilirsiniz.

Daha fazla bilgi için [HDInsight üzerinde Apache Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](hdinsight-hadoop-port-settings-for-services.md) belge.

## <a name="file-locations"></a>Dosya konumları

Hadoop ile ilgili dosyaları küme düğümlerinde bulunabilir `/usr/hdp`. Bu dizin, aşağıdaki alt dizinleri içerir:

* **2.6.5.3006-29**: Dizin adı Hortonworks Data Platform HDInsight tarafından kullanılan sürümüdür. Kümenizi sayısına, burada listelenen farklı olabilir.
* **Geçerli**: Bu dizin alt dizinler bağlantılar içeren **2.6.5.3006-29** dizin. Bu dizinin var olduğundan, sürüm numarasını hatırlamak zorunda kalmazsınız.

Hadoop dağıtılmış dosya sistemi üzerinde örnek verileri ve JAR dosyaları bulunabilir `/example` ve `/HdiSamples`.

## <a name="hdfs-azure-storage-and-data-lake-storage"></a>HDFS, Azure depolama ve Data Lake depolama

Çoğu Hadoop dağıtımları, kümedeki makinelerde yerel depolama tarafından yedeklenen HDFS, veriler depolanır. Yerel depolama ile burada saat veya dakika işlem kaynakları için ücretlendirilirsiniz bulut tabanlı bir çözüm için pahalı olabilir.

HDInsight'ı kullanırken, veri dosyalarını Azure Blob Depolama ve isteğe bağlı olarak Azure Data Lake Storage kullanarak bulutta dayanıklı ve ölçeklenebilir bir şekilde depolanır. Bu hizmetler, aşağıdaki avantajları sağlar:

* Uzun vadeli depolama ucuz.
* Web siteleri, dosya karşıya yükleme/indirme yardımcı programlar, çeşitli dil SDK'ları ve web tarayıcıları gibi dış hizmetlerden erişilebilirlik.
* Büyük dosya kapasite ve büyük ölçeklenebilir depolama.

Daha fazla bilgi için [anlama blobları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) ve [Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/).

Azure depolama ya da Data Lake Storage kullanırken, verilere erişmek için HDInsight özel bir şey yapmanız gerekmez. Örneğin, aşağıdaki komut dosyalarında listeler `/example/data` olup Azure Depolama'da veya Data Lake Store üzerinde depolandığından bağımsız olarak klasörü:

    hdfs dfs -ls /example/data

HDInsight veri depolama kaynaklarını (Azure Blob Depolama ve Azure Data Lake depolama), bilgi işlem kaynaklarından birbirinden ayrılmıştır. Bu nedenle, gereksinim ve iş tamamlandığında, daha sonra kümeyi silmek hesaplama yapmak için HDInsight kümeleri oluşturabilir, ihtiyacınız olduğu sürece bu arada, veri dosyalarını tutmak güvenli bir şekilde bulut depolamada kalıcı olan.


### <a name="URI-and-scheme"></a>URI ve düzeni

Bazı komutlar düzeni URI'ın bir parçası olarak bir dosyaya erişirken belirlemenizi gerektirebilir. Örneğin, Storm-HDFS bileşen düzeni belirtmenizi gerektirir. Varsayılan olmayan depolama ("ek" depolama alanı olarak kümeye eklenen depolama) kullanırken, URI'ın bir parçası olarak her zaman şeması kullanması gerekir.

Kullanırken __Azure depolama__, aşağıdaki URI düzenleri birini kullanın:

* `wasb:///`: Şifrelenmemiş iletişimin kullanarak erişim varsayılan depolama alanı.

* `wasbs:///`: Şifreli iletişim kullanarak erişim varsayılan depolama alanı.  Wasbs şeması yalnızca HDInsight sürümü 3.6 veya sonraki sürümleri ' desteklenir.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Varsayılan olmayan depolama hesabıyla iletişimde kullanılacak. Örneğin, ek bir depolama hesabı veya ne zaman ortak olarak erişilebilen bir depolama hesabında depolanan verilere erişme olduğunda.

Kullanırken __Azure Data Lake depolama Gen2__, aşağıdaki URI düzenleri birini kullanın:

* `abfs:///`: Şifrelenmemiş iletişimin kullanarak erişim varsayılan depolama alanı.

* `abfss:///`: Şifreli iletişim kullanarak erişim varsayılan depolama alanı.  Abfss şeması yalnızca HDInsight sürümü 3.6 veya sonraki sürümleri ' desteklenir.

* `abfs://<container-name>@<account-name>.dfs.core.windows.net/`: Varsayılan olmayan depolama hesabıyla iletişimde kullanılacak. Örneğin, ek bir depolama hesabı veya ne zaman ortak olarak erişilebilen bir depolama hesabında depolanan verilere erişme olduğunda.

Kullanırken __Azure Data Lake depolama Gen1__, aşağıdaki URI düzenleri birini kullanın:

* `adl:///`: ' % S'varsayılan Data Lake depolama kümesi için erişim.

* `adl://<storage-name>.azuredatalakestore.net/`: Varsayılan olmayan Data Lake depolama ile iletişim kurarken kullanılan. Ayrıca, HDInsight kümenizin kök dizininin dışındaki verilere erişmek için kullanılır.

> [!IMPORTANT]  
> HDInsight için varsayılan depolama olarak Data Lake Storage'ı kullanırken, HDInsight depolama kökü olarak kullanılacak bir depo içindeki bir yol belirtmeniz gerekir. Varsayılan yol `/clusters/<cluster-name>/`.
>
> Kullanırken `/` veya `adl:///` verilere erişmek için yalnızca kök dizininde depolanan verilere erişebilirsiniz (örneğin, `/clusters/<cluster-name>/`) kümesi. Deponun herhangi bir yerindeki verilere erişmek için kullandığı `adl://<storage-name>.azuredatalakestore.net/` biçimi.

### <a name="what-storage-is-the-cluster-using"></a>Hangi depolama kümesi kullanıyor mu

Ambari, kümenin varsayılan depolama yapılandırmasını almak için kullanabilirsiniz. Curl kullanarak HDFS yapılandırma bilgilerini almak için aşağıdaki komutu kullanın ve kullanarak filtre [jq](https://stedolan.github.io/jq/):

```bash
curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

> [!NOTE]  
> Bu komut sunucuya uygulanan ilk yapılandırmayı döndürür (`service_config_version=1`), bu bilgileri içerir. En son olanı bulmak için tüm yapılandırma sürümlerini listesinde gerekebilir.

Bu komut, aşağıdaki URI benzer bir değer döndürür:

* `wasb://<container-name>@<account-name>.blob.core.windows.net` bir Azure depolama hesabı kullanıyorsanız.

    Hesap adı, Azure depolama hesabının adıdır. Kapsayıcı adı kökü olan küme depolama blob kapsayıcısıdır.

* `adl://home` Azure Data Lake Storage kullanılıyorsa. Data Lake Storage adını almak için aşağıdaki REST çağrısı kullanın:

     ```bash
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    Bu komut aşağıdaki ana bilgisayar adı döndürür: `<data-lake-store-account-name>.azuredatalakestore.net`.

    HDInsight için kök deposunda dizine almak için aşağıdaki REST çağrısı kullanın:

    ```bash
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    Bu komut aşağıdaki yola benzer bir yol döndürür: `/clusters/<hdinsight-cluster-name>/`.

Ayrıca, Azure portalında aşağıdaki adımları kullanarak depolama bilgileri bulabilirsiniz:

1. Gelen [Azure portalında](https://portal.azure.com/), HDInsight kümenizi seçin.

2. Gelen **özellikleri** bölümünden **depolama hesapları**. Küme için Depolama bilgileri görüntülenir.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Dosyaları dış HDInsight nasıl erişebilirim

HDInsight küme dışından verileri erişmek için bir çeşitli yolları vardır. Yardımcı programları ve verilerinizle çalışmaya kullanılabilir SDK'lar bazı bağlantılar aşağıda verilmiştir:

Kullanıyorsanız __Azure depolama__, verilerinize erişebilirsiniz yolları için aşağıdaki bağlantılara bakın:

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-az-cli2): Azure ile çalışmaya yönelik komut satırı arabirimi komutları. Yükledikten sonra `az storage` depolama kullanma hakkında Yardım için komut veya `az storage blob` blob özgü komutlar için.
* [blobxfer.py](https://github.com/Azure/blobxfer): Azure Depolama'daki blobları ile çalışmak için python betiği.
* Çeşitli SDK'lar:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Depolama REST API’si](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Kullanıyorsanız __Azure Data Lake Storage__, verilerinize erişebilirsiniz yolları için aşağıdaki bağlantılara bakın:

* [Web tarayıcısı](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS REST API](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Visual Studio için Data Lake araçları](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>Kümenizi ölçeklendirme

Küme ölçeklendirme özelliği, küme tarafından kullanılan veri düğümü sayısını dinamik olarak değiştirmenize olanak sağlar. Ölçeklendirme işlemleri sırasında diğer işleri gerçekleştirebilir veya bir kümede çalışan işlemlerin.  Ayrıca bkz [ölçek HDInsight kümeleri](./hdinsight-scaling-best-practices.md)

Farklı küme türü, şu şekilde ölçeklendirerek etkilenir:

* **Hadoop**: Bir kümedeki düğüm sayısını aşağı ölçeklendirme, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Ölçeklendirme işlemlerinin işleri çalıştırma veya bekleyen ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olabilir. İşlem tamamlandığında, işleri yeniden gönderebilirsiniz.
* **HBase**: Bölge sunucuları ölçeklendirme işlemi tamamlandıktan sonra birkaç dakika içinde otomatik olarak dengelenir. El ile bölgesel sunucular dengelemek için aşağıdaki adımları kullanın:

    1. HDInsight kümesine SSH kullanarak bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

    2. HBase Kabuğu'nu başlatmak için aşağıdakileri kullanın:

            hbase shell

    3. HBase kabuğunu yüklendikten sonra el ile bölgesel sunucuları dengelemek için aşağıdakileri kullanın:

            balancer

* **Storm**: Bir ölçeklendirme işlemi gerçekleştirildikten sonra tüm çalışan Storm topolojilerini yeniden dengelemeniz gerekir. Yeniden Dengeleme paralellik ayarları yeni kümedeki düğümlere göre yeniden ayarlayın imkan tanır. Çalışan topolojileri yeniden dengelemeniz için aşağıdaki seçeneklerden birini kullanın:

    * **SSH**: Sunucuya bağlanın ve bir topolojiyi yeniden dengelemek için aşağıdaki komutu kullanın:

            storm rebalance TOPOLOGYNAME

        Başlangıçta topoloji tarafından sağlanan paralellik ipuçları geçersiz kılmak için parametreleri de belirtebilirsiniz. Örneğin, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` 5 çalışan işlemleri ve 3 yürütücüler mavi spout bileşeni için 10 yürütücüler sarı bolt bileşeni için topolojiye yeniden yapılandırır.

    * **Storm kullanıcı Arabirimi**: Storm kullanıcı arabirimini kullanarak bir topolojiyi yeniden dengelemek için aşağıdaki adımları kullanın.

        1. Açık `https://CLUSTERNAME.azurehdinsight.net/stormui` web tarayıcınızda burada `CLUSTERNAME` Storm kümenizin adıdır. İstenirse, HDInsight Küme Yöneticisi (Yönetici) adı ve kümeyi oluştururken belirttiğiniz parolayı girin.
        2. Topolojiyi yeniden dengelemek ve ardından istediğiniz seçin **yeniden dengelemek** düğmesi. Yeniden Dengeleme işleminin gerçekleştirilmeden önce gecikme süresini girin.

* **Kafka**: Bölüm çoğaltmalarını ölçeklendirme işlemlerinden sonra yeniden dengelemeniz gerekir. Daha fazla bilgi için [HDInsight üzerinde Apache Kafka ile verilerinizin yüksek kullanılabilirliği](./kafka/apache-kafka-high-availability.md) belge.

HDInsight kümenizi ölçeklendirme ile ilgili ayrıntılı bilgi için bkz:

* [Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Azure PowerShell kullanarak HDInsight Apache Hadoop kümelerini yönetme](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hue (veya diğer Hadoop bileşenleri) nasıl yüklerim?

HDInsight yönetilen bir hizmettir. Azure küme ile ilgili bir sorun algılarsa, başarısız olan düğümü sil ve değiştirmek için bir düğüm oluşturmak. Kümede şeyler el ile yüklerseniz, bu işlemi meydana geldiğinde, kalıcı değildir. Bunun yerine, [HDInsight betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md). Betik eylemi, aşağıdaki değişiklikleri yapmak için kullanılabilir:

* Yükleyin ve bir hizmeti veya web sitesini yapılandırın.
* Yükleme ve yapılandırma değişiklikleri kümedeki birden çok düğümde gerektiren bir bileşen yapılandırın.

Betik eylemleri, Bash betikleridir. Komut dosyaları, küme oluşturma sırasında çalıştırmak ve ek bileşenlerini yükleme ve yapılandırma için kullanılır. Örnek betikler aşağıdaki bileşenleri yüklemek için sağlanmıştır:

* [Apache giraph'ı](hdinsight-hadoop-giraph-install-linux.md)

Kendi Betik Eylemlerinizi geliştirme hakkında daha fazla bilgi için bkz. [HDInsight ile Betik Eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>Jar dosyaları

Bazı Hadoop teknolojileri öğesinden veya bir MapReduce işi, bir parçası olarak kullanılan işlevler içeren kendi başına bir jar dosyaları sağlanan Pig veya Hive içinde. Bunlar genellikle olmayan herhangi bir ayar gerektiren Küme oluşturulduktan sonra karşıya ve doğrudan kullanılır. Bileşen kümesini yeniden devam eder. emin olmak, jar dosyasını kümeniz (WASB veya ADL) için varsayılan depolama alanında depolayabilirsiniz.

Örneğin, en son sürümünü kullanmak istiyorsanız [Apache DataFu](https://datafu.incubator.apache.org/), projeyi içeren bir jar dosyasını indirin ve HDInsight kümesine yükleyin. Ardından Hive veya Pig kullanma hakkında DataFu belgeleri izleyin.

> [!IMPORTANT]  
> Tek başına jar dosyalarıdır bazı bileşenler, HDInsight ile sağlanan ancak yolu değildir. Belirli bir bileşeni için arıyorsanız, kümenizde aramak için izleme kullanabilirsiniz:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Bu komut tüm eşleşen jar dosyalarının yolunu döndürür.

Bir bileşen farklı bir sürümünü kullanmanız gerekir ve işlerinizde kullanabilirsiniz sürüm yükleyin.

> [!IMPORTANT]
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Bu sorunu çözümlemek ya da bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak isteyen neden olabilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Apache projeleri proje siteleri de [ https://apache.org ](https://apache.org), örneğin: [Hadoop](https://hadoop.apache.org/), [Spark](https://spark.apache.org/).

## <a name="next-steps"></a>Sonraki adımlar

* [Linux tabanlı Windows tabanlı HDInsight ' geçiş](hdinsight-migrate-from-windows-to-linux.md)
* [Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönetme](./hdinsight-hadoop-manage-ambari-rest-api.md)
* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hadoop/hdinsight-use-mapreduce.md)
