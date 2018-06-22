---
title: 'Hızlı Başlangıç: Şablon kullanarak HDInsight’ta Spark kümesi oluşturma'
description: Bu hızlı başlangıçta, Kaynak Yöneticisi şablonu kullanılarak nasıl Azure HDInsight’ta Apache Spark kümesi oluşturulacağı ve basit bir Spark SQL sorgusu çalıştırılacağı gösterilmektedir.
services: azure-hdinsight
author: mumian
manager: cgronlun
editor: cgronlun
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.devlang: na
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: jgao
ms.custom: mvc
ms.openlocfilehash: 06d711c99a6aaffe85adf740d2041c9fcc35ac23
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628105"
---
# <a name="quickstart-create-a-spark-cluster-in-hdinsight-using-template"></a>Hızlı Başlangıç: Şablon kullanarak HDInsight’ta Spark kümesi oluşturma

Azure HDInsight’ta Apache Spark kümesinin nasıl oluşturulacağını ve Spark SQL sorgularının Hive tablolarına karşı nasıl çalıştırılacağını öğrenin. Apache Spark, bellek içi işleme kullanarak hızlı veri analizi ve küme hesaplama sağlar. HDInsight’ta Spark hakkında daha fazla bilgi için bkz. [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md).

Bu hızlı başlangıçta, HDInsight Spark kümesi oluşturmak için Kaynak Yöneticisi şablonu kullanırsınız. Küme, küme depolama alanı olarak Azure Depolama Bloblarını kullanır.

