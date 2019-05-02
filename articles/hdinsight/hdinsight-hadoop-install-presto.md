---
title: Azure HDInsight Linux kümeleri üzerinde presto yükleme
description: Betik eylemlerini kullanarak Linux tabanlı HDInsight Hadoop kümelerinde presto ve Airpal yüklemeyi öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: hrasheed
ms.openlocfilehash: 2bd5e1ae02ffbb62b9a5a95846aabeeab2b448b5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704806"
---
# <a name="install-and-use-presto-on-hadoop-based-hdinsight-clusters"></a>Yükleme ve Hadoop tabanlı HDInsight kümelerinde Presto kullanma

Bu makalede, betik eylemlerini kullanarak Azure HDInsight Hadoop tabanlı kümelerde Presto yükleme açıklanmaktadır. Ayrıca var olan bir Presto HDInsight kümesinde Airpal yüklemeyi öğrenin.

HDInsight için Apache Hadoop kümelerini Presto Yıldız Yağmuru uygulama de sunar. Daha fazla bilgi için [Azure HDInsight üzerinde üçüncü taraf Apache Hadoop uygulamaları yükleme](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-install-applications).

> [!IMPORTANT]  
> Bu makaledeki adımlarda Linux kullanan bir HDInsight 3.5 Hadoop kümesi gerektirir. Linux üzerinde HDInsight sürüm 3.4 veya üzeri kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight sürümleri](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Presto nedir?
[Presto](https://prestosql.io) bir hızlı dağıtılmış SQL sorgu alt için büyük veri yapısıdır. Presto, petabaytlarca verinin etkileşimli sorgulama için uygundur. Presto ve nasıl çalıştıkları bileşenler hakkında daha fazla bilgi için bkz. [Presto kavramları](https://prestosql.io/docs/current/overview/concepts.html).

> [!WARNING]  
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir. Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
> 
> Özel bileşenler Presto gibi daha fazla sorun gidermenize yardımcı olması için ticari açıdan makul destek alırsınız. Bu destek, sorunu çözebilir. Veya bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak amacıyla istenebilir. Kullanılabilecek birçok topluluk siteleri vardır. Örnekler [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) ve [Stack Overflow](https://stackoverflow.com). 
>
> Apache projeleri de proje siteleri [Apache Web sitesi](https://apache.org). Bir örnek [Hadoop](https://hadoop.apache.org/).


## <a name="install-presto-by-using-script-actions"></a>Betik eylemlerini kullanarak presto yükleme

Bu bölümde, Azure portalını kullanarak yeni bir küme oluşturduğunuzda örnek komut dosyası kullanmayı açıklar: 

1. Adımları alarak küme sağlama başlamak [Azure portalını kullanarak HDInsight kümelerini oluşturmak Linux tabanlı](hdinsight-hadoop-create-linux-clusters-portal.md). Kullanarak küme oluşturduğunuzdan emin olun **özel** küme oluşturma akış. Küme, aşağıdaki gereksinimleri karşılaması gerekir:

   * HDInsight sürümü 3.6 ile bir Hadoop kümesini olmalıdır.

   * Azure depolama veri deposu olarak kullanmanız gerekir. Presto depolama seçeneği olarak Azure Data Lake depolama kullanan bir kümesi kullanarak, henüz bir seçenek değildir.

     ![HDInsight, özel (boyut, ayarları, uygulamalar)](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. İçinde **Gelişmiş ayarlar** alanında **betik eylemleri**. Aşağıdaki bilgileri sağlayın. Ayrıca seçebilirsiniz **Presto yükleme** seçeneği için betik türü:
   
   * **AD**. Betik eylemi için bir kolay ad girin.
   * **Bash betiği URI'si**. `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`.
   * **HEAD**. Bu seçeneği belirleyin.
   * **ÇALIŞAN**. Bu seçeneği belirleyin.
   * **ZOOKEEPER**. Bu onay kutusunu boş bırakın.
   * **PARAMETRELERİ**. Bu alanı boş bırakın.


3. Sayfanın alt kısmında **betik eylemleri** alanında seçin **seçin** yapılandırmayı kaydetmek için düğme. Son olarak, seçin **seçin** düğme alttaki **Gelişmiş ayarlar** yapılandırmasını kaydetmek üzere alan.

4. Bölümünde anlatıldığı gibi kümesi sağlamak devam [Azure portalını kullanarak HDInsight kümelerini oluşturmak Linux tabanlı](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]  
    > Azure PowerShell, Azure Klasik CLI, HDInsight .NET SDK veya Azure Resource Manager şablonları betik eylemleri uygulamak için de kullanılabilir. Betik eylemleri zaten kümelerini çalıştırmak için de uygulayabilirsiniz. Daha fazla bilgi için [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemlerini kullanarak](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Presto HDInsight ile kullanma

Presto bir HDInsight kümesinde çalışmak için aşağıdaki adımları uygulayın:

1. SSH kullanarak HDInsight kümesine bağlanma:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Aşağıdaki komutu çalıştırarak Presto Kabuğu'nu başlatın:
   
    `presto --schema default`

3. Bir örnek tablo üzerinde bir sorgu çalıştırdığınızda **hivesampletable**, varsayılan olarak tüm HDInsight kümelerinde kullanılabilen:
   
    `select count (*) from hivesampletable;`
   
    Varsayılan olarak, [Apache Hive](https://prestosql.io/docs/current/connector/hive.html) ve [TPCH](https://prestosql.io/docs/current/connector/tpch.html) bağlayıcılar Presto için zaten yapılandırılmış. Hive Bağlayıcısı'nı varsayılan Hive yükleme kullanmak için yapılandırılır. Hive tüm tablolardan otomatik olarak Presto içinde görülebilir.

    Daha fazla bilgi için [Presto belgeleri](https://prestosql.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Presto ile Airpal kullanın

[Airpal](https://github.com/airbnb/airpal#airpal) Presto için açık kaynak web tabanlı sorgu arabirimidir. Airpal hakkında daha fazla bilgi için bkz. [Airpal belgeleri](https://github.com/airbnb/airpal#airpal).

Kenar düğümüne Airpal yüklemek için aşağıdaki adımları uygulayın:

1. SSH kullanarak, Presto yüklü olduğu HDInsight küme baş düğümüne bağlanın:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Bağlandıktan sonra aşağıdaki komutu çalıştırın:

    `sudo slider registry  --name presto1 --getexp presto` 
   
    Aşağıdaki JSON ile benzer bir çıktı görürsünüz:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Çıktıdaki değerini not edin **değer** özelliği. Küme kenar düğümüne Airpal yüklerken bu değeri gerekir. Yukarıdaki çıktıda gereken değerdir **10.0.0.12:9090**.

4. Kullanım [Bu şablon](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json) bir HDInsight küme kenar düğümüne oluşturmak için. Aşağıdaki ekran görüntüsünde gösterilen değerleri sağlayın.

    ![Özel dağıtım](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. **Satın al**'ı seçin.

6. Küme yapılandırması için değişiklikler uygulandıktan sonra aşağıdaki alarak web arabirimi adımları Airpal erişim [Azure portalında](https://portal.azure.com):

    1. Sol menüden **tüm hizmetleri**.

    1. Altında **ANALYTICS**seçin **HDInsight kümeleri**.

    1. Kümenizi varsayılan görünüm açılır listeden seçin.

    1. Varsayılan görünümünden altında **ayarları**seçin **uygulamaları**.

        ![HDInsight, uygulamalar](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    1. Gelen **yüklü uygulamaların** sayfasında, tablo girişini bulun **airpal**. Seçin **portalı**.

        ![Yüklü uygulamalar](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    1. İstendiğinde, Hadoop tabanlı bir HDInsight kümesi oluştururken belirttiğiniz yönetici kimlik bilgilerini girin.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>HDInsight kümesi üzerinde bir Presto yükleme özelleştirme

Yüklemeyi özelleştirmek için aşağıdaki adımları uygulayın:

1. SSH kullanarak, Presto yüklü olduğu HDInsight küme baş düğümüne bağlanın:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Yapılandırma değişiklik dosyasında `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Presto yapılandırma hakkında daha fazla bilgi için bkz. [YARN tabanlı kümeler için Presto yapılandırma seçenekleri](https://prestosql.github.io/presto-yarn/installation-yarn-configuration-options.html).

3. Durdur ve Presto geçerli çalışan örneğini Kaldır:

    `sudo slider stop presto1 --force` `sudo slider destroy presto1 --force`

4. Presto yeni bir örneğini özelleştirme ile başlayın:

    `sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json`

5. Yeni bir örneğinin hazır olmasını bekleyin. Presto Düzenleyicisi adresini not alın:

    `sudo slider registry --name presto1 --getexp presto`

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Presto çalıştıran HDInsight kümeleri için Kıyaslama verileri üretme

TPC-DS büyük veri sistemlerini dahil olmak üzere birçok karar destek sistemleri performansını ölçmek için endüstri standardıdır. Presto, verileri oluşturmak ve bunu kendi HDInsight Kıyaslama verilerle nasıl karşılaştırır değerlendirmek için kullanabilirsiniz. Daha fazla bilgi için [tpcds hdınsight](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="next-steps"></a>Sonraki adımlar
* [Yükleme ve HDInsight Hadoop kümeler üzerinde Hue kullanma](hdinsight-hadoop-hue-linux.md). Hue, oluşturma, çalıştırma ve Apache Pig ve Hive işlerini kaydetmek kolay kullanıcı Arabirimi bir web tarayıcısı uygulamasıdır.

* [HDInsight Hadoop kümeleri üzerinde Apache giraph'ı yükleyin ve büyük ölçekli grafikleri işlemek için Giraph kullanma](hdinsight-hadoop-giraph-install-linux.md). Küme özelleştirmesi, Hadoop tabanlı HDInsight kümelerinde Giraph'ı yüklemek için kullanın. Giraph ile grafik işlemeyi Hadoop kullanarak gerçekleştirebilirsiniz. Azure HDInsight ile de kullanılabilir.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
