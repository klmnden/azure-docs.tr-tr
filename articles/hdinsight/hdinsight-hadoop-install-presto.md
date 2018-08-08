---
title: Azure HDInsight Linux kümeleri üzerinde presto yükleme
description: Betik eylemlerini kullanarak Linux tabanlı HDInsight Hadoop kümelerinde presto ve Airpal yüklemeyi öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: ea777b13348b84aaeb7cb7628a4d0aac9f5705bd
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591430"
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Yükleme ve Presto HDInsight Hadoop kümelerini kullanma

Bu belgede, Script Action kullanarak HDInsight Hadoop kümelerini Presto yükleme konusunda bilgi edinin. Ayrıca var olan bir Presto HDInsight kümesinde Airpal yüklemeyi öğrenin.

> [!IMPORTANT]
> Bu belgedeki adımlarda gerektiren bir **HDInsight 3.5 Hadoop kümesi** , Linux kullanır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight sürümleri](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Presto nedir?
[Presto](https://prestodb.io/overview.html) bir hızlı dağıtılmış SQL sorgu alt için büyük veri yapısıdır. Presto, petabaytlarca verinin etkileşimli sorgulama için uygundur. Presto ve nasıl çalıştıkları bileşenler hakkında daha fazla bilgi için bkz. [Presto kavramları](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
> 
> Özel bileşenler, Presto gibi daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Bu sorunu çözümlemek ya da bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak isteyen neden olabilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Betik eylemi kullanarak Presto yükleme

Bu bölümde, örnek betik Azure portalını kullanarak yeni bir küme oluştururken kullanmak hakkında yönergeler açıklanmaktadır. 

1. Adımları kullanarak bir kümeyi sağlamaya başlayın [sağlama Linux tabanlı HDInsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md). Küme kullanarak oluşturduğunuz emin **özel** küme oluşturma akış. Küme, aşağıdaki gereksinimleri karşılaması gerekir.

    * HDInsight sürümü 3.6 ile bir Hadoop kümesini olmalıdır.

    * Azure depolama veri deposu olarak kullanmanız gerekir. Azure Data Lake Store depolama seçeneği olarak kullanan bir kümede presto kullanarak henüz desteklenmiyor. 

    ![Özel seçenekleri kullanarak HDInsight küme oluşturma](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Üzerinde **Gelişmiş ayarlar** alanında **betik eylemleri**ve aşağıdaki bilgileri sağlayın:
   
   * **AD**: betik eylemi için bir kolay ad girin.
   * **Bash betiği URI’si**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Bu seçeneği işaretleyin
   * **ÇALIŞAN**: Bu seçeneği işaretleyin
   * **ZOOKEEPER**: Bu onay kutusunu temizleyin
   * **PARAMETRELERİ**: Bu alanı boş bırakın


3. Sayfanın alt kısmında **betik eylemleri** alanı tıklayın **seçin** yapılandırmayı kaydetmek için düğme. Son olarak, tıklayın **seçin** düğme alttaki **Gelişmiş ayarlar** yapılandırmasını kaydetmek üzere alan.

4. Açıklandığı gibi küme sağlama devam [sağlama Linux tabanlı HDInsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Azure PowerShell, Azure CLI, HDInsight .NET SDK veya Azure Resource Manager şablonları betik eylemleri uygulamak için de kullanılabilir. Betik eylemleri zaten kümelerini çalıştırmak için de uygulayabilirsiniz. Daha fazla bilgi için [özelleştirme HDInsight kümeleri ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Presto HDInsight ile kullanma

Presto bir HDInsight kümesinde çalışmak için aşağıdaki adımları kullanın:

1. SSH kullanarak HDInsight kümesine bağlanma:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Aşağıdaki komutu kullanarak Presto Kabuğu'nu başlatın.
   
        presto --schema default

3. Bir örnek tablo üzerinde bir sorgu çalıştırdığınızda **hivesampletable**, varsayılan olarak tüm HDInsight kümelerinde kullanılabilen.
   
        select count (*) from hivesampletable;
   
    Varsayılan olarak, [Hive](https://prestodb.io/docs/current/connector/hive.html) ve [TPCH](https://prestodb.io/docs/current/connector/tpch.html) bağlayıcılar Presto için zaten yapılandırılmış. Hive bağlayıcı Hive tüm tabloları, Presto otomatik olarak görünür olacak varsayılan olarak yüklenen Hive yüklemeyi kullanmak üzere yapılandırılmış.

    Daha fazla bilgi için [Presto belgeleri](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Presto ile Airpal kullanın

[Airpal](https://github.com/airbnb/airpal#airpal) Presto için açık kaynak web tabanlı sorgu arabirimidir. Airpal hakkında daha fazla bilgi için bkz. [Airpal belgeleri](https://github.com/airbnb/airpal#airpal).

Kenar düğümüne Airpal yüklemek için aşağıdaki adımları kullanın:

1. SSH kullanarak, Presto yüklü olduğu HDInsight küme baş düğümüne bağlanın:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Bağlandıktan sonra aşağıdaki komutu çalıştırın.

        sudo slider registry  --name presto1 --getexp presto 
   
    Aşağıdaki JSON ile benzer bir çıktı görürsünüz:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Çıktıdaki değerini not edin **değer** özelliği. Küme edgenode üzerinde Airpal yüklerken bu değeri gerekir. Yukarıdaki çıktıda ihtiyacınız olacak değerdir **10.0.0.12:9090**.

4. Şablonu **[burada](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** bir HDInsight kümesi edgenode oluşturun ve aşağıdaki ekran görüntüsünde gösterilen değerleri sağlayın.

    ![Presto kümede HDInsight yükleme Airpal](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. **Satın al**’a tıklayın.

6. Küme yapılandırması için değişiklikler uygulandıktan sonra aşağıdaki adımları kullanarak Airpal web arabirimi erişebilirsiniz.

    1. Küme iletişim kutusundan tıklayın **uygulamaları**.

        ![Presto kümede HDInsight başlatma Airpal](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    2. Gelen **yüklü uygulamaların** alanı tıklayın **portalı** airpal karşı.

        ![Presto kümede HDInsight başlatma Airpal](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    3. İstendiğinde, HDInsight Hadoop kümesi oluştururken belirttiğiniz yönetici kimlik bilgilerini girin.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>HDInsight kümesi üzerinde bir Presto yükleme özelleştirme

Yüklemeyi özelleştirmek için aşağıdaki adımları kullanın:

1. SSH kullanarak, Presto yüklü olduğu HDInsight küme baş düğümüne bağlanın:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Yapılandırma değişiklik dosyasında `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Presto yapılandırma hakkında daha fazla bilgi için bkz. [YARN tabanlı kümeler için Presto Yapılandırması](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Durdur ve Presto geçerli çalışan örneği sonlandırılır.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Presto yeni bir örneğini özelleştirme ile başlayın.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Presto Düzenleyicisi adresini not alın ve hazır yeni örneğin tamamlanmasını bekleyin.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Presto çalıştıran HDInsight kümeleri için Kıyaslama verileri üretme

TPC-DS birçok karar destek sistemleri, büyük veri sistemlerini dahil performansını ölçmek için endüstri standardıdır. Presto, verileri oluşturmak ve bunu kendi HDInsight Kıyaslama verilerle nasıl karşılaştırır değerlendirmek için kullanabilirsiniz. Daha fazla bilgi için [burada](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Ayrıca bkz.
* [Yükleme ve HDInsight kümelerinde Hue kullanma](hdinsight-hadoop-hue-linux.md). Hue web kullanıcı Arabirimi oluşturmak Çalıştır kolaylaştırır ve Pig ve Hive işlerini ' dir.

* [HDInsight kümelerinde Giraph yükleme](hdinsight-hadoop-giraph-install-linux.md). Küme özelleştirmesi, HDInsight Hadoop kümelerinde Giraph'ı yüklemek için kullanın. Giraph grafik Hadoop kullanarak işleme yapmanıza olanak tanır ve Azure HDInsight ile kullanılabilir.

* [HDInsight kümelerinde Solr yükleme](hdinsight-hadoop-solr-install-linux.md). Küme özelleştirmesi, HDInsight Hadoop kümelerinde Solr'ı yüklemek için kullanın. Solr depolanan veriler üzerinde güçlü arama işlemleri yapmanıza olanak tanır.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