> [!IMPORTANT]
> İster kullanın, ister kullanmayın, HDInsight kümeleri faturalaması dakika başına eşit olarak dağıtılmıştır. Kullanmayı bitirdikten sonra kümenizi sildiğinizden emin olun. Daha fazla bilgi için bu makalenin [Kaynakları temizleme](#clean-up-resources) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesi oluşturma

Azure Resource Manager şablonu kullanarak bir HDInsight Spark kümesi oluşturun. Şablon, [github](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/)’da bulunabilir. 

1. Azure portalındaki şablonu yeni bir tarayıcı sekmesinde açmak için aşağıdaki bağlantıyı seçin:         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank">Azure’a Dağıtma</a>

2. Aşağıdaki değerleri girin:

    | Özellik | Değer |
    |---|---|
    |**Abonelik**|Bu kümeyi oluşturmak için kullanılan Azure aboneliğinizi seçin. Bu hızlı başlangıç için kullanılan abonelik, **&lt;Azure abonelik adı>** şeklindedir. |
    | **Kaynak grubu**|Kaynak grubu oluşturun veya var olan bir grubu seçin. Kaynak grubu, projelerinize ait Azure kaynaklarını yönetmek için kullanılır. Bu hızlı başlangıç için kullanılan yeni kaynak grubu adı, **myspark20180403rg** şeklindedir.|
    | **Konum**|Kaynak grubu için bir konum seçin. Şablon hem kümeyi oluşturmak için hem de varsayılan küme depolaması için bu konumu kullanır. Bu hızlı başlangıç için kullanılan konum, **Doğu ABD 2**’dir.|
    | **ClusterName**|Oluşturmak istediğiniz HDInsight kümesi için bir ad girin. Bu hızlı başlangıç için kullanılan yeni küme adı, **myspark20180403** şeklindedir.|
    | **Küme oturum açma adı ve parolası**|Varsayılan oturum açma adı, admin’dir. Kümede oturum açmak için bir parola seçin. Bu hızlı başlangıç için kullanılan oturum açma adı, admin’dir.|
    | **SSH kullanıcı adı ve parolası**|SSH kullanıcısı için bir parola seçin. Bu hızlı başlangıç için kullanılan SSH kullanıcı adı, **sshuser** şeklindedir.|

    ![Azure Resource Manager şablonu kullanarak HDInsight Spark kümesi oluşturma](./media/apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Azure Resource Manager şablonu kullanarak Spark kümesi oluşturma")

3. **Yukarıdaki hüküm ve koşulları kabul ediyorum**’u ve ardından **Panoya sabitle**’yi seçip **Satın al**’a tıklayın. Artık **Şablon Dağıtımını dağıtma** başlıklı yeni bir kutucuk görebilirsiniz. Kümenin oluşturulması yaklaşık 20 dakika sürer. Sonraki oturumuna devam etmeden önce küme oluşturulması gerekir.

HDInsight kümelerini oluştururken sorunlarla karşılaşırsanız, bunu yapmak için doğru izinlere sahip olmayabilirsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="install-intellijeclipse-for-spark-application"></a>Spark uygulaması için IntelliJ/Eclipse’i yükleme
Scala’da yazılmış Spark uygulamaları geliştirmek için Azure Toolkit for IntelliJ/Eclipse eklentisini kullanın ve sonra bunları doğrudan IntelliJ/Eclipse tümleşik geliştirme ortamından (IDE) Azure HDInsight Spark kümesine gönderin. Daha fazla bilgi için bkz. [Spark uygulaması yazmak/göndermek için IntelliJ kullanma](./apache-spark-intellij-tool-plugin.md) ve [Spark uygulaması yazmak/göndermek için Eclipse kullanma](./apache-spark-eclipse-tool-plugin.md).

## <a name="install-vscode-for-pysparkhive-applications"></a>PySpark/hive uygulamaları için VSCode yükleme
Hive toplu işleri, etkileşimli Hive sorguları, PySpark toplu işi ve PySpark etkileşimli betikleri oluşturup göndermek için Visual Studio Code (VSCode) için Azure HDInsight Araçları’nın nasıl kullanılacağını öğrenin. Azure HDInsight Tools, VSCode tarafından desteklenen platformlara yüklenebilir. Bunlar arasında Windows, Linux ve MacOS yer alır. Daha fazla bilgi için bkz. [PySpark uygulaması yazmak/göndermek için VSCode kullanma](../hdinsight-for-vscode.md).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

Jupyter Notebook, çeşitli programlama dillerini destekleyen etkileşimli bir not defteri ortamıdır. Not defteri, verilerle etkileşim kurmanıza, kodu markdown metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak sağlar. 

1. [Azure portalı](https://portal.azure.com) açın.
2. **HDInsight kümeleri**’ni ve sonra oluşturduğunuz kümeyi seçin.

    ![Azure portalında HDInsight kümesini açma](./media/apache-spark-jupyter-spark-sql/azure-portal-open-hdinsight-cluster.png)

3. Portaldan **Küme panoları**’nı ve sonra **Jupyter Notebook**’u seçin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

   ![Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma")

4. **Yeni** > **PySpark** seçeneklerini belirleyerek bir not defteri oluşturun. 

   ![Jupyter Not Defteri’ni oluşturarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Jupyter Not Defteri’ni oluşturarak etkileşimli Spark SQL sorgusu çalıştırma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.


## <a name="run-spark-sql-statements"></a>Spark SQL deyimleri çalıştırma

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Çekirdeğin hazır olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![HDInsight Spark'ta Hive sorgusu](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "HDInsight Spark'ta Hive sorgusu")

    Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin. 
2. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```PySpark
    %%sql
    SHOW TABLES
    ```
    HDInsight Spark kümeniz için yapılandırılmış bir Jupyter not defteri kullanırken, Spark SQL ile Hive sorguları çalıştırmak için kullanabileceğiniz önceden ayarlanmış bir `sqlContext` alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `sqlContext` ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sonuçları almak 30 saniye kadar sürer. Çıkış aşağıdakine benzer olacaktır: 

    ![HDInsight Spark'ta Hive sorgusu](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "HDInsight Spark'ta Hive sorgusu")

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.
    
2. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```
    
    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight Spark'ta Hive sorgusu çıkışı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "HDInsight Spark'ta Hive sorgusu çıkışı")

2. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Not defterini kapatmak, küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları temizleme
HDInsight, verilerinizi Azure Depolama’da veya Azure Data Lake Store’da depolar, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. [Sonraki adımlar](#next-steps) içinde listelenen öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalına geri dönüp **Sil**’i seçin.

![HDInsight kümesini silme](./media/apache-spark-jupyter-spark-sql/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar 

Bu hızlı başlangıçta HDInsight Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. HDInsight Spark kümesini kullanarak örnek veriler üzerinde etkileşimli sorgular çalıştırma hakkında daha fazla bilgi edinmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Spark üzerinde etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)


